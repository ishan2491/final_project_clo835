# CLO835 Final Project: 2-Tiered Web Application on AWS EKS

## Project Overview

This project demonstrates the deployment of a containerized 2-tiered web application (Flask + MySQL) to Amazon EKS with automated CI/CD, persistent storage, and cloud integration features.

### Architecture

- **Frontend**: Flask web application with S3 background image integration
- **Backend**: MySQL database with persistent storage
- **Container Registry**: Amazon ECR
- **Orchestration**: Amazon EKS (Kubernetes)
- **Storage**: Amazon EBS with dynamic provisioning
- **CI/CD**: GitHub Actions
- **Configuration**: Kubernetes ConfigMaps and Secrets

## Features

✅ **Enhanced Flask Application**
- Dynamic background images from private S3 bucket
- Configurable group name and slogan via environment variables
- MySQL database integration for employee management
- Comprehensive logging and error handling

✅ **Cloud-Native Architecture**
- Containerized with Docker
- Deployed on managed Kubernetes (Amazon EKS)
- Persistent data storage with Amazon EBS
- Private container registry (Amazon ECR)

✅ **Security & Configuration**
- Kubernetes Secrets for sensitive data (DB credentials, AWS keys)
- ConfigMaps for application configuration
- RBAC (Role-Based Access Control) implementation
- Private S3 bucket integration with proper IAM

✅ **Automation & CI/CD**
- GitHub Actions workflow for automated builds
- Automatic image building and pushing to ECR
- Infrastructure as Code with Kubernetes manifests

## Application Components

### Web Application (`app.py`)
- Flask web server listening on port 81
- Employee management system (CRUD operations)
- S3 integration for dynamic background images
- Environment-based configuration

### Database
- MySQL 8.0 with persistent storage
- Pre-populated with sample employee data
- Automatic schema initialization

### Kubernetes Resources
- **Namespace**: `fp` (isolated environment)
- **Deployments**: Flask app and MySQL
- **Services**: LoadBalancer (internet access) and ClusterIP (internal)
- **ConfigMaps**: Application configuration
- **Secrets**: Database credentials and AWS access keys
- **PVC**: 3Gi persistent storage for MySQL
- **RBAC**: Service account with namespace permissions

## Quick Start

### Prerequisites
- AWS CLI configured with appropriate permissions
- kubectl installed and configured
- eksctl for EKS cluster management
- Docker for local development

### 1. Clone Repository
```bash
git clone https://github.com/YOUR_USERNAME/clo835-final-project.git
cd clo835-final-project
```

### 2. Local Development
```bash
# Build and test locally
docker build -t clo835-app:latest .
docker run -p 81:81 clo835-app:latest
```

### 3. Deploy to EKS
```bash
# Create EKS cluster
eksctl create cluster -f eks_config.yaml

# Create namespace
kubectl create namespace fp

# Deploy application
kubectl apply -f k8s-manifests/
```

## Project Structure

```
clo835-final-project/
├── app.py                          # Flask application
├── requirements.txt                # Python dependencies
├── Dockerfile                      # Container definition
├── eks_config.yaml                 # EKS cluster configuration
├── templates/                      # HTML templates
│   ├── base.html
│   ├── index.html
│   ├── employees.html
│   └── add_employee.html
├── k8s-manifests/                  # Kubernetes manifests
│   ├── configmap.yaml
│   ├── secrets.yaml
│   ├── pvc.yaml
│   ├── rbac.yaml
│   ├── mysql.yaml
│   └── app.yaml
├── .github/workflows/              # CI/CD pipeline
│   └── ci-cd.yml
└── static/                         # Static files (runtime)
```

## Configuration

### Environment Variables (ConfigMap)
- `GROUP_NAME`: Display name for the application
- `GROUP_SLOGAN`: Tagline shown in header
- `BACKGROUND_IMAGE_URL`: S3 URL for background image
- `MYSQL_HOST`: Database host (service name)
- `MYSQL_DB`: Database name

### Secrets
- `MYSQL_USER`: Database username
- `MYSQL_PASSWORD`: Database password
- `AWS_ACCESS_KEY_ID`: AWS access key for S3
- `AWS_SECRET_ACCESS_KEY`: AWS secret key for S3
- `AWS_SESSION_TOKEN`: AWS session token

## CI/CD Pipeline

The GitHub Actions workflow automatically:
1. **Tests** the application code
2. **Builds** Docker image
3. **Pushes** to Amazon ECR
4. **Triggers** on every push to main branch

### GitHub Secrets Required
```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_SESSION_TOKEN
```

## Deployment Verification

### 1. Check Pods Status
```bash
kubectl get pods -n fp
```

### 2. Verify Services
```bash
kubectl get services -n fp
```

### 3. Check Persistent Storage
```bash
kubectl get pvc -n fp
kubectl get pv
```

### 4. Access Application
```bash
# Get LoadBalancer URL
kubectl get service flask-app-service -n fp
```

## Key Features Demonstrated

### 🔄 **Data Persistence**
MySQL data persists even when pods are deleted and recreated, thanks to persistent volumes.

### 🖼️ **Dynamic Configuration**
Background images can be changed by updating the ConfigMap and restarting pods.

### 🔒 **Security**
- Private container registry
- Kubernetes secrets for sensitive data
- Private S3 bucket access
- RBAC implementation

### 📈 **Scalability**
Ready for horizontal pod autoscaling and cluster autoscaling.

## Troubleshooting

### Common Issues

**PVC Pending Status**
```bash
# Install EBS CSI driver
eksctl create addon --name aws-ebs-csi-driver --cluster CLUSTER_NAME --region us-east-1
```

**Image Pull Errors**
```bash
# Create ECR secret
kubectl create secret docker-registry ecr-secret \
  --docker-server=ACCOUNT.dkr.ecr.REGION.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region REGION) \
  --namespace=fp
```

**S3 Access Issues**
- Verify AWS credentials in secrets
- Check S3 bucket permissions
- Ensure correct S3 URL format in ConfigMap

## Cleanup

```bash
# Delete EKS cluster
eksctl delete cluster -f eks_config.yaml

# Delete ECR repository
aws ecr delete-repository --repository-name clo835-app --force

# Delete S3 bucket
aws s3 rb s3://YOUR-BUCKET-NAME --force
```

## Learning Outcomes

This project demonstrates proficiency in:
- **Containerization** with Docker
- **Kubernetes** orchestration and resource management
- **AWS Cloud Services** (EKS, ECR, EBS, S3, IAM)
- **CI/CD** with GitHub Actions
- **Infrastructure as Code** practices
- **Security** best practices in cloud environments
- **Persistent Storage** in containerized environments

## Technologies Used

- 🐍 **Python/Flask** - Web application framework
- 🐳 **Docker** - Containerization
- ☸️ **Kubernetes** - Container orchestration
- ☁️ **AWS EKS** - Managed Kubernetes service
- 🗄️ **MySQL** - Database
- 📦 **Amazon ECR** - Container registry
- 💾 **Amazon EBS** - Persistent storage
- 🪣 **Amazon S3** - Object storage
- 🔄 **GitHub Actions** - CI/CD pipeline
- 🔧 **eksctl** - EKS cluster management

## Author

**CLO835 Student Project**
- Course: Cloud Computing and DevOps
- Institution: Seneca College
- Semester: [Your Semester/Year]

---

⭐ **Star this repository if you found it helpful!**
