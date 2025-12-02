# ☁️ AWS - Amazon Web Services

> Estudos e anotações sobre AWS

---

## 📚 Índice

- [Fundamentos](#-fundamentos)
- [Serviços Core](#-serviços-core)
- [Computação](#-computação)
- [Armazenamento](#-armazenamento)
- [Banco de Dados & Analytics](#-banco-de-dados--analytics)
- [Developer Tools & DevOps](#-developer-tools--devops)
- [IoT & Edge](#-iot--edge)
- [Mobile, Frontend & Experiência do Usuário](#-mobile-frontend--experiência-do-usuário)
- [End-User Computing](#-end-user-computing)
- [Comunicação & Integração](#-comunicação--integração)
- [Observabilidade](#-observabilidade)
- [Machine Learning & AI](#-machine-learning--ai)
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

## 🗄️ Banco de Dados & Analytics

- [ ] RDS (Relational Database Service)
- [ ] DynamoDB
- [ ] Redshift
- [ ] ElastiCache
- [ ] DocumentDB
- [ ] Neptune
- [ ] Athena
  - [ ] [Notas Detalhadas](./athena.md)
- [ ] Glue
  - [ ] [Notas Detalhadas](./glue.md)
- [ ] QuickSight
  - [ ] [Notas Detalhadas](./quicksight.md)

---

## 🧰 Developer Tools & DevOps

- [ ] AWS CLI
  - [ ] [Notas Detalhadas](./aws-cli.md)
- [ ] CloudShell
  - [ ] [Notas Detalhadas](./cloudshell.md)
- [ ] Cloud9
  - [ ] [Notas Detalhadas](./cloud9.md)
- [ ] CodeCommit
  - [ ] [Notas Detalhadas](./codecommit.md)
- [ ] CodeBuild
  - [ ] [Notas Detalhadas](./codebuild.md)
- [ ] CodeDeploy
  - [ ] [Notas Detalhadas](./codedeploy.md)
- [ ] CodePipeline
  - [ ] [Notas Detalhadas](./codepipeline.md)
- [ ] CodeStar
  - [ ] [Notas Detalhadas](./codestar.md)

---

## 📡 IoT & Edge

- [ ] IoT Core / Greengrass
  - [ ] [Notas Detalhadas](./iot-core-greengrass.md)

---

## 📱 Mobile, Frontend & Experiência do Usuário

- [ ] Amplify
  - [ ] [Notas Detalhadas](./amplify.md)
- [ ] AppSync
  - [ ] [Notas Detalhadas](./appsync.md)
- [ ] Device Farm
  - [ ] [Notas Detalhadas](./device-farm.md)

---

## 🖥️ End-User Computing

- [ ] AppStream 2.0 / WorkSpaces
  - [ ] [Notas Detalhadas](./appstream-workspaces.md)

---

## 📨 Comunicação & Integração

- [ ] SES (Simple Email Service)
  - [ ] [Notas Detalhadas](./ses.md)
- [ ] AppConfig
  - [ ] [Notas Detalhadas](./appconfig.md)

---

## 🧪 Observabilidade

- [ ] X-Ray
  - [ ] [Notas Detalhadas](./x-ray.md)

---

## 🤖 Machine Learning & AI

- [ ] Visão Geral dos Serviços ML
  - [ ] [Notas Detalhadas](./ml-services-overview.md)
- [ ] SageMaker
  - [ ] [Notas Detalhadas](./sagemaker.md)
- [ ] Rekognition
- [ ] Transcribe
- [ ] Lex
  - [ ] [Notas Detalhadas](./lex.md)
- [ ] Translate
- [ ] Comprehend
- [ ] Textract
- [ ] Forecast
- [ ] Personalize
- [ ] Kendra
- [ ] Polly

---

## 🌐 Networking

- [ ] VPC
- [ ] CloudFront
- [ ] Route 53
- [ ] API Gateway
- [ ] Global Accelerator
  - [ ] [Notas Detalhadas](./global-accelerator.md)
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

