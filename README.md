# 🛒 Spring Boot CRUD Example with MySQL, Docker, Jenkins & Kubernetes

This is a full-stack **Spring Boot CRUD application** with MySQL, built using **Maven**, containerized with **Docker**, deployed via **Kubernetes**, and integrated with **Jenkins CI/CD**. Ideal for showcasing **DevOps skills** in interviews and real-world scenarios.

---

## 🔧 Tech Stack

- 🧩 Spring Boot
- 🐬 MySQL 8
- ☸️ Kubernetes (AKS compatible)
- 🐳 Docker & Docker Compose
- 🧪 Jenkins (Pipeline CI/CD)
- ☁️ Azure Container Registry (ACR)
- 📦 Maven
- ⚙️ Hibernate (JPA)

---

## 🚀 Project Setup

### 1. Prerequisites

Install these tools on your system:

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Kubectl CLI](https://kubernetes.io/docs/tasks/tools/)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- [JDK 17+](https://adoptopenjdk.net/)
- [Maven](https://maven.apache.org/download.cgi)
- [Jenkins](https://www.jenkins.io/download/)
- Optional: [VS Code + Docker & Kubernetes extensions](https://code.visualstudio.com/)

---

## 🧱 Architecture

```bash
                            ┌──────────────────────────┐
                            │        Jenkins CI/CD     │
                            └────────────┬─────────────┘
                                         │
                            ┌────────────▼─────────────┐
                            │  Docker Build & Push     │
                            └────────────┬─────────────┘
                                         │
                          ┌──────────────▼───────────────┐
                          │ Azure Container Registry (ACR)│
                          └──────────────┬───────────────┘
                                         │
                           ┌─────────────▼──────────────┐
                           │  Kubernetes (AKS/Minikube) │
                           └─────────────┬──────────────┘
               ┌─────────────────────────▼────────────────────────┐
               │ Pods: Spring Boot App (CRUD) + MySQL (Stateful) │
               └──────────────────────────────────────────────────┘


🐳 Docker Setup (Local)
Step 1: Build & Start Locally

docker-compose up --build


Docker Compose Structure

version: '3.8'

services:
  mysql:
    image: mysql:8
    container_name: mysql-container
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: Password
      MYSQL_DATABASE: javatechie
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - springboot-network

  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins-container
    ports:
      - "8080:8080"
    networks:
      - springboot-network

volumes:
  mysql_data:

networks:
  springboot-network:
    driver: bridge


☸️ Kubernetes Deployment
1. MySQL Deployment
yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "Password"
        - name: MYSQL_DATABASE
          value: "javatechie"
        ports:
        - containerPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
2. Spring Boot App Deployment
Ensure your environment variables are configured properly:

yaml


env:
  - name: SPRING_DATASOURCE_URL
    value: jdbc:mysql://mysql-service:3306/javatechie
  - name: SPRING_DATASOURCE_USERNAME
    value: root
  - name: SPRING_DATASOURCE_PASSWORD
    value: Password
3. Deploy to Kubernetes

bash

kubectl apply -f mysql-deployment.yaml
kubectl apply -f ecommerce-deployment.yaml


🧪 Verify & Troubleshoot
Use kubectl logs <pod-name> to debug.

If error is Communications link failure, check:

MySQL is running (kubectl get pods)

App has correct JDBC config

Spring Boot waits for MySQL using initContainer or retry config

⚙️ Jenkins Pipeline (CI/CD)
Pipeline stages:

Clone the Spring Boot repo

Build the JAR using mvn clean install

Build Docker image and tag it

Push image to Azure Container Registry (ACR)

Deploy to Kubernetes using kubectl apply

You can write this as a Jenkinsfile or define it via Freestyle Pipeline.




🔐 Docker Login with Azure ACR
bash

az login
az acr login --name ecommerceacr123
docker tag <local-image> ecommerceacr123.azurecr.io/spring-boot-crud-example:latest
docker push ecommerceacr123.azurecr.io/spring-boot-crud-example:latest


🔄 Common Errors & Fixes
❌ Error	✅ Fix
CrashLoopBackOff	Check logs. Usually DB connection issue
ImagePullBackOff	Make sure you're logged into ACR and image is pushed
CommunicationsException	App can't reach MySQL — verify service name & DNS
Jenkins 403 or Login	Use admin and password from /var/jenkins_home/secrets/initialAdminPassword


📁 Build Output
After build:

bash
Copy
Edit
target/spring-boot-crud-example-0.0.1-SNAPSHOT.jar
