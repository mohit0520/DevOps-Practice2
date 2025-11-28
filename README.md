
# 🚀 DevOps Project — Docker | Kubernetes | Helm | Jenkins | Prometheus | Grafana

![DevOps Pipeline](https://img.shields.io/badge/DevOps-Docker%20%7C%20Kubernetes%20%7C%20Jenkins-blue)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Automation-green)
![Monitoring](https://img.shields.io/badge/Monitoring-Prometheus%20%7C%20Grafana-orange)

---

## 📘 Project Overview

This project demonstrates a complete **DevOps workflow** — from source code to automated deployment and monitoring.

- Clone an existing project from GitHub  
- Containerize it using **Docker**  
- Create a local **Kubernetes cluster using Kind**  
- Deploy the application using **Kubernetes cluster**  
- Set up **Prometheus & Grafana** for monitoring using **using Helm Charts**  
- Implement a **Jenkins CI/CD pipeline** for automation  

---

## 🧩 Architecture Diagram

![WhatsApp Image 2025-11-03 at 8 25 04 PM](https://github.com/user-attachments/assets/f13edd4b-57ac-4265-b568-fa7f4fdd1c3e)

```mermaid

flowchart LR
    A[GitHub Repository] -->|Clone| B[Docker Build]
    B --> C[Docker Image - Local]
    C --> D[Kind Cluster]
    D -->|Deploy via| E[Helm Charts]
    E -->|Monitor| F[Prometheus]
    F -->|Visualize| G[Grafana]
    A -->|Trigger CI/CD| H[Jenkins Pipeline]
    H -->|Deploy Updates| D

```

---

## 🐳 Docker — Build & Run (step-by-step)

1. Clone project

```bash

sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
docker --version


```
2. Install Docker(Ubuntu)

```bash

sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
docker --version

```
3. Build Image

```bash

docker build -t <dockerhub-user>/<image-name>:v1 .

```
4. Run Container locally

```bash

docker run -d --name myapp -p 8080:8080 <dockerhub-user>/<image-name>:v1

```

## ☸️ Kind — Local Kubernetes cluster

1. Install Kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64
chmod +x ./kind
sudo mv kind /usr/local/bin/kind
kind --version
```
2. Create a cluster

```bash

kind create cluster --name dev-cluster
kubectl cluster-info --context kind-dev-cluster
kubectl get nodes
![WhatsApp Image 2025-11-03 at 8 27 49 PM (4)](https://github.com/user-attachments/assets/87b83f2d-bc98-4141-91e1-22f0a58b9a8d)

```

3. Load local Docker image into Kind
```bash

kind load docker-image <dockerhub-user>/<image-name>:v1 --name dev-cluster

```
![WhatsApp Image 2025-11-03 at 8 27 49 PM (4)](https://github.com/user-attachments/assets/78b5f7c4-aa80-460f-9773-af857a816de6)
![WhatsApp Image 2025-11-03 at 8 27 49 PM (3)](https://github.com/user-attachments/assets/5d751483-56ef-4034-bdfb-a929a098beab)
![WhatsApp Image 2025-11-03 at 8 27 49 PM (2)](https://github.com/user-attachments/assets/0de9fd62-ec63-4e79-aca2-817af607ba71)


## 🎩 Helm + Prometheus + Grafana (monitoring)

1. Install Helm & add repos
```bash

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

```
2. Install kube-prometheus-stack
```bash

kubectl create namespace monitoring
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring -f monitoring-values.yaml

```
4. Verify services
```bash
kubectl get svc -n monitoring
```

5. Access Prometheus & Grafana locally
```bash
kubectl port-forward svc/prometheus-operated 9090:9090 -n monitoring
# Open http://localhost:9090
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
# Open http://localhost:3000
```

![WhatsApp Image 2025-11-10 at 1 49 50 PM](https://github.com/user-attachments/assets/6695d492-8f7e-4c2c-9ff5-557e7bcfe00d)

## ⚙️ Jenkins — CI/CD pipeline


This project includes a fully automated CI/CD pipeline using Jenkins, which handles building, containerizing, and deploying the application.

🔧 What the Pipeline Does

Automatically triggers on every Git commit

Builds the application

Builds a Docker image

Deploys to Kubernetes (or server)

```mermaid

flowchart LR
A[Git Commit] --> B[Jenkins Build]
B --> C[Docker Build]
C --> D[Docker Push]
D --> E[Kubernetes Deploy]

```
