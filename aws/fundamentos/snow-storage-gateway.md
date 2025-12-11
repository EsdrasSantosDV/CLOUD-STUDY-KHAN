# ❄️ AWS Snow Family e AWS Storage Gateway — Explicação Completa

> Dispositivos físicos e gateway virtual para migração e integração de dados

---

## 📌 Resumo

Dois serviços complementares para migração e integração de dados:

- ✅ **AWS Snow Family** — dispositivos físicos para transferência offline de grandes volumes
- ✅ **AWS Storage Gateway** — appliance virtual para integração híbrida on-premises ↔ AWS

---

## ❄️ AWS Snow Family

### 1. O que é a Snow Family?

A **AWS Snow Family** é uma coleção de dispositivos físicos enviados pela AWS para você usar fora da nuvem — no data center, no edge, em campo ou até em locais remotos sem internet.

**Esses dispositivos servem para:**

- ✅ Transferir dados para dentro da AWS (ex.: do seu data center → Amazon S3)
- ✅ Transferir dados para fora da AWS (ex.: S3 → seu data center)
- ✅ Rodar workloads no edge, usando EC2 adaptado para Snow

É um conceito totalmente diferente do usual na AWS, onde tudo é virtual e acessado via CLI, SDK ou console.

**Aqui, a AWS te envia uma máquina física, você transfere os dados nela, devolve para a AWS, e os dados são carregados no S3.**

---

### 2. Por que isso existe?

Porque nem sempre faz sentido transferir dados via rede.

**Exemplo real:**

Transferir 1 PB via Direct Connect 1 Gbps levaria:

- ⚠️ **104 dias** e ainda geraria custos de tráfego.

Ou seja, para volumes massivos, a solução prática é transportar fisicamente.

---

### 3. Os dispositivos da Snow Family

A Snow Family é composta por:

---

#### 🔹 Snowcone

**O menor dispositivo (portátil)**

- ✅ Capacidade de alguns TB
- ✅ Projetado para edge e locais remotos
- ✅ Pode usar bateria externa
- ✅ Permite rodar EC2 no edge

---

#### 🔹 Snowball (3 versões)

Todos têm o mesmo tamanho físico, mas com propósitos diferentes:

- ✅ **Compute Optimized** — processamento de dados
- ✅ **Compute Optimized com GPU** — workloads com GPU
- ✅ **Storage Optimized** — máxima capacidade de armazenamento

**Usos:**

- ✅ Migração de dezenas a centenas de TB
- ✅ Processamento de dados em campo
- ✅ Análise de dados antes de enviar para a nuvem
- ✅ Execução de EC2 e Lambda no edge

---

#### 🔹 Snowmobile

**Um caminhão-contêiner da AWS**

- ✅ Capacidade de até 100 PB
- ✅ Para migrações de grande porte em data centers corporativos
- ✅ Segurança física extrema (rastreável, blindado, etc.)

---

## 📦 AWS Storage Gateway — Visão Geral e Cenários Práticos

### 1. Problema Inicial da Company A

**Cenário clássico de empresa meio "presa" no on-prem:**

- ✅ Servidores acessando dados críticos em arrays de storage tradicionais

**Limitações claras:**

- ⚠️ **Escala**: hardware tem teto físico, financeiro e de equipe
- ⚠️ **Estratégia futura**: querem reduzir datacenter e ir mais para cloud, então investir pesado em storage local não fecha a conta
- ⚠️ **Acesso centralizado**: várias filiais pelo país, difícil ter um ponto único de dados com storage tradicional

**Conclusão**: precisam de uma solução híbrida, não um "big bang" para cloud.

---

### 2. Onde Entra o AWS Storage Gateway

**Storage Gateway é o "tradutor" entre o mundo on-prem e os serviços de storage da AWS.**

**Arquitetura fica assim:**

**Continuam existindo:**

- ✅ Servidores on-prem
- ✅ Storage local

