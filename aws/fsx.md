# 📁 Amazon FSx — Visão Geral Prática

> Família de file systems gerenciados especializados para diferentes workloads

---

## 📌 Resumo

**Amazon FSx** é uma família de file systems gerenciados que oferece soluções especializadas para diferentes necessidades:

- ✅ **FSx for Windows File Server** — file system Windows completo com integração AD
- ✅ **FSx for Lustre** — file system de alta performance para HPC e ML
- ✅ **FSx for NetApp ONTAP** — migração de workloads NetApp para AWS
- ✅ **FSx for OpenZFS** — file system baseado em ZFS com alta integridade de dados

---

## 🎯 EFS vs FSx — Quando Escolher Cada Um

Antes de entrar nos sabores do FSx, vale fixar o papel de cada serviço:

### Amazon EFS

- ✅ File system gerenciado para **workloads Linux**
- ✅ Baseado em **NFSv4**
- ✅ **Regional**, replicado entre múltiplas AZs
- ✅ Ideal para:
  - Vários EC2 Linux compartilhando arquivos
  - Containers / microserviços Linux
  - Aplicações que só precisam de "um NFS gerenciado e elástico"

### Amazon FSx

- ✅ Família de **file systems especializados**
- ✅ Sabores:
  - FSx for Windows File Server
  - FSx for Lustre
  - FSx for NetApp ONTAP
  - FSx for OpenZFS

### Decisão de Alto Nível

| Necessidade | Solução |
|-------------|---------|
| File system simples para Linux | **EFS** |
| File system Windows (SMB, AD, quotas, shadow copies) | **FSx for Windows File Server** |
| HPC / ML / throughput absurdo | **FSx for Lustre** |
| Já usa NetApp ONTAP on-prem | **FSx for NetApp ONTAP** |
| Já usa ZFS / OpenZFS | **FSx for OpenZFS** |

---

## 🪟 FSx for Windows File Server

File system Windows 100% gerenciado na AWS.

### Características Principais

**Protocolo**: SMB

**Acesso a partir de:**

- ✅ Windows
- ✅ Linux (via cifs-utils)
- ✅ macOS

### Casos de Uso

- ✅ **Home directories**
- ✅ **Line-of-business apps** (ERP, etc.)
- ✅ **Web servers e CMS**
- ✅ Ambientes de desenvolvimento
- ✅ **Workflows de mídia**
- ✅ Data analytics que dependem de shares Windows

---

### Integração e Administração

**Baseado em Windows File Server**

**Integra com Microsoft Active Directory:**

- ✅ AD gerenciado pela AWS ou AD on-prem
- ✅ Controle de:
  - Autenticação
  - Autorização
  - ACLs e permissões compartilhadas

**Recursos administrativos:**

- ✅ **User quotas**
- ✅ **End-user file restore** (shadow copies)
- ✅ Compatível com modelos tradicionais de file server corporativo

---

### Capacidade e Throughput

**Capacidade:**

- ✅ De **32 GB até 64 TB** por file system
- ✅ Pode combinar vários file systems via **DFS (Distributed File System)** → centenas de PB

**Throughput:**

- ✅ De **32 MB/s até 2–12 GB/s** (depende da região)
- ✅ **Modelo de network I/O credit**:
  - Abaixo do baseline → acumula créditos
  - Acima do baseline → consome créditos
  - Permite burst de throughput quando necessário

**Cache:**

- ✅ Cache em memória para arquivos mais acessados → melhora bastante a latência

---

### Otimização de Espaço

**Data deduplication opcional:**

- ✅ Remove blocos duplicados transparentemente
- ✅ Roda em background
- ✅ Ideal para:
  - Diretórios de usuários
  - Repositórios com muitos arquivos semelhantes

---

### Segurança, Backups e Alta Disponibilidade

#### Segurança

- ✅ **Criptografia at rest**
- ✅ **Criptografia in transit** com SMB 3.0+ (Kerberos session keys)

#### Manutenção

- ✅ Janela semanal configurável (até 30 minutos, em geral só uma fração é usada)

#### Backups

- ✅ **Backups automáticos diários** configuráveis (retention de 0 a 35 dias)
- ✅ Backups além de 35 dias → backups manuais (user-initiated)

#### Alta Disponibilidade

**Single-AZ:**

- ✅ Mais barato
- ⚠️ Fica indisponível durante manutenção / falha na AZ

**Multi-AZ:**

- ✅ **Failover/failback automático** entre AZs
- ✅ Maior resiliência, custo um pouco maior

---

### Conectividade

**Acesso por:**

