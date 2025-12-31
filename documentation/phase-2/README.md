# Phase 2: Docker Multi-Stage Builds

## 🎯 Objective
Create optimized Docker images for the **Django backend** and **React frontend** using **multi-stage Docker builds**, ensuring production-ready containers.

---

## ✅ What Was Accomplished

### Backend Dockerization
- Created a multi-stage Dockerfile using **Python 3.11-slim**
- Implemented dependency isolation using builder stage
- Added **non-root user** for security
- Configured **Gunicorn** as the WSGI server
- Added **health check endpoint**
- Built and tested backend container locally
- Pushed image to Docker Hub

### Frontend Dockerization
- Created multi-stage Dockerfile using **Node.js 18** and **NGINX**
- Built optimized React production bundle
- Configured NGINX to serve static files
- Added health endpoint for container checks
- Built and tested frontend container locally
- Pushed image to Docker Hub

---

## 📦 Docker Images Created

| Image Name | Tag | Purpose |
|----------|------|---------|
| expense-tracker-backend | v1 | Django REST API |
| expense-tracker-frontend | v1 | React frontend (NGINX) |

---

## 🖼️ Screenshots

### Backend Build Success
📸 `phase2_backend_build_success.png`  
*Backend Docker image built successfully*

### Frontend Build Success
📸 `phase2_frontend_build_success.png`  
*Frontend Docker image built successfully*

### Docker Hub Login
📸 `phase2_dockerhub_login.png`  
*Authenticated with Docker Hub*

### Docker Hub Push
📸 `phase2_dockerhub_push_success.png`  
*Images successfully pushed to Docker Hub*

---

## 🔧 Key Configuration Files

### Backend Dockerfile (Highlights)
```dockerfile
FROM python:3.11-slim AS builder
# Install dependencies and build environment

FROM python:3.11-slim
# Copy app from builder stage
# Run as non-root user
# Expose port and define CMD
Frontend Dockerfile (Highlights)
FROM node:18-alpine AS builder
# Build React app

FROM nginx:alpine
# Serve build using NGINX
🧪 Testing Summary
Backend Container
docker run -d -p 8000:8000 expense-tracker-backend:v1
curl http://localhost:8000/health


✅ Health endpoint responded successfully
✅ API accessible

Frontend Container
docker run -d -p 3000:80 expense-tracker-frontend:v1


✅ React app loads in browser
✅ Static assets served correctly

🚀 Docker Commands Used
Build Images
docker build -t <docker-username>/expense-tracker-backend:v1 ./backend
docker build -t <docker-username>/expense-tracker-frontend:v1 ./frontend

Push Images
docker login
docker push <docker-username>/expense-tracker-backend:v1
docker push <docker-username>/expense-tracker-frontend:v1

📌 Summary

✔ Multi-stage Docker builds implemented
✔ Secure, optimized containers created
✔ Images successfully pushed to Docker Hub
✔ Ready for Kubernetes deployment

🚀 Next Phase

Phase 3: Kubernetes Deployment (Manifests / Helm / ArgoCD)

📅 Phase Completed: Phase 2 – Dockerization