

#  WordPress com Docker e DevSecOps na AWS

Este projeto realiza o deploy automatizado de uma aplicação WordPress em contêineres Docker, utilizando serviços da AWS para garantir alta disponibilidade, escalabilidade e segurança. A infraestrutura é provisionada com Terraform e o pipeline CI/CD é gerenciado via GitHub Actions, incluindo práticas de DevSecOps.

---

##  Objetivos

- Deploy de WordPress em contêineres Docker
- Infraestrutura escalável e altamente disponível na AWS
- Banco de dados gerenciado com Amazon RDS
- Armazenamento persistente com Amazon EFS
- Balanceamento de carga com Application Load Balancer (ALB)
- Auto Scaling com políticas dinâmicas
- Pipeline CI/CD com GitHub Actions
- Escaneamento de segurança com Trivy e Checkov
- Monitoramento com Amazon CloudWatch

---

##  Arquitetura

Diagrama da Arquitetura

### Componentes AWS Utilizados:

- **VPC** com sub-redes públicas e privadas
- **EC2** com Docker para WordPress
- **Amazon RDS** (MySQL)
- **Amazon EFS** para arquivos estáticos
- **Application Load Balancer (ALB)**
- **Auto Scaling Group (ASG)**
- **Security Groups**
- **Amazon CloudWatch** + **SNS**
- **IAM Roles** para acesso seguro

---

##  Contêineres

- `Dockerfile` personalizado para WordPress
- Persistência de dados com EFS
- Imagens hospedadas no Amazon ECR

---

##  DevSecOps

- **Trivy**: escaneamento de vulnerabilidades em imagens Docker
- **Checkov**: análise de segurança em código Terraform
- **GitHub Actions**: pipeline CI/CD com etapas de build, teste, push e deploy

---

##  CI/CD com GitHub Actions

```yaml
name: Deploy WordPress

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Build Docker Image
        run: docker build -t wordpress-app .

      - name: Scan Image with Trivy
        run: trivy image wordpress-app

      - name: Push to ECR
        run: |
          # comandos para autenticar e enviar imagem

      - name: Deploy to EC2
        run: |
          # comandos para SSH e deploy
