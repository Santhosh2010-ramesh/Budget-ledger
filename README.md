# 💰 Budget Ledger - Cloud Native Deployment

A secure and scalable Spring Boot-based Budget Ledger Application containerized with Docker and deployed to **AWS Elastic Kubernetes Service (EKS)** using Infrastructure as Code (IaC) and CI/CD pipelines.
## Website Access
http://a3fe3df495a4f49998bbbd05c42cf14a-1466525858.us-east-1.elb.amazonaws.com/login
---

## 📁 Project Structure

```
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
```

---

## 🛠️ Features

✅ Java Spring Boot Budget Management System  
✅ Docker Containerized Application  
✅ Kubernetes Deployment & Service Manifests  
✅ CI/CD Pipeline with AWS CodePipeline & CodeBuild  
✅ SonarQube Code Quality Analysis  
✅ Trivy Container Vulnerability Scanning  
✅ EKS Cluster Deployment for Scalability  
✅ YAML Configurations for Cloud-Native Infrastructure  

---

## 🛠️ Prerequisites

- AWS CLI configured (`aws configure`)
- kubectl installed
- eksctl installed
- Docker installed
- SonarQube server (local or hosted)
- Trivy installed
- AWS Account with permissions for:
  - EKS
  - EC2
  - IAM
  - VPC
  - CodePipeline & CodeBuild
  - ECR (Elastic Container Registry)

---

## ⚙️PHASE 1:  EKS Deployment Steps

### 1⃣ Clone the Repository

```bash
git clone https://github.com/Santhosh2010-ramesh/your-repo-name.git
cd your-repo-name
```

---

### 2⃣ Build Docker Image and Push to ECR

Update the region/account details accordingly.

```bash
# Authenticate Docker with ECR
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com

# Build Docker Image
docker build -t budget-ledger-app .

# Tag and Push Image
docker tag budget-ledger-app:latest <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/budget-ledger-app:latest
docker push <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/budget-ledger-app:latest
```

---

### 3⃣ SonarQube Code Analysis

Ensure your SonarQube server is running and credentials are set.

```bash
# Example Maven SonarQube scan
mvn clean verify sonar:sonar   -Dsonar.projectKey=budget-ledger   -Dsonar.host.url=http://<sonarqube-url>   -Dsonar.login=<sonarqube-token>
```
![image](https://github.com/user-attachments/assets/5acf625a-780a-443b-8321-9b0b1969344c)

---

### 4⃣ Trivy Security Scan

```bash
# Scan the Docker image for vulnerabilities
trivy image <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/budget-ledger-app:latest
```

---

### 5⃣ Deploy to AWS EKS

```bash
# Update kubeconfig to connect to EKS
aws eks update-kubeconfig --region <your-region> --name <cluster-name>

# Deploy Application
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

### 6⃣ Verify Deployment

```bash
kubectl get pods
kubectl get svc
```

Access the service using the EXTERNAL-IP provided by `kubectl get svc`.

---

## 📦 PHASE 2: CI/CD Automation with CodePipeline

This project includes `buildspec.yml` for AWS CodeBuild integration:

✅ Builds the Docker Image  
✅ Performs SonarQube Code Analysis  
✅ Scans Image with Trivy  
✅ Pushes to AWS ECR  
✅ Deploys to EKS  

Ensure your CodePipeline is configured to:

- Connect with this GitHub repository  
- Use CodeBuild with the provided `buildspec.yml`  

---

# 🔄 Phase 3 - Multi-Region EKS Deployment with CodePipeline

### 1⃣ CloudFormation Deployment to AWS EKS (us-east-1)

- CloudFormation template provisions EKS Cluster and supporting resources.
- CodePipeline triggers CloudFormation stack update.
- Application is deployed to EKS in `us-east-1` region.
![image](https://github.com/user-attachments/assets/cc4a67cb-f1d2-4777-8e29-02be34491958)


### 2⃣ Terraform Deployment to AWS EKS (us-west-1)

- Terraform scripts provision EKS Cluster and networking in `us-east-2` region.
- CodePipeline triggers Terraform deployment.
- Application is deployed to EKS in `us-east-2` region.
![image](https://github.com/user-attachments/assets/72cb5bda-6acb-416f-85a0-ad1668d30324)


### Phase 4 - Route 53 Failover Routing for Disaster Recovery
#1.-CloudFormation Route 53 Failover Setup (us-east-1)
-CloudFormation stack provisions Route 53 records with failover routing policy.
-Primary DNS points to the EKS Load Balancer in us-east-1.
-Health checks configured to monitor primary endpoint.
Ensure CodePipeline stages are configured with appropriate permissions and regional targeting.
![Screenshot 2025-06-24 023447](https://github.com/user-attachments/assets/5c3c8d20-f32c-43d0-bab7-73afbd9f3129)

#2. -Terraform Route 53 Failover Setup (us-east-2)
-Terraform provisions secondary Route 53 DNS records for failover.
-Secondary DNS points to EKS Load Balancer in us-east-2.
-Automatically switches traffic to us-east-2 if primary region is unhealthy.
-Codepipeline for us-west-1
![image](https://github.com/user-attachments/assets/52accade-1f14-4811-b3fb-183e60fc82cb)


High availability achieved with DNS-based failover using Route 53.


### End Application through load balancer Access
-Signup Page
![image](https://github.com/user-attachments/assets/a8cdd196-35c8-46cd-9f6b-e2b96b18acad)

-Login Page
![image](https://github.com/user-attachments/assets/042b146e-3b4d-4e6f-ae89-4bb10fa07192)

-Dashboard Page
![image](https://github.com/user-attachments/assets/2dc24ea0-4c09-47b9-852d-2ead354ceb81)


---

## 📚 Additional Notes

- Ensure your EKS clusters and node groups are running.
- Modify `deployment.yaml` for image versions and resources.
- Service exposure can be adjusted (LoadBalancer, NodePort, etc.).

---

## 🤝 Contributing

Contributions are welcome! Please fork the repo and submit pull requests for improvements or new features.

---

## 📄 License

MIT License
