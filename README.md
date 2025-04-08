# 🛒 eCommerce CRUD API with DevOps CI/CD Pipeline

This project is a Java Spring Boot–based CRUD API for managing eCommerce entities like products, orders, and customers. It is fully containerized and deployed to Azure Kubernetes Service (AKS) using DevOps tools like Docker, Terraform, Jenkins, and Azure Container Registry (ACR).

---

## 📦 Features

- Java Spring Boot REST API (CRUD)
- Dockerized for containerization
- Terraform for infrastructure provisioning on Azure
- Jenkins for CI/CD automation
- Kubernetes (AKS) deployment
- Azure Container Registry (ACR) for image hosting

---

## 🧰 DevOps Tools & Technologies

| Tool | Purpose |
|------|--------|
| Java + Spring Boot | Application development |
| Maven | Build tool |
| Docker | Containerization |
| Terraform | Infrastructure as Code |
| Azure CLI | Azure resource management |
| Kubernetes (AKS) | Orchestration |
| Azure Container Registry (ACR) | Docker image hosting |
| Jenkins | CI/CD automation |
| Git | Source control |
| VS Code | Code editor |

---

## 🚀 Setup Instructions

### 🛠️ Prerequisites

Install the following:

- [Java 17+](https://adoptium.net/)
- [Maven](https://maven.apache.org/)
- [Docker](https://www.docker.com/products/docker-desktop)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Git](https://git-scm.com/)
- [Jenkins (via Docker)](https://www.jenkins.io/doc/book/installing/docker/)

---

## 🏗️ Project Structure

ecommerce-crud/ ├── src/ # Java source code ├── target/ # Maven build artifacts ├── Dockerfile # App container definition ├── Jenkinsfile # CI/CD pipeline config ├── ecommerce-deployment.yaml # Kubernetes Deployment ├── ecommerce-service.yaml # Kubernetes Service └── terraform/ ├── main.tf # Azure resources (AKS, ACR) ├── variables.tf └── outputs.tf

yaml
Copy
Edit

---

## 🔧 Running Locally

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/ecommerce-crud.git
cd ecommerce-crud
2. Build the Application
bash
Copy
Edit
mvn clean package
3. Run Locally
bash
Copy
Edit
java -jar target/ecommerce-crud.jar
# OR using Docker:
docker build -t ecommerce-crud .
docker run -p 8080:8080 ecommerce-crud
☁️ Deploying to Azure (with Terraform)
1. Login to Azure
bash
Copy
Edit
az login
2. Create Azure Infrastructure
bash
Copy
Edit
cd terraform
terraform init
terraform apply
Creates:

Resource Group

AKS Cluster

Azure Container Registry

🐳 Push Docker Image to ACR
bash
Copy
Edit
az acr login --name <your-acr-name>
docker tag ecommerce-crud <acr-name>.azurecr.io/ecommerce-crud
docker push <acr-name>.azurecr.io/ecommerce-crud
☸️ Deploy to Kubernetes (AKS)
bash
Copy
Edit
az aks get-credentials --resource-group <your-rg> --name <your-aks>
kubectl apply -f ecommerce-deployment.yaml
kubectl apply -f ecommerce-service.yaml
🔁 Jenkins CI/CD Setup
1. Start Jenkins via Docker
bash
Copy
Edit
docker run -d -p 8081:8080 -p 50000:50000 --name jenkins -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
2. Initial Jenkins Setup
bash
Copy
Edit
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
Go to http://localhost:8081 and complete setup.

3. Add Jenkins Pipeline
Create a new pipeline job

Point it to this repo

Use the provided Jenkinsfile

📡 API Endpoints (Sample)
Method	Endpoint	Description
GET	/products	List all products
POST	/products	Add a new product
PUT	/products/{id}	Update a product
DELETE	/products/{id}	Delete a product
💲 Billing Notes
Tool	Free?	Notes
Azure	❌	Billed for AKS, ACR, VMs
Docker Desktop	✅	Free for personal use
Terraform	✅	Free CLI
Jenkins	✅	Free if self-hosted
Git	✅	Free
Kubernetes (local)	✅	Use Minikube or Docker Desktop to avoid costs
