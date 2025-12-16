# Modernizacija aplikacija koristeći AWS, Docker, Amazon ECS, GitHub Actions i AWS Fargate

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Docker](https://img.shields.io/badge/Docker-Container-blue)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-green)
![ECS](https://img.shields.io/badge/Amazon-ECS-yellow)

## 📌 Pregled projekta

Ovaj projekt demonstrira **modernizaciju aplikacije** korištenjem kontejnerskih tehnologija i AWS cloud servisa. Fokus je na izgradnji, sigurnom upravljanju i automatiziranom deploymentu Docker kontejnera koristeći **Amazon ECS s AWS Fargate-om** i **GitHub Actions CI/CD pipeline**.

Projekt je namijenjen kao **edukativni i praktični primjer** za DevOps i Cloud inženjere koji žele razumjeti kompletan lifecycle moderne cloud-native aplikacije.

---

## 🏗️ Arhitektura

![Architecture Diagram](docs/images/architecture.png)

**Arhitekturni tok:**

1. Aplikacija se pakira u Docker image
2. Image se push-a u Amazon ECR
3. Amazon ECS (Fargate) pokreće kontejnere
4. Tajne se dohvaćaju iz AWS Secrets Managera
5. CI/CD pipeline upravlja build & deploy procesom

---

## 🚀 Funkcionalnosti

* ✅ Postavljanje aplikacije pomoću **Docker Compose ECS integratora**
* ✅ Izgradnja vlastite Docker slike koristeći **Docker BuildKit**
* ✅ Deployment slike na **Amazon ECS**
* ✅ Sigurno upravljanje tajnama putem **AWS Secrets Managera**
* ✅ Automatizirani **CI/CD pipeline** pomoću Amazon ECS GitHub Actions dodatka
* ✅ Pokretanje kontejnera na **AWS Fargate** (serverless)

---

## 🧰 Tehnologije

| Tehnologija         | Opis                             |
| ------------------- | -------------------------------- |
| Docker              | Kontejnerizacija aplikacije      |
| Docker Compose      | Lokalni razvoj i ECS integracija |
| Amazon ECS          | Orkestracija kontejnera          |
| AWS Fargate         | Serverless runtime za kontejnere |
| Amazon ECR          | Registry za Docker slike         |
| AWS Secrets Manager | Sigurno čuvanje tajni            |
| GitHub Actions      | CI/CD automatizacija             |

---

## 🐳 Docker & Build

Primjer izgradnje Docker slike koristeći BuildKit:

```bash
DOCKER_BUILDKIT=1 docker build -t modern-app .
```

Push slike u Amazon ECR:

```bash
docker tag modern-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/modern-app:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/modern-app:latest
```

---

## 🔐 Sigurnost kontejnera

Projekt koristi **AWS Secrets Manager** za:

* lozinke
* API ključeve
* osjetljive konfiguracijske vrijednosti

Tajne se injektiraju direktno u ECS task definiciju bez hard-kodiranja vrijednosti u kod ili Docker image.

---

## 🔄 CI/CD – GitHub Actions

![CI/CD Pipeline](docs/images/cicd.png)

CI/CD workflow:

1. Push na `main` branch
2. Build Docker image
3. Push u Amazon ECR
4. Deploy na Amazon ECS (Fargate)

Primjer korištenja **Amazon ECS GitHub Action** omogućava siguran i ponovljiv deployment.

---

## ▶️ Pokretanje aplikacije

Lokalno (Docker Compose):

```bash
docker compose up --build
```

Cloud (ECS):

* Automatski pokrenuto kroz GitHub Actions pipeline

---

## 📂 Struktura projekta

```text
.
├── .github/workflows/   # GitHub Actions CI/CD
├── docs/images/         # Dijagrami i slike
├── Dockerfile
├── docker-compose.yml
├── README.md
```

---

## 🎯 Cilj projekta

Cilj je demonstrirati **best-practice pristup modernizaciji aplikacija** koristeći:

* Infrastructure as Code
* Container-first pristup
* Sigurnost by design
* Automatizirani deployment

---

## 📚 Reference

* AWS ECS Documentation
* Docker Documentation
* GitHub Actions
* AWS Fargate

---

## 👨‍💻 Autor

Projekt razvijen u edukativne svrhe za demonstraciju modernih DevOps i Cloud praksi.
