# 🌶️ PepperCRM - Local Development Environment

This repository contains the source code for the **PepperCRM** project, a modern and modular CRM solution designed to manage clients, sales, and business opportunities.

You can run this project locally using two approaches:

---

## 🚀 Execution Options

### ✅ Option 1: Run with Docker

For a quick and standardized setup using **Docker** and **Docker Compose**, follow the specific guide:

📁 [`docker/README.md`](docker/README.md)

This option is ideal for local development, testing, and staging environments.

---

### ☸️ Option 2: Run with Kubernetes

If you prefer working with **Kubernetes** for orchestration and scalability, you can check the detailed instructions at:

> ⚠️ **Warning:** Deployment with **Kubernetes** is still under development. Some features may not be available or fully configured.

You can check the preliminary instructions at:

📁 [`kubernetes/README.md`](kubernetes/README.md)

Recommended for production environments or advanced cloud deployments.

---

## 🧩 Project Structure

PepperCRM is composed of the following services:

| Service           | Description                                                                  |
|-------------------|------------------------------------------------------------------------------|
| **DB PepperCRM**  | MongoDB database used to store the CRM information.                          |
| **API PepperCRM** | Backend that exposes the system's logic and RESTful endpoints.               |
| **Web PepperCRM** | Frontend application that allows visual interaction with the system.         |
