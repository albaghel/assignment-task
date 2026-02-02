# Nebula Aurora Assignment Solution

This repository contains the complete solution for the User and Post API.

## Project Overview

We have successfully modernized the application:

*   **Containerization**: Built a lightweight Python 3.12-slim Docker image.
*   **Database**: Migrated to PostgreSQL with `asyncpg` for high-performance async access.
*   **Orchestration**: Deployed via Helm with strict limits (2 CPU, 4GB RAM).
*   **Observability**: Integrated Prometheus and Grafana for "Dashboard as Code".

## Key Files & Changes

*   **Application (`solution/app/`)**
    *   `Dockerfile`: Optimized multi-stage build using `python:3.12-slim`.
    *   `requirements.txt`: Pin dependencies including `asyncpg`.
    *   `app/database.py`: Dynamic `DATABASE_URL` for switching between SQLite and Postgres.

*   **Helm Chart (`solution/helm/`)**
    *   `Chart.yaml`: Declares dependencies (`postgresql`, `kube-prometheus-stack`).
    *   `values.yaml`: Configures resource limits, Ingress, and disables heavy components.
    *   `templates/deployment.yaml`: FastAPI deployment manifest.
    *   `templates/dashboard-configmap.yaml`: Grafana dashboard JSON definition.

## Part 1: Local Setup (Ubuntu)

To run locally without Docker:

*   **Prerequisites**
    *   Python 3.12+
    *   `venv` module

*   **Steps**
    1.  Navigate to `solution/app`.
    2.  Create env: `python3 -m venv .venv` and `source .venv/bin/activate`.
    3.  Install: `pip install -r requirements.txt`.
    4.  Run: `uvicorn app.main:app --reload --port 8000`.
    5.  Now check on your local machine url will be http://127.0.0.1:8000
     
## Part 2: Production Deployment (Kubernetes)

*   **Prerequisites**
    *   Docker
    installation: sudo apt install docker.io -y 
    sudo usermod -aG docker $USER
    sudo init 6 (for restart the ubuntu)

    * Kubectl
    * Installation:
      sudo apt update
      sudo apt install -y ca-certificates curl
      curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
      chmod +x kubectl
      sudo mv kubectl /usr/local/bin/
      kubectl version --client



    *   Kubernetes (Kind/Minikube)
    *   Installation: curl -Lo kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
         chmod +x kind
         sudo mv kind /usr/local/bin/
        kind version
        kind create cluster

    
    *   Helm 3
    *   Installation:
        curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
         helm version

         **Now nevigate to solution directory and run all the commands in it**

*   **Step 1: Build Image**
    *   Run: `docker build -t nebula-aurora-assignment:latest ./app`
    *   Load (Kind): `kind load docker-image nebula-aurora-assignment:latest`

*   **Step 2: Deploy**
    *   Update: `helm dependency update ./helm`
    *   Install: `helm install assignment ./helm`

*   **Step 3: Verification**
    *   Check: `kubectl get pods -w`
    *   *Wait for all pods to be Running.*

*   **Access Services (Port Forwarding)**
    *   **FastAPI**: `kubectl port-forward svc/assignment 8000:8000`
        *   URL: [http://localhost:8000/docs](http://localhost:8000/docs)
    *   **Grafana**: `kubectl port-forward svc/assignment-grafana 3000:80`
        *   URL: [http://localhost:3000/grafana/d/creation-dashboard-678/creation](http://localhost:3000/grafana/d/creation-dashboard-678/creation)
        *   Creds: `admin` / `admin`

## Monitoring Details

*   **Dashboard**: `creation-dashboard-678`
*   **Metrics**:
    *   `rate(users_created_total[5m])`
    *   `rate(posts_created_total[5m])`
