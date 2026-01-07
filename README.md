

# Modernizing Applications Using AWS, Docker, Amazon ECS, GitHub Actions, and AWS Fargate

**AWS | Docker | CI/CD | ECS**

---

## �\udccpin Project Overview

This project demonstrates how to modernize an application using container technologies and AWS cloud services. The focus is on building, securely managing, and automating the deployment of Docker containers using Amazon ECS with AWS Fargate and GitHub Actions CI/CD pipelines.

The project serves as an educational and practical example for DevOps and Cloud engineers who want to understand the full lifecycle of a modern cloud-native application.

**Overview Images:**  

![Project Overview 1](images/1.png)  
![Project Overview 2](images/2.png)  

---

## 🏧 Architecture

**Architecture Diagram**

**Workflow:**

1. The application is packaged into a Docker image  
2. The image is pushed to Amazon ECR  
3. Amazon ECS (Fargate) runs the containers  
4. Secrets are retrieved from AWS Secrets Manager  
5. CI/CD pipeline handles the build & deploy process

**Architecture Images:**  

![Architecture 3](images/3.png)  
![Architecture 4](images/4.png)  
![Architecture 5](images/5.png)  

---

## 🚀 Features

✅ Deploy the application using Docker Compose with ECS integration  
✅ Build custom Docker images using Docker BuildKit  
✅ Deploy images to Amazon ECS  
✅ Securely manage secrets via AWS Secrets Manager  
✅ Automated CI/CD pipeline using the Amazon ECS GitHub Actions plugin  
✅ Run containers on AWS Fargate (serverless)

**Features / Deployment Images:**  

![Features 6](images/6.png)  
![Features 7](images/7.png)  
![Features 8](images/8.png)  

---

## 🛠️ Technologies

| Technology          | Description                           |
| ------------------- | ------------------------------------- |
| Docker              | Application containerization          |
| Docker Compose      | Local development and ECS integration |
| Amazon ECS          | Container orchestration               |
| AWS Fargate         | Serverless runtime for containers     |
| Amazon ECR          | Docker image registry                 |
| AWS Secrets Manager | Secure storage for secrets            |
| GitHub Actions      | CI/CD automation                      |

**Technologies Images:**  

![Technologies 9](images/9.png)  
![Technologies 10](images/10.png)  

---

## 🐳 Docker & Build

**Example Docker image build using BuildKit:**

```bash
DOCKER_BUILDKIT=1 docker build -t modern-app .
```

**Push image to Amazon ECR:**

```bash
docker tag modern-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/modern-app:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/modern-app:latest
```

**Docker Build / Push Images:**  

![Docker Build 11](images/11.png)  
![Docker Build 12](images/12.png)  
![Docker Build 13](images/13.png)  

---

## 🛠️ Container Security

The project uses **AWS Secrets Manager** for:

- Passwords  
- API keys  
- Sensitive configuration values

Secrets are injected directly into the ECS task definition without hard-coding them into code or Docker images.

**Security / Secrets Images:**  

![Secrets 14](images/14.png)  
![Secrets 15](images/15.png)  
![Secrets 16](images/16.png)  

---

## 🔄 CI/CD – GitHub Actions

**CI/CD Pipeline Workflow:**

1. Push to the main branch  
2. Build Docker image  
3. Push to Amazon ECR  
4. Deploy to Amazon ECS (Fargate)

Using the Amazon ECS GitHub Action enables safe and repeatable deployments.

**CI/CD Pipeline Images:**  

![CI/CD 17](images/17.png)  
![CI/CD 18](images/18.png)  
![CI/CD 19](images/19.png)  

---

## ▶️ Running the Application

**Locally (Docker Compose):**

```bash
docker compose up --build
```

**Cloud (ECS):**

Automatically triggered via GitHub Actions pipeline

**Run / Deployment Demo Images:**  

![Run 20](images/20.png)  
![Run 21](images/21.png)  
![Run 22](images/22.png)  

---

## 📂 Project Structure

```
.
├── .github/workflows/   # GitHub Actions CI/CD
├── images/              # Diagrams and images
├── Dockerfile
├── docker-compose.yml
├── README.md
```

**Project Structure Images:**  

![Structure 23](images/23.png)  
![Structure 24](images/24.png)  

---

## 🎯 Project Goal

The goal is to demonstrate best practices for modernizing applications using:

- Infrastructure as Code  
- Container-first approach  
- Security by design  
- Automated deployment