**Entra em cena:**

- ✅ **Gateway** (rodando on-prem ou na AWS)

**Esse gateway se conecta, via canal seguro, ao serviço AWS Storage Gateway, que por sua vez fala com:**

- ✅ S3
- ✅ FSx for Windows File Server
- ✅ EBS / snapshots
- ✅ S3 Glacier / Deep Archive, etc.

**A aplicação on-prem continua falando via protocolos conhecidos (NFS/SMB/iSCSI), e o Gateway se encarrega de "empacotar" isso para AWS.**

---

### 3. Formas de Deployment do Gateway

Você pode rodar o Storage Gateway de três maneiras:

#### Máquina Virtual On-Prem

Em:

- ✅ VMware ESXi
- ✅ Microsoft Hyper-V
- ✅ Linux KVM

#### Hardware Appliance da AWS

- ✅ Appliance físico, pré-configurado, que você instala no seu datacenter

#### Na Própria AWS

- ✅ Como EC2 instance
- ✅ Ou em VMware Cloud on AWS

---

### 4. Cache Local

**Ao instalar o gateway, você precisa de pelo menos 150 MB de disco local** (na prática, vai ser bem mais).

**Cache serve para:**

#### Staging

- ✅ Área de preparação para dados que serão enviados para AWS

#### Cache Real

- ✅ Armazena dados acessados recentemente
- ✅ Em cada requisição, o gateway primeiro consulta o cache
- ✅ Só baixa da AWS se não estiver em cache → reduz latência e tráfego

---

### 5. Tipos de Storage Gateway e Protocolos

Você escolhe um tipo ao criar o gateway. Isso define:

- ✅ Protocolo exposto on-prem
- ✅ Serviço de backend na AWS

---

### 5.1 S3 File Gateway

**Protocolo**: NFS ou SMB

**Backend**: Amazon S3

**Mapeia:**

- ✅ Arquivos do share → objetos 1:1 em um bucket S3

**Vantagens:**

- ✅ Acesso a features nativas do S3:
  - Object Lock
  - Versioning
  - Cross-Region Replication
  - Lifecycle (Glacier / Deep Archive)
- ✅ **Cache local ajustável**: até 64 TB
- ✅ Ideal para backups grandes, imagens, dumps de banco etc.

**Limitações:**

- ⚠️ Limitado a **10 file shares**
- ⚠️ **100 usuários ativos** por gateway

---

### 5.2 FSx File Gateway

**Protocolo**: SMB

**Backend**: Amazon FSx for Windows File Server

**Integração:**

- ✅ Active Directory
- ✅ Experiência completa de Windows file system:
  - Shadow copies
  - Application-consistent backup
  - Data deduplication
  - ACLs Windows

**Características:**

- ✅ Otimizado para arquivos pequenos e mistos (documentos, shares de equipe)
- ✅ **Multi-user file share**:
  - Ilimitados file shares
  - Até **500 usuários ativos** por gateway
- ✅ Requer Direct Connect ou VPN entre on-prem e AWS

---

### 5.3 Tape Gateway

**Protocolo**: iSCSI, expondo:

- ✅ Virtual Tape Drives
- ✅ Media Changer

**Backend:**

- ✅ Virtual Tape Library em S3
- ✅ Tapes arquivados em:
  - S3 Glacier Flexible Retrieval
  - S3 Glacier Deep Archive

**Objetivo:**

- ✅ Substituir bibliotecas de fitas físicas por VTL na AWS
- ✅ **"Drop-in replacement"** para ferramentas de backup tradicionais:
  - Dell EMC, CommVault, IBM, etc.

---

### 5.4 Volume Gateway

**Protocolo**: iSCSI (block storage)

**Backend:**

- ✅ Dados/snapshots armazenados em S3 como EBS-style snapshots

**Modos:**

#### Cached Volumes

