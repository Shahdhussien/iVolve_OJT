# Lab 13: Docker Environment Variables

## ✅ 1. Clone the App Code

```
git clone https://github.com/Ibrahim-Adel15/Docker-3.git
cd Docker-3
```

## ✅ 2. Write Dockerfile

```
FROM python:3.9-slim

# Set default env vars 
ENV APP_MODE=production
ENV APP_REGION=canada-west

WORKDIR /app
COPY app.py .
RUN pip install flask
EXPOSE 8080
CMD ["python", "app.py"]
```

## ✅ 3. Build the Docker Image

```
docker build -t env-app .
```

## 🔁 Method 1: Pass Environment Variables in Docker Run Command

```
docker run -d --name dev-app \
  -e APP_MODE=development \
  -e APP_REGION=us-east \
  -p 8080:8080 \
  env-app
```

### 🔍 Test:

```
curl http://localhost:8080
```
Output:
```
Mode: development, Region: us-east
```

## 📄 Method 2: Pass Variables from .env File

### 1. Create staging.env

```
APP_MODE=staging
APP_REGION=us-west
```
### 2. Run container with the file

```
docker run -d --name staging-app \
  --env-file staging.env \
  -p 8081:8080 \
  env-app
```
#### 🔍 Test:
```
curl http://localhost:8081
```
Output:
```
Mode: staging, Region: us-west
```

## 🔒 Method 3: Set Variables Inside Dockerfile

The environment variables are already set in the Dockerfile using ENV.

#### Run the container without passing any variables:
```
docker run -d --name prod-app -p 8082:8080 env-app
```
#### 🔍 Test:
```
curl http://localhost:8082
```
Output:
```
Mode: production, Region: canada-west
```

## 🧼 Clean Up

```
docker stop dev-app staging-app prod-app
docker rm dev-app staging-app prod-app
```

