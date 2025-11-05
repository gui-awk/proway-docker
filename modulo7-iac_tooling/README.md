# Jewelry App

Aplicação Vue.js para exibição de joias com deploy automatizado na AWS via Terraform.

## Pré-requisitos

* Node.js 18+
* Docker
* Terraform
* AWS CLI (ou AWS CloudShell)

## Execução Local

### Desenvolvimento

```bash
npm install
npm run dev
```

Acesse: [http://localhost:5173](http://localhost:5173)

### Docker Local

```bash
make docker-run
# ou
docker build -t jewelry-app .
docker run -p 8080:80 jewelry-app
```

Acesse: [http://localhost:8080](http://localhost:8080)

## Deploy na AWS

### Configuração Inicial

```bash
# Se não estiver no CloudShell
aws configure
# informe Access Key, Secret, região (ex.: us-east-1) e formato (json)
```

### Deploy Automatizado

```bash
# Build + infraestrutura + aplicação
make aws-deploy
```

### Deploy Manual

```bash
# 1) Inicializar Terraform
make init

# 2) Planejar mudanças
make plan

# 3) Aplicar infraestrutura
make apply

# 4) Build e deploy da aplicação (executado via user_data na VM)
make deploy
```

## Comandos Úteis

```bash
# Build da aplicação
make build

# Limpar artefatos temporários
make clean

# Destruir infraestrutura na AWS
make aws-destroy
```

## Estrutura do Projeto

```
├── src/           # Código-fonte Vue.js
├── main.tf        # Configuração Terraform (AWS)
├── Dockerfile     # Container da aplicação
├── Makefile       # Comandos automatizados
└── deploy.sh      # Script de deploy
```

## Infraestrutura AWS

O Terraform provisiona/usa:

* VPC existente: vpc-modulo9
* Subnet existente na VPC
* Security Group existente: jewelry-nsg
* Elastic IP (EIP), Network Interface (ENI) e associação
* EC2 Ubuntu com Docker via user_data
* Key Pair gerado pelo Terraform (tls_private_key + aws_key_pair)
* Outputs: IP público e URL da aplicação

A aplicação roda na porta 8080 da VM. Use o output `app_url` após `terraform apply`.
