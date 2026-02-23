# MEAN Stack CRUD Application

A full-stack MEAN (MongoDB, Express, Angular, Node.js) application for managing tutorials with full CRUD operations.

## Project Overview

This is a DevOps task implementation that includes:
- **Backend**: Node.js + Express REST API on port 8080
- **Frontend**: Angular 15 application on port 80 (via Nginx)
- **Database**: MongoDB on port 27017
- **Reverse Proxy**: Nginx to route traffic to frontend and backend

## Features

- Create, Read, Update, Delete tutorials
- Search tutorials by title
- RESTful API architecture
- Docker containerization
- CI/CD with GitHub Actions

## Prerequisites

- Node.js 18+
- Docker & Docker Compose
- Git
- Docker Hub account
- Ubuntu VM (for deployment)

## Local Development Setup

### Backend

```
bash
cd backend
npm install
node server.js
```

The backend runs on http://localhost:8080

### Frontend

```
bash
cd frontend
npm install
ng serve --port 8081
```

The frontend runs on http://localhost:8081

## Docker Deployment

### Build and Run Locally

```
bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

### Access the Application

- Frontend: http://localhost
- Backend API: http://localhost/api/tutorials

## CI/CD Pipeline

The project includes GitHub Actions workflow for automated build and deployment.

### Required Secrets

Configure these secrets in your GitHub repository:

| Secret | Description |
|--------|-------------|
| DOCKERHUB_USERNAME | Your Docker Hub username |
| DOCKERHUB_TOKEN | Docker Hub access token |
| SSH_PRIVATE_KEY | Private key for VM access |
| VM_IP | IP address of your Ubuntu VM |
| VM_USER | Username for SSH access (e.g., ubuntu) |

### Pipeline Flow

1. Code push triggers GitHub Actions
2. Backend Docker image builds and pushes to Docker Hub
3. Frontend Docker image builds and pushes to Docker Hub
4. SSH into VM and pull latest images
5. Restart containers with docker-compose

## Project Structure

```
crud-dd-task-mean-app/
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   ├── server.js
│   └── app/
│       ├── config/db.config.js
│       ├── controllers/tutorial.controller.js
│       ├── models/tutorial.model.js
│       └── routes/turorial.routes.js
├── frontend/
│   ├── Dockerfile
│   ├── nginx.conf
│   ├── package.json
│   └── src/
├── docker-compose.yml
├── .github/
│   └── workflows/
│       └── deploy.yml
└── README.md
```

## Nginx Configuration

The Nginx reverse proxy is configured to:
- Serve the Angular frontend at root `/`
- Proxy API requests to `/api/` to the backend container
- Enable WebSocket support for better connectivity

## MongoDB Setup

The application uses MongoDB container with:
- Database name: `dd_db`
- Data persistence via Docker volume

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/tutorials | Get all tutorials |
| GET | /api/tutorials/:id | Get tutorial by ID |
| POST | /api/tutorials | Create new tutorial |
| PUT | /api/tutorials/:id | Update tutorial |
| DELETE | /api/tutorials/:id | Delete tutorial |
| DELETE | /api/tutorials | Delete all tutorials |
| GET | /api/tutorials?title=xxx | Find tutorials by title |

## Deployment to VM

### One-time VM Setup

```
bash
# SSH into your VM
ssh ubuntu@<VM_IP>

# Install Docker
sudo apt update
sudo apt install -y docker.io docker-compose

# Pull the project
git clone <REPO_URL>
cd crud-dd-task-mean-app

# Create necessary directories
mkdir -p mean-app
cp docker-compose.yml mean-app/

# Run the application
cd mean-app
docker-compose up -d
```

### Manual Deployment

```
bash
# On the VM
cd /home/mean-app
docker-compose pull
docker-compose up -d
```

## Troubleshooting

### Check Container Logs
```
bash
docker-compose logs -f [container_name]
```

### Restart Services
```
bash
docker-compose restart
```

### Rebuild Images
```
bash
docker-compose build --no-cache
```

## Screenshots

The following should be documented:
- CI/CD pipeline execution in GitHub Actions
- Docker images pushed to Docker Hub
- Application running on VM (accessing via browser)
- Nginx configuration

## License

ISC
