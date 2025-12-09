# 🟥 Migração para AWS – Fase Migrate and Modernize

> Serviços para migrar servidores, aplicações, bancos de dados e dados

---

## 📌 Resumo

Nesta aula vamos revisar os serviços que ajudam na migração e modernização, ou seja, o momento em que você realmente move suas soluções para a AWS.

A explicação está dividida em duas partes:

1. ✅ **Serviços para migrar servidores, bancos de dados e aplicações**
2. ✅ **Serviços para migrar dados do data center para AWS**

---

## 🖥️ 1. AWS Application Migration Service (MGN)

Esse é o serviço principal da AWS para migrar servidores e aplicações com o mínimo de downtime, substituindo o antigo CloudEndure.

**Ele segue o modelo lift-and-shift:**

- ✅ Um agente é instalado no servidor de origem
- ✅ Ele replica continuamente os dados para instâncias virtuais na AWS
- ✅ O servidor original continua funcionando até o momento do cutover

Isso reduz custo, complexidade e risco de interrupção.

**Você pode migrar:**

- ✅ Servidores físicos
- ✅ Máquinas virtuais
- ✅ Ambientes Windows e Linux
- ✅ Aplicações como SAP, Oracle, SQL Server etc.

**Depois da migração**, ainda é possível refatorar ou replatformar para tirar proveito total da AWS.

---

### 🔧 Como funciona a configuração?

Antes de começar, você cria um **Replication Settings Template**, definindo:

- ✅ Sub-rede da área de staging
- ✅ Tipo da instância de replicação
- ✅ Tipo do volume EBS
- ✅ Criptografia do EBS
- ✅ Security Groups
- ✅ Configuração de roteamento
- ✅ Throttling de rede
- ✅ Tags de recursos

Depois disso, você instala o agente nos servidores.

**O processo no console passa pelo workflow:**

1. ✅ Source environment
2. ✅ Staging
3. ✅ Testing
4. ✅ Cutover

---

### 🧪 Testes e Cutover

Você pode configurar parâmetros de launch para cada servidor:

- ✅ Right-sizing automático
- ✅ Definição de IP privado
- ✅ Transferência de tags
- ✅ Tipo de licença do sistema operacional
- ✅ Inicialização automática ou manual após o cutover

O serviço permite testar individualmente ou em grupos antes de liberar para produção.

---

## 🗄️ 2. AWS Database Migration Service (DMS)

O **DMS** é projetado para migrar:

- ✅ Bancos relacionais
- ✅ Bancos NoSQL
- ✅ Data warehouses

Com mínimo downtime.

**Ele suporta:**

- ✅ Migrações homogêneas (ex.: Oracle → Oracle)
- ✅ Migrações heterogêneas (ex.: SQL Server → Aurora, MySQL → Redshift)
- ✅ Também permite consolidar vários bancos em um único destino, como Redshift para análise de dados em escala de petabytes

---

### 🔄 Quando o banco é compatível

Se source e target têm:

- ✅ Mesma estrutura
- ✅ Mesmos tipos de dados
- ✅ Código similar

A migração se torna praticamente um pipeline de uma etapa, simples e eficiente.

---

### 🔧 Quando o banco é diferente

Aí você precisa usar:

**AWS Schema Conversion Tool (SCT)**

Ela converte:

- ✅ Esquemas
- ✅ Tipos de dados
- ✅ Procedures
- ✅ Funções

Para o novo banco de destino.

---

### 🛠️ Como o DMS funciona tecnicamente?

1. ✅ Você define o source e o target
2. ✅ O DMS cria a **Replication Instance** (uma EC2 gerenciada)
3. ✅ Você cria **Endpoints** para origem e destino
4. ✅ Cria **Replication Tasks**, que movem os dados

**Existem três tipos de tarefas:**

- ✅ **Full Load** — migração completa inicial
- ✅ **Full Load + CDC** — migração completa + captura de alterações contínuas
- ✅ **CDC apenas** — apenas sincronização contínua

---

## 📦 3. AWS Service Catalog

O **AWS Service Catalog** é um serviço organizacional que permite padronizar e controlar a criação de recursos na AWS.

Ele é extremamente útil em ambientes corporativos ou grandes migrações.

---

### 🎯 O que ele faz?

- ✅ Permite que administradores criem portfólios de produtos pré-aprovados
- ✅ Permite que usuários finais criem recursos sem violar governança
- ✅ Garante uso consistente de padrões de arquitetura e segurança

**Um produto pode ser:**

- ✅ Uma instância EC2
- ✅ Um banco de dados
- ✅ Um stack inteiro criado por CloudFormation
- ✅ Um software de marketplace
- ✅ Uma aplicação multi-tier

Todos criados e publicados como "produtos".

---

### 📚 Portfólios

São coleções de produtos.

**Características:**

- ✅ Têm dono, descrição e regras
- ✅ Podem ser compartilhados entre contas
- ✅ Podem impor constraints, como:
  - ✅ Tamanho permitido de instâncias
  - ✅ Tipos de VPC
  - ✅ Permissões
  - ✅ Parâmetros fixos

Isso garante que desenvolvedores só criem coisas dentro das políticas da empresa.

---

## 📤 4. Migração de Dados

Agora que cobrimos servidores, bancos e aplicações, a segunda parte trata dos serviços de transferência de dados:

- ✅ **AWS DataSync** — transferência rápida de grandes volumes
- ✅ **AWS Transfer Family** — protocolos FTP/SFTP gerenciados
- ✅ **AWS Snow Family** — migração offline de petabytes
- ✅ **AWS Storage Gateway** — integração híbrida

**📘 Para detalhes completos sobre serviços de transferência de dados:**

