

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
```
---
# Etapa 01:

## Clonar o Repositório no WSL:

```
git clone https://github.com/seu-usuario/wordpress-devsecops-aws.git 

cd wordpress-devsecops-aws 

code .  
```
## Criar Estrutura de Pastas

```
mkdir -p infra/terraform 

mkdir -p app/wp-content 

mkdir -p .github/workflows 

mkdir docs 

touch README.md 
```
### Explicação:

infra/terraform: onde ficará o código de infraestrutura.

app/wp-content: onde ficará o código da aplicação WordPress (plugins, temas, etc.).

.github/workflows: onde ficará o pipeline CI/CD com GitHub Actions. 

docs: para diagramas, imagens e documentação extra.

touch README.md: cria o arquivo de documentação principal.
---
# Etapa 2 – Deploy Manual do WordPress com Docker

## Criar o Dockerfile: 
Objetivo: Definir como o contêiner do WordPress será construído.No diretório app/, crie um arquivo chamado Dockerfile com o seguinte conteúdo:
```
FROM wordpress:latest

# Porta padrão do WordPress
EXPOSE 80
```
## Criar o docker-compose.yml
No mesmo diretório app/, crie um arquivo docker-compose.yml:
```
version: '3.8'

services:
  wordpress:
    build: .
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - ./wp-content:/var/www/html/wp-content

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