- ✅ **Dados primários em S3**
- ✅ Apenas dados frequentemente acessados ficam localmente
- ✅ Focado em reduzir storage on-prem

#### Stored Volumes

- ✅ **Dados primários on-prem**
- ✅ Backup assíncrono para S3 (snapshots)
- ✅ Perfeito para DR com dados locais íntegros

**Snapshots:**

- ✅ Usando scheduler nativo do Gateway ou AWS Backup
- ✅ Podem ser restaurados como:
  - Volumes de Storage Gateway
  - Volumes EBS anexáveis a EC2

---

### 6. Modelo de Preços

Três componentes principais:

#### Storage

Depende do backend:

- ✅ **S3 File Gateway** → preço de S3
- ✅ **FSx File Gateway** → preço de FSx for Windows
- ✅ **Volume / Tape** → armazenado em S3 / Glacier com preço dos respectivos serviços

#### Requests

Também seguem preços de:

- ✅ S3
- ✅ FSx, etc.

#### Data Transfer

- ✅ **Entrada para AWS**: Geralmente sem custo (inbound)
- ✅ **Saída do Storage Gateway para on-prem**: Cobrança por TB/mês transferido de volta

> 💡 **Importante**: Conferir página oficial de pricing para números atualizados

---

### 7. Cenários Práticos e Escolha do Gateway

#### 7.1 Cenário 1 — Backup e Arquivamento (Hadoop + SQL on-prem)

**Empresa quer:**

- ✅ Manter Hadoop e SQL on-prem (latência)
- ✅ Mas mudar backup / arquivamento para cloud

**Gateways aplicáveis:**

- ✅ S3 File Gateway
- ✅ Volume Gateway
- ⚠️ (Tape não faz sentido aqui, pois não falamos de fitas)

---

##### 7.1.1 Usando S3 File Gateway

**Fluxo:**

1. Deploy e ativação do gateway
2. Criação de file share NFS/SMB
3. Associação do share a um bucket S3
4. Montagem do share nos servidores (Hadoop/SQL)
5. Tudo que é escrito no share vira objeto no S3 (1 arquivo = 1 objeto)

**Benefícios:**

- ✅ **S3 como backend**:
  - Object Lock
  - Versionamento
  - Lifecycle para Glacier / Deep Archive
  - Replicação entre regiões
- ✅ **Custos**: Pode jogar dados antigos para classes mais baratas via lifecycle
- ✅ **Latência**: Cache local dimensionado ~ tamanho do working set
  - Exemplo: 1 TB acessado diariamente → cache de 1 TB
  - Limite: até 64 TB de cache

---

##### 7.1.2 Usando FSx File Gateway

**Serve quando:**

- ✅ Cenário for mais de file share Windows multi-usuário
- ✅ Precisa de:
  - Shadow copies
  - Dedup
  - Application-consistent backup
  - Experiência Windows "full"

**Diferenças chaves vs S3 File Gateway:**

| Aspecto | S3 File Gateway | FSx File Gateway |
|---------|----------------|------------------|
| **Melhor para** | Backups grandes, imagens, dumps, arquivos grandes | Group shares, home directories, edição colaborativa |
| **File shares** | Limitado a 10 | Ilimitados |
| **Usuários ativos** | 100 por gateway | 500 por gateway |

**Minha visão:**

- ✅ Se o foco é **backup + integração com S3/lifecycle** → **S3 File Gateway**
- ✅ Se o foco é **infra de file server Windows compartilhado** → **FSx File Gateway**

---

#### 7.2 Cenário 2 — Backup + Migração Futura de SQL para EC2

**Novo requisito:**

- ✅ Além de backup, a empresa quer migrar o SQL para EC2 no futuro
- ✅ Começar guardando backups como EBS snapshots, depois subir como volumes EC2

**Gateway ideal**: Volume Gateway

**Fluxo:**

