# 📤 AWS DataSync e AWS Transfer Family — Explicação Completa

> Serviços para transferência de dados na migração para AWS

---

## 📌 Resumo

Dois serviços essenciais para transferência de dados durante a migração para AWS:

- ✅ **AWS DataSync** — transferência rápida e segura de grandes volumes
- ✅ **AWS Transfer Family** — protocolos FTP/SFTP gerenciados para integrações existentes

---

## 🚀 AWS DataSync

Se você já trabalhou com diferentes serviços de armazenamento da AWS, provavelmente já encontrou o **AWS DataSync**.

Ele é um serviço que permite transferir dados de forma fácil, rápida e segura entre:

- ✅ Seu data center on-premises → AWS
- ✅ Ou entre dois serviços de armazenamento dentro da AWS

**Isso o torna perfeito para:**

- ✅ Migração de dados
- ✅ Sincronização contínua
- ✅ Substituição de storage legacy
- ✅ Movimentação de grandes volumes entre serviços diferentes

---

### 📦 Serviços compatíveis

O DataSync suporta dados armazenados em:

- ✅ **NFS** (Network File System)
- ✅ **SMB** (Server Message Block)
- ✅ Storage de objetos auto-gerenciado

**E também pode transferir dados diretamente para:**

- ✅ Amazon S3
- ✅ Amazon EFS
- ✅ Amazon FSx for Windows File Server
- ✅ AWS Snowcone

---

### 🔗 Conectividade usando VPC Endpoints

Quando você realiza transferências — seja do on-premises para AWS ou dentro da AWS — o DataSync pode usar **AWS VPC Endpoints**, obtendo:

- ✅ Alta largura de banda
- ✅ Baixa latência
- ✅ Tráfego totalmente interno na rede da AWS
- ✅ Gestão simplificada

---

### ⚡ Alto desempenho

O DataSync usa:

- ✅ Um protocolo de transferência próprio, otimizado
- ✅ Arquitetura paralela e multithreaded

**Isso permite que cada tarefa do DataSync atinja até 10 Gbps de throughput** entre seu data center e a AWS — algo essencial quando migrando grandes volumes.

---

### 🔒 Segurança (duas camadas)

#### 1. Criptografia

- ✅ **Em trânsito**: usando TLS
- ✅ **Em repouso**: usando os mecanismos nativos do serviço de destino:
  - ✅ Amazon S3 (criptografia padrão)
  - ✅ EFS e FSx (criptografia nativa)

#### 2. Validação dos dados

O DataSync valida tudo que foi transferido, garantindo que:

- ✅ Nada foi corrompido no caminho
- ✅ O dado no destino é idêntico ao original

Essa verificação adicional é crítica em migrações corporativas.

---

## 📁 AWS Transfer Family

O **AWS Transfer Family** é um serviço totalmente gerenciado que permite transferir arquivos de forma segura para dentro ou para fora da AWS, utilizando Amazon S3 ou Amazon EFS como destino.

**A grande vantagem é que você não precisa gerenciar nenhum servidor ou infraestrutura de transferência de arquivos** — a AWS cuida de tudo para você.

A AWS provisiona automaticamente todos os recursos necessários para operar seus endpoints do Transfer Family, oferecendo:

- ✅ **Alta disponibilidade**, já que os recursos funcionam em múltiplas zonas de disponibilidade da região
- ✅ **Escalabilidade automática**, garantindo desempenho constante mesmo em períodos de alta demanda

Isso torna o serviço ideal tanto para trocas B2B recorrentes quanto para transferências eventuais entre empresas.

---

### 📡 Protocolos suportados

O AWS Transfer Family permite criar endpoints usando:

- ✅ **FTP** — protocolo tradicional (não criptografado)
- ✅ **FTPS** — FTP com TLS
- ✅ **SFTP** — transferência via SSH
- ✅ **AS2** — protocolo corporativo comum para EDI e integração B2B

---

### ⭐ Características importantes

#### 1. Totalmente gerenciado

Você não precisa manter servidores FTP/SFTP.

**A AWS cuida de tudo:**

- ✅ Alta disponibilidade (opera em múltiplas AZs)
- ✅ Escalabilidade automática
- ✅ Segurança
- ✅ Logs opcionais com CloudWatch

#### 2. Autenticação e integrações

O Transfer Family funciona com provedores de autenticação como:

- ✅ **Microsoft Active Directory**
- ✅ **LDAP**
- ✅ **Amazon Cognito**

#### 3. Workflows Gerenciados (MFTW)

O Transfer Family permite executar ações automáticas após o upload do arquivo, como:

- ✅ Varredura antivírus
- ✅ Aplicação de tags
- ✅ Descriptografia
- ✅ Outras tarefas de processamento pós-upload

Isso permite que você trate arquivos "em trânsito", sem código adicional.

---

### 🔧 Como começar a usar o AWS Transfer Family?

#### 1. Configure seu destino

Pode ser:

- ✅ Bucket S3
- ✅ File System EFS

#### 2. Configure autenticação

Escolha um dos provedores suportados:

- ✅ Microsoft Active Directory
- ✅ LDAP
- ✅ Amazon Cognito
- ✅ IAM (para acesso direto)

#### 3. Conceda permissões via IAM

O Transfer Server precisa acessar seu storage.

---

### 💰 Preços

A cobrança funciona da seguinte forma:

- ✅ **Uma taxa por hora** para cada protocolo habilitado no endpoint
- ✅ **Uma taxa por gigabyte transferido** nos protocolos FTP, FTPS e SFTP
- ✅ **Uma taxa fixa por mensagem** para endpoints AS2

---

## 📊 Comparação: DataSync vs Transfer Family

| Aspecto | AWS DataSync | AWS Transfer Family |
|---------|-------------|---------------------|
| **Propósito** | Migração e sincronização de grandes volumes | Protocolos FTP/SFTP para integrações existentes |
| **Protocolos** | NFS, SMB, objetos | SFTP, FTPS, FTP, AS2 |
| **Destinos** | S3, EFS, FSx, Snowcone | S3, EFS |
| **Performance** | Até 10 Gbps | Escalável automaticamente |
| **Validação** | Validação automática de integridade | Via protocolos padrão |
| **Uso típico** | Migração de dados, sincronização contínua | Substituir servidores FTP legados |
| **Gerenciamento** | Agente no on-premises | Totalmente gerenciado |

---

## 🎯 Quando Usar Cada Serviço

### AWS DataSync

**Use quando:**

- ✅ Precisa migrar grandes volumes de dados (TB/PB)
- ✅ Quer sincronização contínua entre on-premises e AWS
- ✅ Precisa alta performance (até 10 Gbps)
- ✅ Tem dados em NFS ou SMB
- ✅ Quer validação automática de integridade
- ✅ Precisa mover dados entre serviços AWS (S3 → EFS, etc.)
- ✅ Está substituindo storage legacy

**Casos de uso:**

- ✅ Migração inicial de dados para AWS
- ✅ Backup contínuo para S3
- ✅ Sincronização de file shares para EFS
- ✅ Migração de dados para Snowcone

---

### AWS Transfer Family

**Use quando:**

- ✅ Precisa manter protocolos FTP/SFTP existentes
- ✅ Tem integrações B2B que usam FTP/SFTP
- ✅ Quer substituir servidores FTP legados
- ✅ Precisa de protocolo AS2 para EDI
- ✅ Quer eliminar manutenção de servidores FTP
- ✅ Precisa de alta disponibilidade para transferências de arquivos
- ✅ Quer processar arquivos durante transferência (workflows)

**Casos de uso:**

- ✅ Substituir servidor FTP on-premises
- ✅ Integrações B2B via SFTP/FTPS
- ✅ EDI via AS2
- ✅ Migração gradual de clientes FTP para AWS
- ✅ Processamento automático de arquivos recebidos

---

## 🔄 Fluxo de Trabalho Típico

### Com DataSync

```
┌─────────────────────────────────────────────────────────┐
│  On-Premises (NFS/SMB)                                  │
│         │                                                │
│         │ DataSync Agent                                 │
│         │ (instalado no on-premises)                     │
│         ▼                                                │
│  ┌──────────────────────────────────────┐             │
│  │   AWS DataSync                         │             │
│  │   • Protocolo otimizado               │             │
│  │   • Até 10 Gbps                       │             │
│  │   • Validação automática               │             │
│  └──────────────┬───────────────────────┘             │
│                 │                                        │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐             │
│  │   Destino AWS                         │             │
│  │   • S3 / EFS / FSx                    │             │
│  └───────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────┘
```

### Com Transfer Family

```
┌─────────────────────────────────────────────────────────┐
│  Cliente/Sistema Externo                                 │
│  (usando SFTP/FTPS/FTP)                                  │
│         │                                                │
│         │ Protocolo padrão                               │
│         ▼                                                │
│  ┌──────────────────────────────────────┐             │
│  │   AWS Transfer Family                 │             │
│  │   • Totalmente gerenciado             │             │
│  │   • Alta disponibilidade              │             │
│  │   • Workflows (opcional)              │             │
│  └──────────────┬───────────────────────┘             │
│                 │                                        │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐             │
│  │   Destino AWS                         │             │
│  │   • S3 / EFS                          │             │
│  └───────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Recursos Adicionais

- [AWS DataSync](https://aws.amazon.com/datasync/)
- [AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/)
- [DataSync Documentation](https://docs.aws.amazon.com/datasync/)
- [Transfer Family Documentation](https://docs.aws.amazon.com/transfer/)
- [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html)
- [DataSync Best Practices](https://docs.aws.amazon.com/datasync/latest/userguide/best-practices.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender AWS DataSync e seus casos de uso
- [ ] Conhecer protocolos suportados pelo DataSync (NFS, SMB)
- [ ] Entender destinos suportados (S3, EFS, FSx, Snowcone)
- [ ] Compreender performance do DataSync (até 10 Gbps)
- [ ] Entender segurança do DataSync (criptografia e validação)
- [ ] Conhecer AWS Transfer Family e seus protocolos
- [ ] Entender quando usar Transfer Family vs DataSync
- [ ] Compreender características do Transfer Family (gerenciado, workflows)
- [ ] Saber quando usar cada serviço na migração
- [ ] Entender integração com VPC Endpoints

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#datasync` `#transfer-family` `#data-transfer` `#ftp` `#sftp` `#nfs` `#smb`

---

## 🎯 Conclusão

**AWS DataSync** e **AWS Transfer Family** são serviços complementares para transferência de dados na migração:

- ✅ **DataSync**: Para migração e sincronização de grandes volumes com alta performance
- ✅ **Transfer Family**: Para manter protocolos FTP/SFTP existentes sem quebrar integrações

Ambos são essenciais em diferentes cenários de migração para AWS.

---

**Última atualização**: 📅 [DD/MM/YYYY]

