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
- **YML Configuration**:
  - Created deployment and service YML files to deploy Twoge on Minikube.
  - Created deployment and service YML files to deploy a Postgres database.
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

### 8. CI/CD
- **GitHub Actions Setup**:
  - Set up a CI/CD pipeline using GitHub Actions for automating Docker image building and deployment to EKS.
  - Ensured that the pipeline utilized the appropriate namespace and Docker Hub credentials.

## Getting Started

### Prerequisites

Before starting, ensure you have the following tools installed:

- **Docker**: To build and push Docker images.
  - Installation instructions can be found [here](https://docs.docker.com/get-docker/).

- **Minikube**: For local Kubernetes deployment.
  - Installation instructions can be found [here](https://minikube.sigs.k8s.io/docs/start/).

- **kubectl**: The Kubernetes command-line tool.
  - Installation instructions can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

- **AWS CLI**: To manage AWS services and interact with EKS.
  - Installation instructions can be found [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

### Setting Up GitHub Secrets

To securely manage your AWS credentials and Docker Hub credentials, create the following GitHub secrets in your repository:

1. **AWS Credentials**:
   - `AWS_ACCESS_KEY_ID`: Your AWS Access Key ID.
   - `AWS_SECRET_ACCESS_KEY`: Your AWS Secret Access Key.
   - `AWS_REGION`: Your AWS region (e.g., `us-west-2`).

2. **Docker Hub Credentials**:
   - `DOCKER_USERNAME`: Your Docker Hub username.
   - `DOCKER_PASSWORD`: Your Docker Hub password.

To add secrets in GitHub:

1. Navigate to your repository on GitHub.
2. Go to `Settings` > `Secrets` > `Actions`.
3. Click `New repository secret` and add the required secrets.

### 1. Clone the Repository
Fork the repository and clone it to your local machine:
```bash
git clone <your-forked-repo-url>
cd twoge
```

### 2. Build Docker Image
Build the Docker image for the Twoge application:
```bash
docker build -t twoge-kube:latest .
```

### 3. Deploy on Minikube
- Create a secret.yml file:
```bash
echo "apiVersion: v1
kind: Secret
metadata:
  name: twoge-secret
  namespace: tristan
type: Opaque
data:
  postgres-username: dXNlcjE= # user1 base64 encoded
  postgres-password: cGFzc3dvcmQ= # password base64 encoded" > k8s/secrets.yml
```

- Apply the YML files for deployment and service:
```bash
kubectl apply -f namespace.yml
kubectl apply -f flask-deployment.yml
kubectl apply -f flask-service.yml
kubectl apply -f configmap.yml
kubectl apply -f secrets.yml
kubectl apply -f postgres-deployment.yml
kubectl apply -f postgres-service.yml
```
- This can also be ran with the following:
```bash
kubectl apply -f namespace.yml
kubectl apply -f .
```
- Once the pods are running you can start the app with:
```bash
minikube service flask-service
```

### Switching from Minikube to AWS EKS

1. **Update Your Kubernetes Configuration**:
   - When switching from Minikube to AWS EKS, update your kubeconfig file to point to the EKS cluster. Run:
     ```bash
     aws eks update-kubeconfig --name <your-eks-cluster-name> --region <your-aws-region>
     ```

2. **Update Kubernetes Files**:
   - In the `flask-service.yml` file, change the service type from `NodePort` (used in Minikube) to `LoadBalancer` for EKS to allow external access.
   - In the `storageclass.yml` file, comment out the code needed for minikube and uncomment out the code needed for EKS.

3. **Apply the YML Files to EKS**:
   - Use `kubectl` to apply your configurations to the EKS cluster:
     ```bash
     kubectl apply -f k8s/namespace.yml
     kubectl apply -f k8s/ -n <namespace>
     ```

4. **Verify Deployment**:
   - Check the status of your pods and services on EKS:
     ```bash
     kubectl get pods -n <namespace>
     kubectl get services -n <namespace>
     ```

### 5. CI/CD
- Try making a small change to the flask-deployment.yml and pushing it up to github. For example, change the number replicas. 
- After pushing up the change, go to the actions tab in your repo and verify the jobs finsihed successfuly.
- You should now also see the number of flask-deployment pods changed and you should have a new docker image on dockerhub.