1. Deploy do Volume Gateway
2. Criar volumes iSCSI e anexar às aplicações on-prem
3. Aplicação lê/escreve nesses volumes normalmente
4. Agendar snapshots:
   - Pelo scheduler do gateway
   - Ou via AWS Backup
5. Snapshots:
   - Ficam em S3, em bucket gerenciado (não acessível como objetos S3 normais)
   - Podem ser restaurados como:
     - Volumes de gateway
     - Volumes EBS montados em EC2

**Escolha do modo:**

- ✅ **Cached volumes**: Dados primários em S3, apenas hot data local → Bom para reduzir storage on-prem
- ✅ **Stored volumes**: Dados primários locais, backup assíncrono em S3 → Melhor para este cenário (dados críticos on-prem + DR na AWS)

**Extras:**

- ✅ Cache de dados acessados com frequência
- ✅ Compressão de dados em trânsito → menos banda e menos custo de storage

---

#### 7.3 Cenário 3 — Aposentar Fitas Físicas (Virtual Tape Library)

**Cliente hoje:**

- ✅ Usa fitas físicas
- ✅ Armazena off-site

**Problemas:**

- ⚠️ Tempo para comprar/gerenciar fitas
- ⚠️ Fitas degradam
- ⚠️ Restauração é lenta e incerta

**Gateway ideal**: Tape Gateway

**Comportamento:**

1. Deploy do Tape Gateway
2. Montagem em servidores on-prem:
   - Virtual Tape Drives
   - Media Changer
3. Integração via iSCSI com o software de backup existente
4. Software de backup passa a escrever em fitas virtuais
5. Gateway:
   - Comprime
   - Criptografa
   - Armazena em Virtual Tape Library baseada em S3
6. Quando a fita não precisa de acesso imediato:
   - Você "eject/export" no software
   - Gateway arquiva para:
     - Glacier Flexible Retrieval
     - Glacier Deep Archive

**Resultado:**

- ✅ Deixa de manter fitas físicas, mas preserva mesma lógica e ferramenta de backup (drop-in replacement)

---

### 8. Guia Rápido — Quando Usar Cada Tipo

#### S3 File Gateway

**Use quando:**

- ✅ Backup e arquivamento custo-efetivo
- ✅ Ingest para:
  - Data lakes
  - Analytics
  - Machine Learning
- ✅ Quando você quer poder do S3 (lifecycle, object lock, versionamento etc.)

---

#### FSx File Gateway

**Use quando:**

- ✅ Multi-user interactive file sharing:
  - Group shares
  - Project shares
  - Home directories
- ✅ Edição de mídia / documentos
- ✅ Quando você quer paridade com Windows file server (shadow copy, dedup, AD, ACLs)

---

#### Volume Gateway

**Use quando:**

- ✅ Backup de volumes on-prem para cloud
- ✅ Migração de volumes para EC2 (via EBS snapshots)
- ✅ Disaster Recovery com restauração rápida em EC2

---

#### Tape Gateway

**Use quando:**

- ✅ Transição de tapes físicos → VTL na AWS
- ✅ Backup e arquivamento em S3 + Glacier

---

## 📊 Comparação: Snow Family vs Storage Gateway

| Aspecto | Snow Family | Storage Gateway |
|---------|-------------|----------------|
| **Tipo** | Dispositivo físico | Appliance virtual |
| **Instalação** | AWS envia para você | Você instala VM no seu ambiente |
| **Uso** | Transferência offline | Integração híbrida contínua |
| **Capacidade** | TB a PB | Limitado pelo storage local |
| **Conexão** | Não precisa internet | Precisa conexão com AWS |
| **Quando usar** | Migração massiva, edge remoto | Integração contínua, backup |

---

## 📊 Resumo Rápido

### AWS Snow Family

| Dispositivo | Capacidade | Propósito |
|-------------|-----------|-----------|
| **Snowcone** | Alguns TB | Edge, mobilidade, EC2 no campo |
| **Snowball** | Dezenas a centenas de TB | Migração, processamento no edge |
| **Snowmobile** | Até 100 PB | Migração massiva de data centers |

