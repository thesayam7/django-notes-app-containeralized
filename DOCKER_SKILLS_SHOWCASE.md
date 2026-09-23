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
docker compose up --pull always
```

*This command pulls the latest base images, builds all containers, and starts them in the correct order.*

### Step 4: Access the Application

- **Frontend (Nginx)**: http://localhost  
- **Django API**: http://localhost:8000  
- **Health Status**: `docker compose ps`

---

## 📚 What We Learned

### ✅ **Multi-Container Architecture**
- Decomposing a monolithic application into separate containerized services
- Each service runs in its own container with a single responsibility
- Services communicate securely via custom Docker networks

### ✅ **Docker Compose Orchestration**
- Defining multiple services in a single `docker-compose.yml` file
- Service dependencies and conditional startup (`depends_on` with health checks)
- Automatic DNS resolution between services using container names
- Network isolation and custom bridge networks for service communication

### ✅ **Database Persistence & Initialization**
- Named volumes (`mysql_data`) for MySQL data persistence across container restarts
- Automatic database schema creation and migrations during container startup
- Health checks to verify database readiness before dependent services start

### ✅ **Environment & Configuration Management**
- Multi-source environment configuration (inline `environment` + `env_file`)
- Using `.env` files for local development without hardcoding secrets
- Runtime environment variable injection into containers

### ✅ **Health Checks & Service Dependencies**
- Application-level health monitoring with `healthcheck` directives
- TCP/command-based health verification (HTTP for Django, MySQL ping for database)
- Conditional service startup (`condition: service_healthy`)
- Graceful startup delays with `start_period`

### ✅ **Reverse Proxy & Load Balancing**
- Nginx configuration for reverse proxy and upstream service routing
- HTTP header forwarding for client IP, protocol, and host information
- Container DNS resolution in upstream configurations
- Port exposure strategy and external accessibility

### ✅ **Image Optimization**
- `.dockerignore` to exclude unnecessary files from build context
- Reduced build time and image size by excluding: `.git`, `__pycache__`, `node_modules`, `.env`, etc.
- Security best practices: preventing sensitive files from being baked into images

### ✅ **Production Readiness**
- Automatic container restart policies (`restart: always`)
- Production WSGI server (Gunicorn) instead of Django development server
- Lightweight Alpine Linux base images (Nginx) for minimal footprint
- Proper logging and container visibility with Docker Compose

### ✅ **CI/CD Integration**
- Jenkins pipeline for automated Docker image building and testing
- Docker registry authentication and image pushing to Docker Hub
- Container image tagging and versioning strategies
- Automated deployment workflow in DevOps pipelines

### ✅ **Networking & Port Management**
- Custom bridge network (`notes-app`) for inter-container communication
- Service-to-service DNS resolution using container names
- Port mapping strategies for external access (80 → Nginx, 8000 → Django)
- Isolation of database service (no external port exposure)

### ✅ **Container Lifecycle Management**
- `docker compose up` to start, build, and pull images
- `docker compose down` for graceful shutdown
- `docker compose logs` for debugging and monitoring
- `docker compose exec` for running commands in containers

### ✅ **Django Containerization**
- Dockerfile with proper Python environment setup
- System dependency installation (MySQL client, build tools)
- Application dependency management with `pip` and `requirements.txt`
- Database migration execution before application startup
- Static file serving in production with WhiteNoise

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
- Docker Compose service definition and lifecycle management
- Service dependency ordering and health-based startup
- Named volume creation and persistence strategies
- Custom network configuration for service isolation

### 2. **Containerization Best Practices**
- Dockerfile optimization with proper base images
- Layer caching strategies and build efficiency
- `.dockerignore` for build context optimization
- Security considerations (non-root users, secret exclusion)

### 3. **Health Checks & Monitoring**
- Application-level health detection (HTTP, TCP, command-based)
- Conditional service startup based on dependency health
- Container lifecycle monitoring with Docker Compose

### 4. **Environment & Configuration Management**
- Multi-source environment variable injection
- `.env` file support for local development
- Separation of concerns (build vs. runtime configuration)

### 5. **Networking & Port Management**
- Custom bridge networks for service communication
- DNS-based service discovery
- Port mapping and exposure strategies
- Reverse proxy configuration for routing

### 6. **Database Containerization**
- MySQL containerization with persistence
- Automatic initialization and schema creation
- Health checks for database readiness
- Named volumes for data durability

### 7. **Production Deployment**
- Gunicorn WSGI server configuration
- Automatic restart policies for high availability
- Proper logging and observability
- CI/CD integration with Jenkins

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

## 📈 Learning Path

1. **Understand containerization**: Read the Dockerfile, understand base images, layers, and optimization
2. **Learn Docker Compose**: Study `docker-compose.yml` service definitions, dependencies, and networks
3. **Explore networking**: Use `docker network inspect` to understand service communication
4. **Debug containers**: Use `docker compose logs` and `docker compose exec` to troubleshoot
5. **Practice deployment**: Deploy locally with `docker compose up --pull always`
6. **Enhance security**: Add resource limits, secret management, and proper restart policies

---

## 🔒 Security Considerations

**Currently Implemented:**
- Environment variable management (avoiding hardcoded secrets)
- `.dockerignore` for secret exclusion (.env file not baked into image)
- Custom bridge network for service isolation
- Non-root considerations in Alpine Nginx image

**Recommendations for Production:**
- Use Docker Secrets or HashiCorp Vault for sensitive data
- Implement read-only root filesystems where possible
- Add resource limits (memory, CPU) to prevent resource exhaustion
- Scan images for vulnerabilities with `docker scout`
- Enable Docker Content Trust for image signing
- Implement centralized logging (ELK, Splunk)
- Use secrets management (AWS Secrets Manager, Azure Key Vault)

---

## 📁 Project Structure

```
django-notes-app/
├── Dockerfile                    # Django backend image definition
├── docker-compose.yml            # Multi-container orchestration config
├── .dockerignore                 # Build optimization rules
├── .env.example                  # Environment template
├── Jenkinsfile                   # CI/CD pipeline definition
├── requirements.txt              # Python dependencies
├── manage.py                     # Django CLI
├── notesapp/                     # Django application code
├── api/                          # REST API endpoints
├── mynotes/                      # Frontend code (Node.js optional)
│   └── Dockerfile
├── nginx/                        # Reverse proxy
│   ├── Dockerfile
│   └── default.conf
└── staticfiles/                  # Static assets
```

---

## 📚 Resources & References

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Django in Docker](https://docs.docker.com/samples/django/)
- [Nginx in Docker](https://hub.docker.com/_/nginx)
- [MySQL in Docker](https://hub.docker.com/_/mysql)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

---

## 🎯 Next Steps for Enhancement

1. **Multi-Stage Builds**: Reduce backend image size with build and runtime stages
2. **Docker Secrets**: Implement secure credential management
3. **Resource Limits**: Add CPU and memory constraints to services
4. **Centralized Logging**: Implement ELK or Splunk for log aggregation
5. **Metrics & Monitoring**: Add Prometheus + Grafana for observability
6. **Automated Testing**: Integrate unit and integration tests in CI/CD
7. **Private Registry**: Push images to Docker Hub or private registry
8. **Kubernetes Migration**: Scale to Kubernetes for advanced orchestration
9. **SSL/TLS**: Add HTTPS with Let's Encrypt in Nginx
10. **Development Tools**: Use Docker Dev Environments for team consistency

---

**Created**: 2024 | **Status**: Production-Ready ✅
