[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/97WR5HaV)


# Kubernetes Flask PostgreSQL Deployment

This project demonstrates a Kubernetes deployment of a Flask web application with PostgreSQL database using Minikube. The application showcases containerization, persistent storage, and Kubernetes service configuration.

## Project Structure
```
k8s-flask-app/
│── manifests/
│   │── deployment/
│   │   │── flask-deployment.yaml
│   │   │── postgres-deployment.yaml
│   │── service/
│   │   │── flask-service.yaml
│   │   │── postgres-service.yaml
│   │── configmap/
│   │   │── postgres-configmap.yaml
│   │── secret/
│   │   │── postgres-secret.yaml
│── app/
│   │── Dockerfile
│   │── requirements.txt
│   │── app.py
│── submission/
    │── deployment_snapshot.txt
    │── kubectl_get_all.txt
    │── scaling_up.txt
    │── scaling_down.txt
    │── replica_investigation.txt
```

## Components

### Flask Application
- Simple web application that connects to PostgreSQL
- Containerized using Docker
- Deployed with multiple replicas for high availability

### PostgreSQL Database
- Persistent storage using PVC
- Headless service for internal communication
- Secured credentials using Kubernetes secrets

### Kubernetes Configuration
- Deployments for both Flask and PostgreSQL
- Services for network access
- ConfigMap for configuration
- Secrets for sensitive data
- PersistentVolumeClaim for database storage

## Setup Instructions

1. Start Minikube:
```bash
minikube start --driver=docker
```

2. Build Flask Application:
```bash
cd app
eval $(minikube docker-env)
docker build -t flask-app:latest .
```

3. Apply Kubernetes Manifests:
```bash
cd ../manifests
kubectl apply -f configmap/postgres-configmap.yaml
kubectl apply -f secret/postgres-secret.yaml
kubectl apply -f postgres-pvc.yaml
kubectl apply -f deployment/postgres-deployment.yaml
kubectl apply -f service/postgres-service.yaml
kubectl apply -f deployment/flask-deployment.yaml
kubectl apply -f service/flask-service.yaml
```

4. Verify Deployment:
```bash
kubectl get all
```

## Scaling

The application supports horizontal scaling:
```bash
# Scale up
kubectl scale deployment flask-app --replicas=4

# Scale down
kubectl scale deployment flask-app --replicas=1
```

## Testing

Access the application:
```bash
minikube service flask-service --url
```

## Submission Files

1. `deployment_snapshot.txt`: Internal deployment configuration
2. `kubectl_get_all.txt`: Cluster resource status
3. `scaling_up.txt`: Scaling test results (increasing replicas)
4. `scaling_down.txt`: Scaling test results (decreasing replicas)
5. `replica_investigation.txt`: Analysis of replica scaling limits

## Key Features

1. High Availability
   - Multiple Flask application replicas
   - Persistent database storage

2. Security
   - Secrets management for credentials
   - Internal service communication

3. Scalability
   - Horizontal scaling capability
   - Resource management

4. Configuration Management
   - ConfigMaps for application settings
   - Environment variable injection

## Technologies Used

- Kubernetes 1.32.0
- Minikube
- Flask
- PostgreSQL 13
- Docker
- Python 3.9

## Notes

- The PostgreSQL password is base64 encoded in the secrets file
- The Flask application automatically reconnects to the database
- Service discovery is handled by Kubernetes DNS