### AWS Storage Gateway

| Tipo | Protocolo | Backend | Ideal para |
|------|-----------|---------|------------|
| **S3 File Gateway** | NFS/SMB | S3 | Backups grandes, arquivos grandes, integração com S3 features |
| **FSx File Gateway** | SMB | FSx for Windows | File sharing Windows multi-usuário, shadow copies, AD |
| **Volume Gateway (Stored)** | iSCSI | S3 (snapshots) | Dados críticos on-prem + DR na AWS |
| **Volume Gateway (Cached)** | iSCSI | S3 (dados primários) | Reduzir storage on-prem, dados primários na nuvem |
| **Tape Gateway** | iSCSI (VTL) | S3 + Glacier | Substituir fitas físicas, backup tradicional |

---

## 🎯 Quando Usar Cada Serviço

### AWS Snow Family

**Use quando:**

- ✅ Precisa migrar volumes massivos (TB/PB)
- ✅ Rede é lenta ou cara demais
- ✅ Precisa transferir dados offline
- ✅ Está em local remoto sem internet confiável
- ✅ Quer processar dados no edge antes de enviar para AWS
- ✅ Precisa de segurança física extrema (Snowmobile)

**Escolha do dispositivo:**

- ✅ **Snowcone**: Edge computing, locais remotos, alguns TB
- ✅ **Snowball**: Migração média/grande, processamento edge, dezenas/centenas de TB
- ✅ **Snowmobile**: Migração massiva de data centers, até 100 PB

---

### AWS Storage Gateway

**Use quando:**

- ✅ Precisa integração contínua entre on-premises e AWS
- ✅ Quer expandir storage sem comprar hardware
- ✅ Precisa backup automático para S3
- ✅ Quer substituir fitas físicas
- ✅ Precisa acessar dados S3 como NFS
- ✅ Quer reduzir storage on-premises (Cached Volumes)
- ✅ Precisa baixa latência local com backup na nuvem (Stored Volumes)

**Escolha do tipo:**

- ✅ **S3 File Gateway**: Backup e arquivamento custo-efetivo, ingest para data lakes/analytics, quando quer features do S3
- ✅ **FSx File Gateway**: File sharing Windows multi-usuário, group shares, home directories, edição colaborativa
- ✅ **Volume Gateway (Stored)**: Dados críticos on-prem + backup assíncrono na AWS, DR
- ✅ **Volume Gateway (Cached)**: Reduzir storage local, dados primários na nuvem, migração futura para EC2
- ✅ **Tape Gateway**: Substituir bibliotecas de fita físicas, backup tradicional com ferramentas existentes

---

## 🔄 Fluxo de Trabalho Típico

### Com Snow Family

```
┌─────────────────────────────────────────────────────────┐
│  1. Solicitar dispositivo Snow                          │
│     ↓                                                    │
│  2. AWS envia dispositivo físico                         │
│     ↓                                                    │
│  3. Conectar no data center                              │
│     ↓                                                    │
│  4. Transferir dados para dispositivo                    │
│     ↓                                                    │
│  5. Devolver dispositivo para AWS                        │
│     ↓                                                    │
│  6. AWS carrega dados no S3                             │
└──────────────────────────────────────────────────────────┘
```

### Com Storage Gateway

