# 📦 **Demostración Local de PepperCRM con Docker**

Esta guía explica cómo ejecutar **PepperCRM** localmente usando Docker y Docker Compose. Incluye los requisitos, instrucciones rápidas y detalladas para levantar los servicios, además de una descripción completa de la infraestructura definida en el archivo `docker-compose.yml`.

---

## ✅ **Requisitos Previos**

Antes de comenzar, asegúrate de tener instalado lo siguiente:

- **Docker**  
  👉 [Guía de instalación oficial](https://docs.docker.com/get-docker/)
  
- **Docker Compose**  
  👉 [Guía de instalación oficial](https://docs.docker.com/compose/install/)  
  > Si estás usando Docker Desktop, Docker Compose ya viene incluido.

---

## 🚀 **Cómo Ejecutar el Proyecto**

> ⚠️ **Nota:** Asegúrate de que Docker esté corriendo antes de ejecutar cualquier comando.

### 🔹 **Opción 1: Ejecución Rápida (One-liner)**

Este método es ideal si quieres levantar todo en un solo paso desde la terminal:

```bash
curl -L https://raw.githubusercontent.com/PepperTechDev/.github/main/docker/docker-compose.yml -o docker-compose.yml && docker-compose -p peppercrm up -d
```

🔧 Este comando:
- Descarga automáticamente el archivo `docker-compose.yml`
- Inicia los contenedores en segundo plano con el proyecto `peppercrm`

📍 Servicios disponibles:

- 🔗 **MongoDB**: `mongodb://admin:admin@localhost:27018/peppercrm/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongo`
- 🔗 **API**: `http://localhost:8091`
- 🔗 **Frontend Web**: `http://localhost:5173`

🛑 Para detener los servicios:

```bash
docker-compose -p peppercrm down
```

---

### 🔹 **Opción 2: Ejecución Paso a Paso**

Ideal si prefieres tener más control sobre cada etapa.

#### 📥 Paso 1: Descargar el archivo `docker-compose.yml`

```bash
curl -L https://raw.githubusercontent.com/peppercrmTech/.github/main/docker-compose.yml -o docker-compose.yml
```

#### 🧱 Paso 2: Descargar las imágenes Docker

```bash
docker pull sebastian190030/db-peppercrm:latest
docker pull sebastian190030/api-peppercrm:latest
docker pull sebastian190030/web-peppercrm:latest
```

#### 🚀 Paso 3: Levantar los contenedores

```bash
docker-compose -p peppercrm up -d
```

#### 🌐 Paso 4: Acceder a los servicios

- 🗃️ **MongoDB**: `mongodb://admin:admin@localhost:27018/peppercrm/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongo`
- 🛠️ **API**: `http://localhost:8091`
- 🌍 **Web**: `http://localhost:5173`

#### 🛑 Paso 5: Detener los servicios

```bash
docker-compose -p peppercrm down
```

---

## 🗂️ **Estructura del Proyecto**

El archivo `docker-compose.yml` orquesta los siguientes servicios:

| Servicio          | Descripción                                                        |
|-------------------|--------------------------------------------------------------------|
| **MongoDB**        | Base de datos que almacena toda la información de la aplicación. |
| **API (Backend)**  | Servicio RESTful que expone la lógica del sistema.               |
| **Web (Frontend)** | Interfaz de usuario que consume los servicios de la API.         |

---

## 📄 **Contenido del docker-compose.yml**

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
- 📦 Volumen persistente: `mongo_data`
- 🌐 Puerto expuesto: `27018`

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
    # ...otras variables omitidas por brevedad
  ports:
    - "8091:8091"
  depends_on:
    - db-peppercrm
  networks:
    - peppercrm-network
```
- 🔐 Configura JWT y CORS
- 🧱 Se asegura que MongoDB esté listo antes de levantar la API (`depends_on`)

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
- ⚙️ `VITE_API_BASE_URL`: Base para llamadas a la API
- 🌐 Accesible desde el navegador: `http://localhost:5173`

---

### 🔸 4. **Volúmenes**
```yaml
volumes:
  mongo_data:
```
Usado por MongoDB para almacenamiento persistente.

---

### 🔸 5. **Redes**
```yaml
networks:
  peppercrm-network:
    driver: bridge
```
Permite la comunicación entre los servicios internos de la aplicación.

---

## 🧾 **Resumen de Accesos**

| Servicio        | URL de Acceso                          |
|-----------------|----------------------------------------|
| MongoDB         | `mongodb://admin:admin@localhost:27018/peppercrm/...` |
| API Backend     | `http://localhost:8091`                |
| Web Frontend    | `http://localhost:5173`                |