- ✅ **EC2 na mesma VPC** (via Elastic Network Interface criada pelo FSx)
- ✅ **On-prem** (via VPN ou Direct Connect)

**Mapeamento de drive:**

- ✅ Usar o DNS name do share a partir do console FSx

---

## ⚡ FSx for Lustre

File system para **High Performance Computing (HPC)**.

### Visão Geral

- ✅ Baseado no **Lustre** (open source)
- ✅ File system **POSIX compartilhado**
- ✅ Focado em:
  - **Throughput altíssimo**
  - **Milhões de IOPS**
  - **Latência sub-millisecond**
  - Acesso concorrente por **centenas de milhares de cores**

---

### Casos de Uso

- ✅ **HPC em geral**
- ✅ **Treinamento e inferência de Machine Learning**
- ✅ **Processamento de vídeo**
- ✅ **Simulações científicas**
- ✅ Qualquer workload onde storage lento vira gargalo do cluster

---

### SSD vs HDD e Cache

**Se o workload é:**

- ✅ **Sensível a latência / IOPS** e faz acesso aleatório pequeno → use **SSD**
- ✅ **Sensível a throughput sequencial** (arquivos grandes, streaming de dados) → use **HDD**

**Possível usar:**

- ✅ **HDD como base**
- ✅ **SSD cache** para arquivos mais acessados → latência sub-ms e IOPS alta

---

### Cloud Bursting

**Cenário típico:**

- ✅ Dados on-prem
- ✅ Falta capacidade de compute on-prem

**Estratégia:**

1. Subir cluster de compute na AWS
2. Criar FSx for Lustre
3. Montar o FSx tanto:
   - No cluster cloud
   - Quanto on-prem (via Direct Connect / VPN)
4. Mover dados criticamente necessários para o FSx (ficam perto do compute)
5. Processar
6. Devolver resultados e desligar recursos de compute

**Isso reduz custo, mantendo apenas armazenamento duradouro (muitas vezes em S3) e liga compute sob demanda.**

---

### Integração com S3

**Ponto-chave do FSx for Lustre:**

**Link direto com S3:**

- ✅ Você associa um **bucket S3 ao FSx**
- ✅ Os objetos aparecem como arquivos no file system

**Modelo de operação:**

- ✅ **Primeira vez que acessa um "arquivo"**:
  - FSx faz **lazy load** do objeto S3 para o file system
- ✅ **Ao salvar**:
  - FSx **backfill** o objeto de volta no S3
- ✅ **Arquivos novos**:
  - Viram objetos novos no S3

**Benefícios:**

- ✅ **S3 vira source of truth**
- ✅ Pode desligar FSx e compute quando não estiver usando
- ✅ Pode associar o mesmo bucket a clusters em múltiplas AZs

---

## 🏢 FSx for NetApp ONTAP

Para quem já vive no mundo **NetApp ONTAP on-prem**.

### Objetivo

Migrar workloads ONTAP para AWS sem perder:

- ✅ Snapshots
- ✅ SnapMirror
- ✅ FlexClone
- ✅ Modelo de administração NetApp

---

### Recursos Principais

**Protocolos suportados:**

- ✅ NFS
- ✅ SMB
- ✅ iSCSI

**Integração:**

- ✅ Active Directory

**Funcionalidades:**

- ✅ **Dynamic capacity & throughput scaling**
- ✅ **Deploy Multi-AZ** para alta disponibilidade
- ✅ **Cross-region replication**
- ✅ **Backups automáticos** para DR
- ✅ **Tiering de storage**:
  - Dados menos acessados vão para capacity pool storage mais barato
  - Compressão possível para reduzir ainda mais custo

---

### Performance

Escala para:

- ✅ **Centenas de milhares de IOPS**
- ✅ Até **6 GB/s de throughput**
- ✅ **Latência sub-ms**

---

### Migração

**Usando SnapMirror:**

- ✅ Replicação de dados on-prem → FSx for NetApp ONTAP
- ✅ Feita via Direct Connect ou VPN

---

## 🐧 FSx for OpenZFS

Para quem já usa **ZFS/OpenZFS** em ambientes Linux/Unix.

### Características

**Focado em:**

- ✅ **Integridade de dados a longo prazo**
- ✅ **Consistência e proteção de dados** (modelo ZFS)

**Alta disponibilidade:**

- ✅ Suporte a **Multi-AZ**

**Proteção e recuperação:**

- ✅ **Snapshots instantâneos** (point-in-time)
- ✅ **Backups cross-region** para DR

**Protocolos:**

