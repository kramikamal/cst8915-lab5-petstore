# CST8915 Lab 5: Containerizing the Algonquin Pet Store with Docker

- **Student Name**: Kamal Krami
- **Student ID**: 041273436
- **Course**: CST8915 Full-stack Cloud-native Development
- **Semester**: Fall 2026

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/MLGwCUtl3jY)

---

## Service Repositories

These repositories were imported from the course repositories, and each one contains my Dockerfile.

| Service | Repository |
| --- | --- |
| Order Service (Node.js) | https://github.com/kramikamal/order-service-L4 |
| Product Service (Python) | https://github.com/kramikamal/product-service-python-L4 |
| Store Front (Vue.js + Nginx) | https://github.com/kramikamal/store-front-L4 |

---

## Docker Hub Images

| Image | Link |
| --- | --- |
| order-service | https://hub.docker.com/r/kamalkrami/order-service |
| product-service | https://hub.docker.com/r/kamalkrami/product-service |
| store-front | https://hub.docker.com/r/kamalkrami/store-front |

---

## Deployment Details

- **Product Service implementation:** Python (Flask)
- **API host embedded in the Store Front image:** `4.223.121.68`
  - Order Service URL: `http://4.223.121.68:3000`
  - Product Service URL: `http://4.223.121.68:3030`
- **Environment:** Azure VM (Ubuntu 24.04, Standard B2s) with Docker Engine and the Docker Compose plugin

The API addresses are added to the Store Front during the build, because the browser calls the APIs directly. If the stack runs on a different host, the Store Front image must be rebuilt with the new IP address.

---

## How to Run

1. Copy `.env.example` to `.env` and choose a password made of letters and numbers.
2. Run the stack:

   ```bash
   docker compose config --quiet
   docker compose pull
   docker compose up -d --wait
   ```

3. Open `http://<API_HOST>:8080` in a browser.

---

## Challenges and Lessons Learned

- **Docker permission error:** After adding my user to the `docker` group, I got a "permission denied" error in a new terminal. I learned that `newgrp docker` only works in the current terminal, so the VS Code session had to be restarted for the change to apply everywhere.
- **Using the correct repositories:** At first, I cloned my old repositories from Labs 2 and 3. I noticed that they were missing files such as `.dockerignore` and `.env.example`, so I imported the L4 versions instead.
- **Build-time API addresses:** I learned that the Store Front needs the public IP of the VM during the build, not when the container starts, because Vue adds the API addresses to the website files.
- **Layer caching and volumes:** When I ran Docker Compose, the build took only a few seconds because all the layers were cached. Also, the queued orders were kept after `docker compose down`, because RabbitMQ stores its data in a named volume.