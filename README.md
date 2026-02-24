architecture flow:
Browser
   ↓
Nginx (Port 80)
   ↓
Frontend (Angular)
   ↓
Backend (Node.js / Express)
   ↓
MongoDB (Docker)


MEAN-DEVOPS/
│
├── backend/
│   ├── Dockerfile
│   └── (Node.js source code)
│
├── frontend/
│   ├── Dockerfile
│   └── (Angular source code)
│
│
├── docker-compose.yaml
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── screenshots/
│
└── README.md

Docker Compose Setup
    -Services defined in docker-compose.yaml:
    -mongodb
    -backend
    -frontend
    -nginx

    -EC2 Deployment Details
    -OS: Ubuntu 24.04 (AWS EC2)
    -Docker Engine (Official)
    -Docker Compose v2
    -Security Group: Port 80 open


Nginx Reverse Proxy Configuration

Located in:
nginx/default.conf which is stored in ubuntu vm(ec2) inside /app folder
Configuration:

server {
    listen 80;

    location /api/ {
        proxy_pass http://backend:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        proxy_pass http://frontend;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

Entire application accessible via Port 80


Workflow Location for github actions
 .github/workflows/deploy.yml

Pipeline runs automatically on:
-push to main or develop branch

Pipeline Steps

-Checkout repository
-Login to Docker Hub
-Build Backend Docker image
-Build Frontend Docker image
-Push images to Docker Hub
-SSH into EC2 server
-Pull latest Docker images
-Restart containers using Docker Compose

Application can be accessed using ec2 public-ip URL:
http://http://51.21.248.19

note:
