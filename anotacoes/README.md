# 📝 Anotações Rápidas

> Anotações, dicas e truques rápidos sobre Cloud

---

## 💡 Dicas Gerais

### Boas Práticas
- Sempre use tags para organizar recursos
- Configure billing alerts desde o início
- Use Infrastructure as Code (IaC) para reprodutibilidade
- Implemente least privilege no IAM
- Habilite logging e monitoring desde o início

### Economia de Custos
- Use reserved instances para cargas de trabalho previsíveis
- Configure lifecycle policies para storage
- Remova recursos não utilizados regularmente
- Use spot instances quando possível
- Monitore custos com ferramentas nativas

---

## 🚀 Comandos Rápidos

### AWS CLI
```bash
# Listar todas as regiões
aws ec2 describe-regions --query 'Regions[].RegionName' --output text

# Listar instâncias EC2
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]' --output table

# Copiar arquivo para S3
aws s3 cp arquivo.txt s3://meu-bucket/
```

### Azure CLI
```bash
# Listar todas as regiões
az account list-locations --query '[].name' --output table

# Listar VMs
az vm list --output table

# Criar storage account
az storage account create --name mystorageaccount --resource-group myResourceGroup
```

### gcloud CLI
```bash
# Listar todas as regiões
gcloud compute regions list

# Listar instâncias
gcloud compute instances list

# Criar bucket
gsutil mb gs://meu-bucket
```

---

## 📚 Glossário Rápido

| Termo | Significado |
|-------|-------------|
| **IaaS** | Infrastructure as a Service |
| **PaaS** | Platform as a Service |
| **SaaS** | Software as a Service |
| **VPC** | Virtual Private Cloud |
| **CDN** | Content Delivery Network |
| **IAM** | Identity and Access Management |
| **CI/CD** | Continuous Integration/Continuous Deployment |
| **IaC** | Infrastructure as Code |
| **AZ** | Availability Zone |
| **SLA** | Service Level Agreement |

---

## ⚠️ Erros Comuns e Soluções

### AWS
- **Erro**: "Access Denied"
  - **Solução**: Verificar políticas IAM e permissões

- **Erro**: "Instance limit exceeded"
  - **Solução**: Solicitar aumento de limite na AWS Support

### Azure
- **Erro**: "Resource group not found"
  - **Solução**: Verificar nome do resource group e região

- **Erro**: "Quota exceeded"
  - **Solução**: Verificar quotas no Azure Portal

### GCP
- **Erro**: "Permission denied"
  - **Solução**: Verificar roles IAM do usuário

- **Erro**: "Quota exceeded"
  - **Solução**: Solicitar aumento de quota no GCP Console

---

## 🔗 Links Úteis

### Calculadoras de Custo
- [AWS Pricing Calculator](https://calculator.aws/)
- [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)
- [GCP Pricing Calculator](https://cloud.google.com/products/calculator)

### Status de Serviços
- [AWS Status](https://status.aws.amazon.com/)
- [Azure Status](https://status.azure.com/)
- [GCP Status](https://status.cloud.google.com/)

---

## 📅 Checklist de Deploy

Antes de fazer deploy em produção:

- [ ] Revisão de segurança (IAM, firewall, encryption)
- [ ] Configuração de backup
- [ ] Configuração de monitoring e alertas
- [ ] Documentação atualizada
- [ ] Testes de disaster recovery
- [ ] Revisão de custos
- [ ] Tags aplicadas a todos os recursos
- [ ] Logging habilitado

---

**Última atualização**: 📅 [DD/MM/YYYY]

