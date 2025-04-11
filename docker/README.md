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
```

#### 🚀 Step 3: Start the containers

```bash
docker-compose -p peppercrm up -d
```

#### 🌐 Step 4: Access the services

- 🗃️ **MongoDB**: `mongodb://admin:admin@localhost:27018/peppercrm/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongo`
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
    - APP_NAME=peppercrm-API
    - PORT=8091
    - DATA_SOURCE_DOMAIN=db-peppercrm:27017
    - DATA_SOURCE_USERNAME=admin
    - DATA_SOURCE_PASSWORD=admin
    - DATA_SOURCE_DB=peppercrm
    - SECURITY_JWT_SECRET_KEY=c8e9b6803afbcfa6edd9569c94c75ff4b144622b0a0570a636dffd62c24a3476
    - SECURITY_JWT_EXPIRATION=86400000
    - HEADER_CORS_ALLOWED_ORIGINS=http://localhost:5173
    # ...other variables omitted for brevity
  ports:
    - "8091:8091"
  depends_on:
    - db-peppercrm
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

### 🔸 4. **Volumes**
```yaml
volumes:
  mongo_data:
```
Used by MongoDB for persistent storage.

---

### 🔸 5. **Networks**
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