- ✅ Acesso via **NFS**
- ✅ A partir de:
  - Linux
  - macOS
  - Windows (via NFS)

---

### Performance

Entrega:

- ✅ **> 1 milhão de IOPS**
- ✅ Até **21 GB/s de throughput**

---

## 📊 Comparação Rápida: Tipos de FSx

| Tipo | Protocolo | Caso de Uso Principal | Performance |
|------|-----------|------------------------|-------------|
| **FSx for Windows File Server** | SMB | File sharing Windows, AD integration | Até 2-12 GB/s |
| **FSx for Lustre** | POSIX/NFS | HPC, ML, alta performance | Milhões de IOPS, sub-ms latência |
| **FSx for NetApp ONTAP** | NFS/SMB/iSCSI | Migração NetApp, enterprise storage | Centenas de milhares de IOPS, até 6 GB/s |
| **FSx for OpenZFS** | NFS | Integridade de dados, ZFS workloads | > 1M IOPS, até 21 GB/s |

---

## 🎯 Quando Usar Cada Tipo

### FSx for Windows File Server

**Use quando:**

- ✅ Precisa file system Windows completo
- ✅ Precisa integração com Active Directory
- ✅ Precisa shadow copies e quotas de usuário
- ✅ Tem aplicações Windows que dependem de file shares
- ✅ Precisa compatibilidade com modelos tradicionais de file server

---

### FSx for Lustre

**Use quando:**

- ✅ Precisa throughput extremamente alto
- ✅ Tem workloads de HPC ou ML
- ✅ Precisa latência sub-millisecond
- ✅ Quer integrar diretamente com S3
- ✅ Precisa cloud bursting de compute

---

### FSx for NetApp ONTAP

**Use quando:**

- ✅ Já usa NetApp ONTAP on-prem
- ✅ Quer migrar sem perder funcionalidades NetApp
- ✅ Precisa SnapMirror, FlexClone, snapshots
- ✅ Quer manter modelo de administração familiar

---

### FSx for OpenZFS

**Use quando:**

- ✅ Já usa ZFS/OpenZFS
- ✅ Precisa máxima integridade de dados
- ✅ Quer proteção de dados robusta
- ✅ Precisa snapshots instantâneos
- ✅ Tem workloads Linux/Unix que dependem de ZFS

---

## 🔗 Recursos Adicionais

- [Amazon FSx - Página do Produto](https://aws.amazon.com/fsx/)
- [FSx for Windows File Server - Documentação](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html)
- [FSx for Lustre - Documentação](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html)
- [FSx for NetApp ONTAP - Documentação](https://docs.aws.amazon.com/fsx/latest/ONTAPGuide/what-is.html)
- [FSx for OpenZFS - Documentação](https://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/what-is-fsx.html)
- [Comparação EFS vs FSx](https://aws.amazon.com/efs/faq/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender diferença entre EFS e FSx
- [ ] Conhecer os quatro tipos de FSx disponíveis
- [ ] Entender FSx for Windows File Server e suas características
- [ ] Compreender integração com Active Directory
- [ ] Entender recursos administrativos (quotas, shadow copies)
- [ ] Conhecer capacidades e throughput do FSx for Windows
- [ ] Entender FSx for Lustre e casos de uso (HPC, ML)
- [ ] Compreender integração FSx for Lustre com S3
- [ ] Entender cloud bursting com FSx for Lustre
- [ ] Conhecer FSx for NetApp ONTAP e migração de workloads NetApp
- [ ] Entender FSx for OpenZFS e características ZFS
- [ ] Saber quando usar cada tipo de FSx
- [ ] Compreender diferenças de protocolo e acesso

---

## 🏷️ Tags

`#aws` `#storage` `#file-system` `#fsx` `#fsx-windows` `#fsx-lustre` `#fsx-netapp` `#fsx-openzfs` `#windows-file-server` `#hpc` `#machine-learning` `#smb` `#nfs` `#active-directory`

---

## 🎯 Conclusão

**Amazon FSx** oferece file systems gerenciados especializados para diferentes necessidades:

- ✅ **FSx for Windows File Server**: Solução completa para ambientes Windows com integração AD
- ✅ **FSx for Lustre**: File system de alta performance para HPC e ML com integração S3
- ✅ **FSx for NetApp ONTAP**: Migração de workloads NetApp sem perder funcionalidades
- ✅ **FSx for OpenZFS**: File system baseado em ZFS com máxima integridade de dados

Cada tipo atende a necessidades específicas, permitindo escolher a solução certa para cada workload.

---

**Última atualização**: 📅 [DD/MM/YYYY]

