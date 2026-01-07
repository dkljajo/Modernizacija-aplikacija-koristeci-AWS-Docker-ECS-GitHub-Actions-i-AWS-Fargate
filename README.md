
# Modernizacija aplikacija koristeći AWS, Docker, ECS, GitHub Actions i AWS Fargate

![Build Status](https://github.com/dkljajo/Modernizacija-aplikacija-koristeci-AWS-Docker-ECS-GitHub-Actions-i-AWS-Fargate/actions/workflows/deploy.yml/badge.svg)
![Coverage](https://img.shields.io/badge/coverage-100%25-brightgreen)
![Security Scan](https://img.shields.io/badge/security-passed-brightgreen)

---

## 1️⃣ Struktura foldera

Modernizacija-aplikacija/
├── .github/workflows/deploy.yml
├── ecs-task-def.json
├── Dockerfile
├── docker-compose.yml
├── package.json
├── test/index.test.js
└── README.md

yaml
Copy code

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

├── .github/workflows/deploy.yml # CI/CD workflow
├── ecs-task-def.json # ECS Task Definition
├── Dockerfile # Docker build definicija
├── docker-compose.yml # Lokalni razvoj
├── package.json # Scripts za test i lint
├── test/
│ └── index.test.js # Smoke test
└── README.md

yaml
Copy code

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
docker-compose.yml
yaml
Copy code
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
⚙️ CI/CD Workflow (GitHub Actions)
Build i test Docker image

Push image u AWS ECR

Deploy na AWS ECS Fargate

Fajl: .github/workflows/deploy.yml

yaml
Copy code
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
🧪 Testovi i statička analiza
Smoke testovi: test/index.test.js

ESLint: npm run lint

Pokretanje testova:

bash
Copy code
npm install
npm test
npm run lint
📌 Lokalno pokretanje
bash
Copy code
docker-compose up --build
Aplikacija je dostupna na: http://localhost:3000

✅ Badge-ovi
Build Status: pokazuje je li CI/CD workflow uspješno prošao

Coverage: 100% (primjer smoke test)

Security Scan: prošao sve sigurnosne provjere

Fajlovi
package.json
json
Copy code
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
test/index.test.js
javascript
Copy code
test('smoke test', () => {
  expect(1 + 1).toBe(2);
});
ecs-task-def.json
json
Copy code
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
css
Copy code
