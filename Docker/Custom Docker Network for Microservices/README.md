# 📘 Lab 15 – Docker Microservices with Custom Network

## 🧾 Overview

This project demonstrates how to containerize a simple microservices architecture using Docker.
It consists of two services:

- Backend: Python Flask API

- Frontend: Python Flask web app that communicates with the backend

All containers run inside a custom Docker bridge network.

## 🐳 Backend Dockerfile

```
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

## 🖥️ Frontend Dockerfile

```
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

## 🔗 Docker Networking

### 🛠️ Create custom network

```
docker network create ivolve-network
```

### 🚀 Run Containers

```
# Run backend container
docker build -t custom-backend ./backend
docker run -d --name backend --network ivolve-network custom-backend

# Run frontend1 on same network
docker build -t custom-frontend ./frontend
docker run -d --name frontend1 --network ivolve-network custom-frontend

# Run frontend2 on default bridge (for testing isolation)
docker run -d --name frontend2 custom-frontend
```

## 🧪 Communication Testing

### ✅ Successful communication:

```
docker exec -it frontend1 curl http://backend:5000
```
Oytput:
```
Hello from Backend!
```

### ❌ Communication failure (isolated network):

```
docker exec -it frontend2 curl http://backend:5000
```
Oytput:

```
Could not resolve host: backend
```

## 🧼 Cleanup

```
docker stop backend frontend1 frontend2
docker rm backend frontend1 frontend2
docker network rm ivolve-network
```

