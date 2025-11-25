# ☁️ AWS - Amazon Web Services

> Estudos e anotações sobre AWS

---

## 📚 Índice

- [Fundamentos](#-fundamentos)
- [Serviços Core](#-serviços-core)
- [Computação](#-computação)
- [Armazenamento](#-armazenamento)
- [Banco de Dados](#-banco-de-dados)
- [Networking](#-networking)
- [Segurança](#-segurança)
- [Certificações](#-certificações)

---

## 🎯 Fundamentos

### Conceitos Básicos
- [ ] Regiões e Availability Zones
- [ ] Conta AWS e IAM
- [ ] Billing e Cost Management
- [ ] AWS Well-Architected Framework

---

## 🛠️ Serviços Core

### EC2 (Elastic Compute Cloud)
- [ ] Instâncias e tipos
- [ ] AMIs (Amazon Machine Images)
- [ ] Security Groups
- [ ] Elastic IPs
- [ ] [Notas Detalhadas](./ec2.md)

### S3 (Simple Storage Service)
- [ ] Buckets e objetos
- [ ] Classes de armazenamento
- [ ] Versionamento
- [ ] Lifecycle policies
- [ ] [Notas Detalhadas](./s3.md)

### VPC (Virtual Private Cloud)
- [ ] Subnets
- [ ] Route Tables
- [ ] Internet Gateway
- [ ] NAT Gateway
- [ ] [Notas Detalhadas](./vpc.md)

---

## 💻 Computação

- [ ] EC2
- [ ] Lambda
- [ ] ECS (Elastic Container Service)
- [ ] EKS (Elastic Kubernetes Service)
- [ ] Fargate
- [ ] Batch
- [ ] Lightsail

---

## 💾 Armazenamento

- [ ] S3
- [ ] EBS (Elastic Block Store)
- [ ] EFS (Elastic File System)
- [ ] Glacier
- [ ] Storage Gateway

---

## 🗄️ Banco de Dados

- [ ] RDS (Relational Database Service)
- [ ] DynamoDB
- [ ] Redshift
- [ ] ElastiCache
- [ ] DocumentDB
- [ ] Neptune

---

## 🌐 Networking

- [ ] VPC
- [ ] CloudFront
- [ ] Route 53
- [ ] API Gateway
- [ ] Direct Connect
- [ ] VPN

---

## 🔒 Segurança

- [ ] IAM (Identity and Access Management)
- [ ] KMS (Key Management Service)
- [ ] Secrets Manager
- [ ] WAF (Web Application Firewall)
- [ ] Shield
- [ ] GuardDuty
- [ ] CloudTrail
- [ ] Config

---

## 🎓 Certificações AWS

### AWS Certified Cloud Practitioner
- [ ] Fundamentos AWS
- [ ] Billing e pricing
- [ ] [Plano de Estudos](./certificacoes/cloud-practitioner.md)

### AWS Certified Solutions Architect - Associate
- [ ] Arquitetura de soluções
- [ ] Design de sistemas escaláveis
- [ ] [Plano de Estudos](./certificacoes/solutions-architect-associate.md)

---

## 📝 Comandos Úteis

### AWS CLI
```bash
# Configuração inicial
aws configure

# Listar buckets S3
aws s3 ls

# Criar instância EC2
aws ec2 run-instances --image-id ami-xxx --instance-type t2.micro
```

---

## 🔗 Recursos

- [Documentação AWS](https://docs.aws.amazon.com/)
- [AWS Training](https://aws.amazon.com/training/)
- [AWS Whitepapers](https://aws.amazon.com/whitepapers/)

---

**Última atualização**: 📅 [DD/MM/YYYY]

