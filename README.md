# PodPilot 🚀

PodPilot is a scalable, production-ready Task Management application built with **Django** and deployed using a **Kubernetes** sidecar architecture. This project demonstrates modern DevOps practices, including containerization, automated migrations, and local cluster orchestration.

## 🏗️ Architecture
The application uses a **Sidecar Pattern**:
* **Django (Main Container):** Handles logic and API requests on port 8000.
* **Nginx (Sidecar):** Acts as a reverse proxy on port 80, serving static files and routing traffic.
* **PostgreSQL:** Persistent state managed via a `StatefulSet`.



## 🛠️ Tech Stack
* **Backend:** Python / Django
* **Database:** PostgreSQL
* **Infrastructure:** Kubernetes (k8s)
* **Containerization:** Docker
* **Local Env:** Docker Desktop / kubectl

## 🚀 Getting Started

### 1. Prerequisites
* Docker Desktop (with Kubernetes enabled)
* `kubectl` installed

### 2. Environment Setup
Create the dedicated namespace and set the context:
```bash
kubectl create namespace podpilot
kubectl config set-context --current --namespace=podpilot
```
### 3. Build & Load Image
Build the application image locally:
```bash
docker build -t task-manager:latest ./task_manager
```
### 4. Deploy Infrastructure
Apply secrets, configurations, and the database:

```bash
# Secrets & Configs
kubectl apply -f k8s/secrets/
kubectl apply -f k8s/base/nginx-configmap.yaml

# Database
kubectl apply -f k8s/base/postgres-service.yaml
kubectl apply -f k8s/base/postgres-statefulset.yaml

# Run Migrations
kubectl apply -f k8s/migrate-job.yaml
```
### 5. Launch Application
```bash
kubectl apply -f k8s/base/service.yaml
kubectl apply -f k8s/base/deployment.yaml
```
## 🔗 Accessing the App
To view the application on your local machine, use port-forwarding:

```bash
kubectl port-forward svc/task-manager 8080:80 -n podpilot
Visit: http://localhost:8080/api
```