```
┌─────────────────────────────────────────────────────────┐
│  On-Premises                                             │
│  ┌──────────────────────────────────────┐               │
│  │   Aplicação                          │               │
│  │   (NFS/iSCSI/Backup)                 │               │
│  └──────────────┬───────────────────────┘               │
│                 │                                        │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐               │
│  │   Storage Gateway (VM)                │               │
│  │   • Cache local                       │               │
│  │   • Sincronização contínua            │               │
│  └──────────────┬───────────────────────┘               │
│                 │                                        │
│                 │ HTTPS/TLS                               │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐               │
│  │   AWS (S3/Glacier/EBS)                │               │
│  └───────────────────────────────────────┘               │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Recursos Adicionais

### AWS Snow Family

- [AWS Snow Family](https://aws.amazon.com/snow/)
- [AWS Snowcone](https://aws.amazon.com/snowcone/)
- [AWS Snowball](https://aws.amazon.com/snowball/)
- [AWS Snowmobile](https://aws.amazon.com/snowmobile/)

### AWS Storage Gateway

- [AWS Storage Gateway - Página do Produto](https://aws.amazon.com/storagegateway/)
- [AWS Storage Gateway - Documentação](https://docs.aws.amazon.com/storagegateway/)
- [S3 File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/FileGateway.html)
- [FSx File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/fsx-file-gateway.html)
- [Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/VolumeGateway.html)
- [Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/TapeGateway.html)
- [Storage Gateway Pricing](https://aws.amazon.com/storagegateway/pricing/)

---

## ✅ Checklist de Aprendizado

### AWS Snow Family

- [ ] Entender o conceito da AWS Snow Family (dispositivos físicos)
- [ ] Conhecer os três dispositivos: Snowcone, Snowball, Snowmobile
- [ ] Entender quando usar Snow Family vs transferência via rede
- [ ] Saber escolher o dispositivo correto (Snowcone vs Snowball vs Snowmobile)

### AWS Storage Gateway

- [ ] Entender o conceito de Storage Gateway como "tradutor" entre on-prem e AWS
- [ ] Conhecer as três formas de deployment (VM on-prem, Hardware Appliance, EC2)
- [ ] Entender o papel do cache local (staging e cache real)
- [ ] Conhecer os tipos de Storage Gateway e seus protocolos
- [ ] Entender S3 File Gateway (NFS/SMB → S3)
- [ ] Entender FSx File Gateway (SMB → FSx for Windows)
- [ ] Entender diferenças entre S3 File Gateway e FSx File Gateway
- [ ] Entender Volume Gateway Stored (dados on-prem, backup S3)
- [ ] Entender Volume Gateway Cached (dados S3, cache local)
- [ ] Entender Tape Gateway (iSCSI VTL → S3/Glacier)
- [ ] Saber quando usar cada tipo de Storage Gateway
- [ ] Entender modelo de preços (storage, requests, data transfer)
- [ ] Conhecer cenários práticos de uso (backup, migração, fitas)
- [ ] Compreender diferença entre Snow Family e Storage Gateway

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#snow-family` `#snowcone` `#snowball` `#snowmobile` `#storage-gateway` `#file-gateway` `#s3-file-gateway` `#fsx-file-gateway` `#volume-gateway` `#tape-gateway` `#hybrid-cloud` `#edge-computing` `#backup` `#disaster-recovery`

---

## 🎯 Conclusão

**AWS Snow Family** e **AWS Storage Gateway** são serviços complementares:

- ✅ **Snow Family**: Para transferência offline de volumes massivos quando a rede não é viável
- ✅ **Storage Gateway**: Para integração híbrida contínua entre on-premises e AWS, funcionando como "tradutor" entre protocolos on-prem (NFS/SMB/iSCSI) e serviços AWS (S3/FSx/EBS/Glacier)

**Storage Gateway oferece:**

- ✅ **S3 File Gateway**: Backup e arquivamento custo-efetivo com acesso a features nativas do S3
- ✅ **FSx File Gateway**: File sharing Windows completo com integração Active Directory
- ✅ **Volume Gateway**: Backup e migração de volumes com snapshots restaurados como EBS
- ✅ **Tape Gateway**: Substituição de fitas físicas por VTL virtual na AWS

Ambos são essenciais em diferentes cenários de migração e integração de dados, permitindo estratégias híbridas sem "big bang" para cloud.

---

**Última atualização**: 📅 [DD/MM/YYYY]

