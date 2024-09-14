# Assessment-4: Deploying Python Flask-Based Twitter Alternative 'Twoge' Using Kubernetes and AWS EKS

![alt text](./Screenshot%202024-09-14%20at%2010.50.42 AM.png)

## Objective

The purpose of this assessment was to deepen my understanding of Kubernetes by deploying the Twoge application. The task involved containerizing the application, deploying it on a local Kubernetes cluster (Minikube), and then migrating the deployment to AWS Elastic Kubernetes Service (EKS). Additionally, the assessment aimed to familiarize me with managing containerized applications, configuring database connections, and implementing lifecycle management through probes.

## Milestones

### 1. Docker Image Creation
- **Dockerizing Twoge**: 
  - I began by writing a Dockerfile for the Twoge application, ensuring it included all necessary dependencies and configurations for the Python Flask environment.
  - Built the Docker image and tested it using Docker Compose to ensure it worked as expected.

### 2. Deployment on Minikube
- **YAML Configuration**:
  - Created deployment and service YAML files to deploy Twoge on Minikube.
  - Created deployment and service YAML files to deploy a postgres database.
  - Configured the app service to expose the application within the Minikube cluster.

### 3. Database Configuration
- **ConfigMap and Secrets**:
  - Defined ConfigMap and Secrets to securely manage and inject database configurations into the application.
  - Deployed the database on Kubernetes, ensuring proper connectivity and security.

### 4. Namespace Setup
- **Namespace Creation**:
  - Created a dedicated namespace for the Twoge application to organize and isolate resources within the cluster.

### 5. Probes Implementation
- **Lifecycle Management**:
  - Implemented a startup probe to monitor the application's startup process and ensure it was running smoothly.
  - As a bonus, I also implemented a readiness probe to manage application readiness more effectively.

### 6. Deployment on AWS EKS
- **Migration to EKS**:
  - Migrated the Twoge deployment from Minikube to AWS EKS.
  - Updated the service type to `LoadBalancer` to enable public access on EKS.

### 7. Persistent Volume Claim
- **Storage Configuration**:
  - Created a Persistent Volume Claim (PVC) using a storage class to ensure data persistence for the application.

### 8. CI/CD (Optional)
- **GitHub Actions Setup**:
  - Set up a CI/CD pipeline using GitHub Actions for automating Docker image building and deployment to EKS.
  - Ensured that the pipeline utilized the appropriate namespace and Docker Hub credentials.

## Getting Started

### 1. Clone the Repository
Clone the repository to your local machine:
```bash
git clone https://github.com/chandradeoarya/twoge.git
cd twoge
```

### 2. Build Docker Image
Build the Docker image for the Twoge application:
```bash
docker build -t twoge-kube:latest .
```

### 3. Deploy on Minikube
- Apply the YAML files for deployment and service:
```bash
kubectl apply -f namespace.yaml
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
kubectl apply -f configmap.yaml
kubectl apply -f secrets.yaml
kubectl apply -f postgres-deployment.yaml
kubectl apply -f postgres-service.yaml
```
- This can also be ran with the following:
```bash
kubectl apply -f namespace.yaml
kubectl apply -f .
```
- Once the pods are running you can start the app with:
```bash
minikube service flask-service
```

### 4. Deploy on AWS EKS
- Update the service type in `flask-service.yaml` to `LoadBalancer`.
- Apply the YAML files to the EKS cluster:
```bash
kubectl apply -f namespace.yaml
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
kubectl apply -f configmap.yaml
kubectl apply -f secrets.yaml
kubectl apply -f postgres-deployment.yaml
kubectl apply -f postgres-service.yaml
kubectl apply -f storageclass.yaml
kubectl apply -f pvc.yaml
```

### 5. CI/CD Setup (Optional)
- Configure GitHub Actions for automated builds and deployments by updating the workflow file in `.github/workflows/`.

## Deliverables

- **Dockerfile**: Used to create the Docker image of the Twoge application.
- **YAML Files**: Includes configurations for deployment, services, ConfigMap, Secrets, Namespace, and probes.
- **Architecture Diagram**: Outlines the deployment architecture.
- **Screenshots/Screen Recordings**: Demonstrate successful deployments on both Minikube and AWS EKS.