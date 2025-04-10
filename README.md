# 🌶️ PepperCRM - Entorno de Desarrollo Local

Este repositorio contiene el código fuente del proyecto **PepperCRM**, una solución CRM moderna y modular diseñada para gestionar clientes, ventas y oportunidades de negocio.

Puedes ejecutar este proyecto localmente utilizando dos enfoques:

---

## 🚀 Opciones de Ejecución

### ✅ Opción 1: Ejecución con Docker

Para una configuración rápida y estandarizada mediante **Docker** y **Docker Compose**, sigue la guía específica:

📁 [`docker/README.md`](docker/README.md)

Esta opción es ideal para desarrollo local, testing y entornos de staging.

---

### ☸️ Opción 2: Ejecución con Kubernetes

Si prefieres trabajar con **Kubernetes** para orquestación y escalabilidad, puedes consultar las instrucciones detalladas en:

> ⚠️ **Advertencia:** El despliegue con **Kubernetes** aún se encuentra en desarrollo. Algunas funcionalidades podrían no estar disponibles o completamente configuradas.

Puedes consultar las instrucciones preliminares en:

📁 [`kubernetes/README.md`](kubernetes/README.md)

Recomendada para entornos de producción o despliegues avanzados en la nube.

---

## 🧩 Estructura del Proyecto

PepperCRM se compone de los siguientes servicios:

| Servicio          | Descripción                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| **DB PepperCRM**  | Base de datos de mongoDB utilizada para almacenar la información del CRM.   |
| **API PepperCRM**  | Backend que expone la lógica del sistema y los endpoints RESTful.           |
| **Web PepperCRM**  | Aplicación frontend que permite la interacción visual con el sistema.       |