- [AWS DataSync e Transfer Family](./datasync-transfer-family.md)
- [AWS Snow Family e Storage Gateway](./snow-storage-gateway.md)

---

## 📊 Resumo dos Serviços

| Serviço | Propósito | Quando Usar |
|---------|-----------|-------------|
| **Application Migration Service (MGN)** | Migrar servidores e aplicações | Lift & Shift de servidores físicos/VMs com mínimo downtime |
| **Database Migration Service (DMS)** | Migrar bancos de dados | Migração de bancos relacionais/NoSQL, homogênea ou heterogênea |
| **Service Catalog** | Padronizar recursos AWS | Ambientes corporativos, governança, grandes migrações |
| **DataSync** | Transferir grandes volumes de dados | Migração contínua de dados pela rede |
| **Transfer Family** | Protocolos FTP/SFTP gerenciados | Substituir servidores FTP antigos |
| **Snow Family** | Migração offline de dados | Petabytes/exabytes, rede lenta ou cara |
| **Storage Gateway** | Integração híbrida | Cenários híbridos durante migração |

---

## 🔄 Fluxo de Trabalho Completo

```
┌─────────────────────────────────────────────────────────┐
│         FASE MIGRATE & MODERNIZE                         │
│                                                          │
│  ┌──────────────────────────────────────┐               │
│  │   Migração de Servidores             │               │
│  │   • Application Migration Service    │               │
│  │   • Replicação contínua              │               │
│  │   • Testes e cutover                 │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  ┌──────────────────────────────────────┐               │
│  │   Migração de Bancos de Dados        │               │
│  │   • Database Migration Service        │               │
│  │   • Schema Conversion Tool (se necessário)            │
│  │   • Full Load + CDC                  │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  ┌──────────────────────────────────────┐               │
│  │   Migração de Dados                  │               │
│  │   • DataSync (rede)                  │               │
│  │   • Snow Family (offline)            │               │
│  │   • Transfer Family (FTP/SFTP)       │               │
│  │   • Storage Gateway (híbrido)         │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  ┌──────────────────────────────────────┐               │
│  │   Modernização e Governança           │               │
│  │   • Service Catalog                  │               │
│  │   • Padronização de recursos          │               │
│  │   • Refactoring/Replatforming        │               │
│  └──────────────────────────────────────┘               │
└──────────────────────────────────────────────────────────┘
```

---

## 🎯 Quando Usar Cada Serviço

### Application Migration Service (MGN)

**Use quando:**

- ✅ Precisa migrar servidores físicos ou VMs
- ✅ Quer mínimo downtime (replicação contínua)
- ✅ Está fazendo Lift & Shift
- ✅ Precisa testar antes do cutover
- ✅ Quer automação completa do processo

---

### Database Migration Service (DMS)

**Use quando:**

- ✅ Precisa migrar bancos relacionais (MySQL, PostgreSQL, Oracle, SQL Server)
- ✅ Quer migração com mínimo downtime (CDC)
- ✅ Precisa consolidar múltiplos bancos
- ✅ Está fazendo migração homogênea ou heterogênea
- ✅ Precisa sincronização contínua durante migração

**Combine com SCT quando:**

- ✅ Migração heterogênea (ex: Oracle → PostgreSQL)
- ✅ Precisa converter esquemas e procedures
- ✅ Tipos de dados são diferentes entre source e target

---

### Service Catalog

**Use quando:**

- ✅ Ambiente corporativo com múltiplas equipes
- ✅ Precisa garantir governança e compliance
- ✅ Quer padronizar recursos criados
- ✅ Precisa controlar tipos de instâncias/VPCs permitidos
- ✅ Quer self-service controlado para desenvolvedores
- ✅ Está fazendo grandes migrações com muitos recursos

---

## 🔗 Recursos Adicionais

- [AWS Application Migration Service (MGN)](https://aws.amazon.com/application-migration-service/)
- [AWS Database Migration Service (DMS)](https://aws.amazon.com/dms/)
- [AWS Schema Conversion Tool (SCT)](https://aws.amazon.com/dms/schema-conversion-tool/)
- [AWS Service Catalog](https://aws.amazon.com/servicecatalog/)
- [AWS DataSync](https://aws.amazon.com/datasync/)
- [AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/)
- [AWS Snow Family](https://aws.amazon.com/snow/)
- [AWS Storage Gateway](https://aws.amazon.com/storagegateway/)
- [Migration Hub](https://aws.amazon.com/migration-hub/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o papel da fase Migrate & Modernize
- [ ] Conhecer AWS Application Migration Service (MGN) e seu workflow
- [ ] Entender como funciona replicação contínua e cutover
- [ ] Conhecer AWS Database Migration Service (DMS)
- [ ] Entender tipos de tarefas DMS (Full Load, CDC)
- [ ] Conhecer AWS Schema Conversion Tool (SCT)
- [ ] Entender diferença entre migração homogênea e heterogênea
- [ ] Conhecer AWS Service Catalog e portfólios
- [ ] Entender como Service Catalog garante governança
- [ ] Saber quando usar cada serviço de migração
- [ ] Entender serviços de transferência de dados (DataSync, Snow, Transfer, Storage Gateway)

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#migrate` `#modernize` `#mgn` `#dms` `#service-catalog` `#datasync` `#snow` `#transfer-family`

---

## 🎯 Conclusão

A fase Migrate & Modernize é onde a migração realmente acontece. Com os serviços corretos (MGN para servidores, DMS para bancos, Service Catalog para governança), você pode migrar workloads com mínimo downtime e estabelecer padrões desde o início.

---

**Última atualização**: 📅 [DD/MM/YYYY]

