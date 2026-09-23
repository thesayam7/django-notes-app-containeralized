# Django Notes App - Docker Skills Showcase

A production-ready multi-container Docker application demonstrating advanced containerization, orchestration, and DevOps practices.

---

## 🐳 What We Containerized

This Django Notes application containerizes a complete microservices architecture using **Docker Compose** to orchestrate three services: a **Django REST API backend** with **Gunicorn**, a **MySQL 8.0 database**, and an **Nginx reverse proxy**. All services communicate via a custom bridge network with persistent storage, health checks, and automated database migrations.

---

## 📦 Technology Stack

| Component | Version | Purpose | Container Name |
|-----------|---------|---------|-----------------|
| **Docker** | 20.10+ | Container runtime & orchestration | — |
| **Docker Compose** | 2.0+ | Multi-container orchestration | — |
| **Python** | 3.9 | Django backend runtime | `django_cont` |
| **Django** | Latest | Web framework & REST API | `django_cont` |
| **Gunicorn** | Latest | Production WSGI server | `django_cont` |
| **Nginx** | 1.23.3-alpine | Reverse proxy & load balancer | `nginx_cont` |
| **MySQL** | 8.0 | Relational database | `db_cont` |
| **Node.js** | 8 | Frontend (optional) | — |

---

## 🚀 Quick Start Guide

### Step 1: Clone the Repository

```bash
git clone https://github.com/LondheShubham153/django-notes-app.git
cd django-notes-app
```

### Step 2: Create Environment Configuration

```bash
cp .env.example .env
```

### Step 3: Start All Services

```bash
docker compose up 
```

### Step 4: Access the Application

- **Frontend (Nginx)**: http://localhost  
- **Django API**: http://localhost:8000  
- **Health Status**: `docker compose ps`

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                    Client Browser                    │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP (Port 80)
                       ▼
          ┌────────────────────────────┐
          │   Nginx (Alpine 1.23.3)    │
          │   Reverse Proxy/LB         │
          │   Container: nginx_cont    │
          └────────────┬───────────────┘
                       │ Internal Network
                       ▼
          ┌────────────────────────────┐
          │   Django + Gunicorn        │
          │   Python 3.9               │
          │   Container: django_cont   │
          │   Port: 8000               │
          └────────────┬───────────────┘
                       │ Internal Network
                       ▼
          ┌────────────────────────────┐
          │   MySQL 8.0 Database       │
          │   Container: db_cont       │
          │   Volume: mysql_data       │
          └────────────────────────────┘

Network: notes-app (custom bridge)
All containers can communicate by name: django_app, db, nginx
```

---

## 🔧 Docker Skills Demonstrated

### 1. **Multi-Container Orchestration**
### 2. **Containerization Best Practices**
### 3. **Health Checks & Monitoring**
### 4. **Environment & Configuration Management**
### 5. **Networking & Port Management**
### 6. **Database Containerization**
### 7. **Production Deployment**
---

## 📋 Key Features

| Feature | Implementation | Benefit |
|---------|-----------------|---------|
| **Service Orchestration** | Docker Compose | Easy multi-container management |
| **Custom Networking** | Bridge network `notes-app` | Secure inter-service communication |
| **Data Persistence** | Named volume `mysql_data` | Database survives container restarts |
| **Health Checks** | Application + database probes | Automated dependency management |
| **Environment Config** | `.env` + inline variables | Flexible configuration for dev/prod |
| **Reverse Proxy** | Nginx with upstream | Load balancing and SSL termination ready |
| **Database Migrations** | Automatic on startup | Zero-downtime deployments |
| **Image Optimization** | `.dockerignore` | Faster builds, smaller images |
| **Restart Policy** | `restart: always` | High availability |
| **Container Visibility** | Logging and exec support | Easy debugging and troubleshooting |

---

## 🛠️ Common Commands

```bash
# Start all services
docker compose up -d

# View running services
docker compose ps

# View logs
docker compose logs -f django_app
docker compose logs -f db

# Stop all services
docker compose down

# Stop and remove volumes
docker compose down -v

# Execute command in container
docker compose exec django_app python manage.py shell

# Access MySQL
docker compose exec db mysql -uroot -proot test_db

# Rebuild services
docker compose build --no-cache

# Inspect network
docker network inspect django-notes-app_notes-app
```

---

