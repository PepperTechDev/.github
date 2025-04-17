# 📦 **Local Demonstration of PepperCRM with Docker**

This guide explains how to run **PepperCRM** locally using Docker and Docker Compose. It includes prerequisites, quick and detailed setup instructions, and a complete description of the infrastructure defined in the `docker-compose.yml` file.

---

## ✅ **Prerequisites**

Before getting started, make sure you have the following installed:

- **Docker**  
  👉 [Official installation guide](https://docs.docker.com/get-docker/)
  
- **Docker Compose**  
  👉 [Official installation guide](https://docs.docker.com/compose/install/)  
  > If you're using Docker Desktop, Docker Compose is already included.

---

## 🚀 **How to Run the Project**

> ⚠️ **Note:** Make sure Docker is running before executing any commands.

### 🔹 **Option 1: Quick Run (One-liner)**

This method is ideal if you want to bring everything up in a single step from the terminal:

```bash
curl -L https://raw.githubusercontent.com/PepperTechDev/.github/main/docker/docker-compose.yml -o docker-compose.yml && docker-compose -p peppercrm up -d
```

🔧 This command:
- Automatically downloads the `docker-compose.yml` file
- Starts the containers in detached mode using the project name `peppercrm`

📍 Available services:

- 🔗 **MongoDB**: `mongodb://admin:admin@localhost:27018/peppercrm/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongo`
- 🔗 **API**: `http://localhost:8091`
- 🔗 **Web Frontend**: `http://localhost:5173`

🛑 To stop the services:

```bash
docker-compose -p peppercrm down
```

---

### 🔹 **Option 2: Step-by-Step Execution**

Ideal if you prefer more control over each step.

#### 📥 Step 1: Download the `docker-compose.yml` file

```bash
curl -L https://raw.githubusercontent.com/peppercrmTech/.github/main/docker-compose.yml -o docker-compose.yml
```

#### 🧱 Step 2: Pull Docker images

```bash
docker pull sebastian190030/db-peppercrm:latest
docker pull sebastian190030/api-peppercrm:latest
docker pull sebastian190030/web-peppercrm:latest
docker pull sebastian190030/cache-peppercrm:latest
```

#### 🚀 Step 3: Start the containers

```bash
docker-compose -p peppercrm up -d
```

#### 🌐 Step 4: Access the services

- 🗃️ **MongoDB**: `mongodb://admin:admin@localhost:27018/peppercrm/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongo`
- 🔗 **Redis**: `redis://localhost:6379`
- 🛠️ **API**: `http://localhost:8091`
- 🌍 **Web**: `http://localhost:5173`

#### 🛑 Step 5: Stop the services

```bash
docker-compose -p peppercrm down
```

---

## 🗂️ **Project Structure**

The `docker-compose.yml` file orchestrates the following services:

| Service            | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| **MongoDB**         | Database that stores all application data.                        |
| **Redis (Cache)**   | In-memory key-value store for caching and session data.           |
| **API (Backend)**   | RESTful service exposing the system's business logic.             |
| **Web (Frontend)**  | User interface that consumes the API services.                    |


---

## 📄 **docker-compose.yml Contents**

### 🔸 1. **MongoDB**
```yaml
db-peppercrm:
  image: sebastian190030/db-peppercrm:latest
  container_name: peppercrm-db
  ports:
    - "27018:27017"
  volumes:
    - mongo_data:/data/db
  networks:
    - peppercrm-network
```
- 📦 Persistent volume: `mongo_data`
- 🌐 Exposed port: `27018`

---

### 🔸 2. **API Backend**
```yaml
  api-peppercrm:
    image: sebastian190030/api-peppercrm:latest
    container_name: peppercrm-api
    environment:
      - APP_NAME=PepperCRM-API
      - PORT=8091
      - TITLE=peppercrm API
      - DESCRIPTION=Documentación de la API REST de PepperCRM
      - VERSION=1.0.0
      - AUTHOR=peppercrm Developers
      - DATA_CONNECTION_METHOD=mongodb
      - DATA_SOURCE_USERNAME=admin
      - DATA_SOURCE_PASSWORD=admin
      - DATA_SOURCE_DOMAIN=db-peppercrm:27017
      - DATA_SOURCE_DB=peppercrm
      - DATA_PARAMS=authSource=admin&directConnection=true&serverSelectionTimeoutMS=100000&socketTimeoutMS=10000&appName=mongo
      - CACHE_TYPE=redis
      - CACHE_HOST=cache-peppercrm
      - CACHE_PORT=6379
      - CACHE_DB=0
      - CACHE_USERNAME=default
      - CACHE_PASSWORD=redislocalpass
      - CACHE_TIMEOUT=2000
      - CACHE_LETTUCE_POOL_MAX_ACTIVE=8
      - CACHE_LETTUCE_POOL_MAX_WAIT=-1
      - CACHE_LETTUCE_POOL_MAX_IDLE=8
      - CACHE_LETTUCE_POOL_MIN_IDLE=8
      - CACHE_TIME_TO_LIVE=300000
      - CACHE_NULL_VALUES=false
      - SECURITY_JWT_SECRET_KEY=c8e9b6803afbcfa6edd9569c94c75ff4b144622b0a0570a636dffd62c24a3476
      - SECURITY_JWT_EXPIRATION=86400000
      - SECURITY_PUBLIC_ROUTES=/auth/login,/auth/verify
      - HEADER_CORS_ALLOWED_ORIGINS=http://localhost:5173
      - SERVER_TOMCAT_TIMEOUT=600000
      - DEBUGGER_MODE=INFO
    ports:
      - "8091:8091"
    depends_on:
      - db-peppercrm
      - cache-peppercrm
    networks:
      - peppercrm-network
```
- 🔐 Configures JWT and CORS
- 🧱 Ensures MongoDB is ready before launching the API (`depends_on`)

---

### 🔸 3. **Web Frontend**
```yaml
web-peppercrm:
  image: sebastian190030/web-peppercrm:latest
  container_name: peppercrm-web
  environment:
    - VITE_API_BASE_URL=http://localhost:8091
  ports:
    - "5173:5173"
  networks:
    - peppercrm-network
```
- ⚙️ `VITE_API_BASE_URL`: Base URL for API calls
- 🌐 Accessible from browser: `http://localhost:5173`

---

### 🔸 4. **Redis Cache**
```yaml
cache-peppercrm:
  image: cache-peppercrm
  container_name: peppercrm-cache
  ports:
    - "6379:6379"
  networks:
    - peppercrm-network
```
- ⚡ Exposes Redis on the default port `6379`
- 🧠 Used by the API for caching and fast data access

---


### 🔸 5. **Volumes**
```yaml
volumes:
  mongo_data:
```
Used by MongoDB for persistent storage.

---

### 🔸 6. **Networks**
```yaml
networks:
  peppercrm-network:
    driver: bridge
```
Allows communication between internal services in the application.

---

## 🧾 **Access Summary**

| Service        | Access URL                                             |
|----------------|--------------------------------------------------------|
| MongoDB        | `mongodb://admin:admin@localhost:27018/peppercrm/...` |
| API Backend    | `http://localhost:8091`                                |
| Web Frontend   | `http://localhost:5173`                                |