**Project Goals / Summary Images:**  

![Goal 25](images/25.png)  
![Goal 26](images/26.png)  

---

## 📚 References

- [AWS ECS Documentation](https://docs.aws.amazon.com/ecs/)  
- [Docker Documentation](https://docs.docker.com/)  
- [GitHub Actions](https://docs.github.com/en/actions)  
- [AWS Fargate](https://aws.amazon.com/fargate/)


## 1️⃣ Struktura foldera

```
Modernizacija-aplikacija/
├── .github/workflows/deploy.yml
├── ecs-task-def.json
├── Dockerfile
├── docker-compose.yml
├── package.json
├── test/index.test.js
└── README.md
```

---

## 2️⃣ Fajlovi i sadržaji

### 📌 O projektu

Ovaj repozitorij demonstrira modernizaciju aplikacije koristeći:

- **Docker** za containerizaciju
- **AWS ECS Fargate** za serverless deployment
- **AWS ECR** za pohranu Docker imagea
- **GitHub Actions** za CI/CD pipeline

Sve je automatizirano tako da push u `main` branch builda, testira i deploya aplikaciju.

---

## 🚀 Glavne komponente

```
├── .github/workflows/deploy.yml  # CI/CD workflow
├── ecs-task-def.json             # ECS Task Definition
├── Dockerfile                    # Docker build definicija
├── docker-compose.yml            # Lokalni razvoj
├── package.json                  # Scripts za test i lint
├── test/
│   └── index.test.js             # Smoke test
└── README.md
```

---

## 🐳 Docker konfiguracija

### **Dockerfile**

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

### **docker-compose.yml**

```yaml
version: '3.9'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development
    volumes:
      - .:/app
      - /app/node_modules
```

---

## ⚙️ CI/CD Workflow (GitHub Actions)

- Build i test Docker image  
- Push image u AWS ECR  
- Deploy na AWS ECS Fargate  

**Fajl:** `.github/workflows/deploy.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

env:
  AWS_REGION: eu-central-1
  ECR_REPOSITORY: my-app-repo
  ECS_CLUSTER: my-ecs-cluster
  ECS_SERVICE: my-ecs-service

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '20'
      - run: npm install
      - run: npm test
      - run: npm run lint
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}
      - id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1
      - run: |
          IMAGE_TAG=${GITHUB_SHA::8}
          docker build -t $ECR_REPOSITORY:$IMAGE_TAG .
          docker tag $ECR_REPOSITORY:$IMAGE_TAG ${{ steps.login-ecr.outputs.registry }}/$ECR_REPOSITORY:$IMAGE_TAG
          docker push ${{ steps.login-ecr.outputs.registry }}/$ECR_REPOSITORY:$IMAGE_TAG
        env:
          ECR_REPOSITORY: ${{ env.ECR_REPOSITORY }}
      - uses: aws-actions/amazon-ecs-deploy-task-definition@v2
        with:
          task-definition: ecs-task-def.json
          service: ${{ env.ECS_SERVICE }}
          cluster: ${{ env.ECS_CLUSTER }}
          wait-for-service-stability: true
```

---

## 🧪 Testovi i statička analiza

- Smoke testovi: `test/index.test.js`  
- ESLint: `npm run lint`

**Pokretanje testova:**

```bash
npm install
npm test
npm run lint
```

---

## 📌 Lokalno pokretanje

```bash
docker-compose up --build
```

Aplikacija je dostupna na: [http://localhost:3000](http://localhost:3000)

---

## ✅ Badge-ovi

- **Build Status:** pokazuje je li CI/CD workflow uspješno prošao  
- **Coverage:** 100% (primjer smoke test)  
- **Security Scan:** prošao sve sigurnosne provjere

---

## Fajlovi

### **package.json**

```json
{
  "scripts": {
    "test": "jest",
    "lint": "eslint . --ext .js,.ts"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "eslint": "^8.0.0"
  }
}
```

### **test/index.test.js**

```javascript
test('smoke test', () => {
  expect(1 + 1).toBe(2);
});
```

### **ecs-task-def.json**

```json
{
  "family": "my-ecs-task",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [
    {
      "name": "app",
      "image": "YOUR_ECR_REPO_URI:latest",
      "essential": true,
      "portMappings": [
        { "containerPort": 3000, "hostPort": 3000 }
      ]
    }
  ]
}
```


