💰 Budget Ledger - Cloud Native Deployment
A secure and scalable Spring Boot-based Budget Ledger Application containerized with Docker and deployed to AWS Elastic Kubernetes Service (EKS) using Infrastructure as Code (IaC) and CI/CD pipelines.

📁 Project Structure
pgsql
Copy
Edit
.
├── .mvn/wrapper              # Maven Wrapper
├── src                       # Application Source Code
├── .gitattributes
├── .gitignore
├── Dockerfile                # Docker Image Build Definition
├── buildspec.yml             # AWS CodeBuild Build Specification
├── deployment.yaml           # Kubernetes Deployment Manifest
├── mvnw, mvnw.cmd            # Maven Wrapper Scripts
├── pom.xml                   # Maven Project Configuration
├── service.yaml              # Kubernetes Service Manifest
🚀 Features
✅ Java Spring Boot Budget Management System
✅ Docker Containerized Application
✅ Kubernetes Deployment & Service Manifests
✅ CI/CD Pipeline with AWS CodePipeline & CodeBuild
✅ EKS Cluster Deployment for Scalability
✅ YAML Configurations for Cloud-Native Infrastructure

🛠️ Prerequisites
AWS CLI configured (aws configure)

kubectl installed

eksctl installed

Docker installed

AWS Account with permissions for:

EKS

EC2

IAM

VPC

CodePipeline & CodeBuild

ECR (Elastic Container Registry)

⚙️ EKS Deployment Steps
1️⃣ Clone the Repository
bash
Copy
Edit
git clone https://github.com/Santhosh2010-ramesh/your-repo-name.git
cd your-repo-name
2️⃣ Build Docker Image and Push to ECR
Update the region/account details accordingly.

bash
Copy
Edit
# Authenticate Docker with ECR
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com

# Build Docker Image
docker build -t budget-ledger-app .

# Tag and Push Image
docker tag budget-ledger-app:latest <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/budget-ledger-app:latest
docker push <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/budget-ledger-app:latest
3️⃣ Deploy to AWS EKS
bash
Copy
Edit
# Update kubeconfig to connect to EKS
aws eks update-kubeconfig --region <your-region> --name <cluster-name>

# Deploy Application
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
4️⃣ Verify Deployment
bash
Copy
Edit
kubectl get pods
kubectl get svc
Access the service using the EXTERNAL-IP provided by kubectl get svc.

📦 CI/CD Automation with CodePipeline
This project includes buildspec.yml for AWS CodeBuild integration:

✅ Builds the Docker Image
✅ Pushes to AWS ECR
✅ Deploys to EKS

Ensure your CodePipeline is configured to:

Connect with this GitHub repository

Use CodeBuild with the provided buildspec.yml

📚 Additional Notes
Ensure your EKS cluster and node groups are running.

You may modify deployment.yaml to set correct image versions and resource limits.

Service exposure can be adjusted (LoadBalancer, NodePort, etc.).

🤝 Contributing
Contributions are welcome! Please fork the repo and submit pull requests for improvements or new features.

📄 License
MIT License
