# MultienvApp

This project demonstrates the containerized deployment of a ticket management application consisting of:

- **Development Backend** (Flask)
- **Production Backend** (Flask)
- **Frontend UI** (React)

All services are containerized using **Docker** and orchestrated via **Docker Compose**, supporting local development and testing of multi-environment setups.

---

## 📁 Project Structure

```bash
MultienvApp-main/
├── docker-compose.yml
├── backend/
│   ├── dev/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env
│   └── prod/
│       ├── app.py
│       ├── requirements.txt
│       ├── Dockerfile
│       └── .env
└── frontend/
    ├── src/
    ├── public/
    ├── Dockerfile
    └── package.json
```

## Deployment Steps
### 1. Clone or Extract the Repository

```bash
git clone https://github.com/govind02420/Multienv_deployment.git
cd Multienv_deployment
```
### 2. Docker & Docker Compose Setup
   Install Docker
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
```
  Check Docker installation
```bash
docker --version
docker-compose --version
```
### 3. Docker Configuration
   Dockerfiles:
    - backend/dev/Dockerfile
    - backend/prod/Dockerfile
    - frontend/Dockerfile

* Each backend Dockerfile uses python:3.9-slim, installs requirements, and runs app.py.
* The frontend Dockerfile uses node:18, builds React, and runs on port 3000.

  docker-compose.yml:
    - Defines 3 services (backend-dev, backend-prod, frontend) and their ports + environment variables.

### 4. Build & Run with Docker Compose

```bash
docker-compose up --build
```
## Access URLs

| Service         | Port | URL                                                      |
| --------------- | ---- | -------------------------------------------------------- |
| Frontend UI     | 3000 | [http://localhost:3000](http://localhost:3000)           |
| Development API | 3001 | [http://localhost:3000/dev](http://localhost:3000/dev)   |
| Production API  | 3002 | [http://localhost:3000/prod](http://localhost:3000/prod) |

NOTE: Frontend internally calls dev/prod APIs using configured environment variables.

## Docker Overview
### Backend (Dev & Prod)
 1. Python 3.9-slim
 2. Flask App exposed on 3001 (dev) & 3002 (prod)
 3. Requirements auto-installed

### Frontend
 1. Node.js 18
 2. React UI served on port 3000

### Compose
```bash
services:
  backend-dev:
    build: ./backend/dev
    ports: ["3001:3001"]

  backend-prod:
    build: ./backend/prod
    ports: ["3002:3002"]

  frontend:
    build: ./frontend
    ports: ["3000:3000"]
    environment:
      - REACT_APP_DEV_URL=http://localhost:3001
      - REACT_APP_PROD_URL=http://localhost:3002
    depends_on:
      - backend-dev
      - backend-prod

```

## Testing Instructions
After running docker-compose up --build, test the following in your browser:

1. Frontend Dashboard
2. Development Endpoint
3. Production Endpoint

## Screenshots
 1. docker ps showing all 3 services running
 2. Browser access of frontend, /dev, and /prod
 3. docker-compose logs showing successful startup
