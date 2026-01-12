# 🔄 AWS Elastic Disaster Recovery (DRS) — Visão Geral

> Serviço de disaster recovery nativo da AWS com RPO de segundos e RTO de minutos

---

## 📌 Resumo

O **AWS Elastic Disaster Recovery (DRS)** é o serviço de disaster recovery nativo da AWS, criado para substituir ambientes de DR físicos e caros por um modelo elástico e sob demanda.

Ele replica dados de servidores físicos, virtuais, EC2, on-premises ou até de outras clouds para a AWS, em nível de bloco, garantindo:

- ✅ **RPO de segundos**
- ✅ **RTO de minutos**

**Ou seja**, DRS mantém uma cópia atualizada dos seus servidores dentro da AWS, sem ter recursos rodando o tempo todo. Você só paga por instâncias de recuperação quando um drill ou failover realmente é iniciado.

---

## 🎯 Visão Geral

### O Problema que Resolve

**Ambientes de DR tradicionais:**

- ⚠️ Hardware duplicado ocioso
- ⚠️ Custos fixos altos
- ⚠️ Problemas de sincronização de replicação
- ⚠️ Complexidade de gerenciamento

**Solução DRS:**

- ✅ Modelo elástico e sob demanda
- ✅ Custo apenas quando usa (pay-as-you-go)
- ✅ Replicação contínua em nível de bloco
- ✅ RPO de segundos, RTO de minutos

---

## ⚙️ Como o DRS Funciona — Visão Macro

A arquitetura sempre segue o mesmo fluxo:

### 1. Inicializa o DRS na Região de Destino

- ✅ Cria permissões e configurações padrão
- ✅ Define o tipo e tamanho dos replication servers (ex.: `t3.small`)

### 2. DRS Cria Servidores de Replicação Automaticamente

- ✅ São **EC2 leves** que escrevem os dados recebidos em EBS volumes
- ✅ Eles ficam dentro de uma **staging area subnet**

### 3. Você Instala o AWS Replication Agent nos Servidores Fonte

**Suporta:**

- ✅ Windows
- ✅ Linux
- ✅ VMware
- ✅ Bare metal
- ✅ EC2
- ✅ Outras clouds (Azure, GCP, etc.)

**O agente replica continuamente os blocos do disco → EBS**

- ✅ Cada disco do servidor vira um volume EBS correspondente
- ✅ Esse processo inclui:
  - SO
  - Aplicações
  - Dados completos

### 4. Em Caso de Drill ou Failover

**O DRS converte os dados replicados em um EC2 executável**

- ✅ Usando um launch template gerado automaticamente
- ✅ O template inclui:
  - Subnet
  - Security Group
  - IAM Role
  - Tipo de instância
  - User Data

---

## 🔧 Configurações Essenciais do DRS

### 1. Replication Servers

- ✅ **EC2 leves**, criados sob demanda
- ✅ Cada replication server pode lidar com múltiplas máquinas replicadas
- ✅ Comunicam-se via **TCP 1500**

---

### 2. Tipos de EBS

**Você pode:**

- ✅ Deixar que o DRS escolha automaticamente o tipo baseado no throughput do disco original
- ✅ Ou forçar:
  - **SSD** (gp3 / io2)
  - **HDD** (st1 / sc1)

---

### 3. Configurações de Rede

- ✅ **Security Group** que permita comunicação TCP 1500
- ✅ Pode usar **Direct Connect** ou **VPN**
- ✅ **Throttling opcional** para limitar banda
- ✅ Suporte a **retenção de snapshots por até 1 ano**, para pontos-no-tempo (PITR) — crucial para cenários de ransomware

---

### 4. Right-Sizing de Instâncias

- ✅ O DRS tenta mapear automaticamente o hardware de origem para a instância EC2 equivalente

**Para Windows, você escolhe:**

- ✅ **AWS-provided license**
- ✅ **BYOL** (Bring Your Own License)

---

### 5. Launch Settings + Launch Templates

- ✅ Definem como os EC2 de recuperação/drill serão criados
- ✅ Cada servidor tem seu próprio launch template derivado do template principal

---

## 📥 Instalando o AWS Replication Agent

**O agente pode rodar em:**

- ✅ EC2 existentes
- ✅ Servidores on-prem (bare metal ou virtualizados)
- ✅ Outras clouds (Azure, GCP etc.)

**Características:**

- ✅ Ele captura dados em **nível de bloco**, o que o torna independente de SO ou aplicações
- ✅ **Essa é uma diferença do DRS**: ele replica o servidor inteiro, não apenas dados

---

## 🧪 Processo de Drill (Teste de Recuperação)

Antes de confiar no DR, você deve testar. O drill funciona assim:

### 1. Seleciona Servidores

- ✅ Um ou vários servidores (até 100 simultâneos)

### 2. Escolhe Ponto-no-Tempo

- ✅ **Último estado replicado**
- ✅ Ou um **snapshot exato (PITR)**

### 3. DRS Cria Recovery Instances

- ✅ Na subnet de recuperação

### 4. Você Valida

- ✅ **Boot correto**
- ✅ **Conectividade** entre instâncias
- ✅ **Aplicações funcionando**
- ✅ DNS, fila, API, integrações etc.

> ⚠️ **É fundamental testar cadeia completa, não só ver se a instância subiu.**

---

## 🚨 Processo de Failover Real

É idêntico ao drill, exceto pelo propósito:

### 1. Inicia um Recovery Job

- ✅ Seleciona ponto-no-tempo desejado

### 2. Instâncias EC2 São Criadas

- ✅ Com boot já funcional

### 3. Você Ajusta

- ✅ DNS/tráfego para a nova região/AZ

### 4. Failback (Quando Apropriado)

- ✅ Assim que os servidores originais estiverem saudáveis
- ✅ Usa o DRS para failback, retornando operações ao ambiente primário

---

## 🎯 Casos de Uso Estratégicos

### a) Substituir Datacenters de DR

**Você elimina:**

- ✅ Hardware duplicado ocioso
- ✅ Custos fixos
- ✅ Problemas de sincronização de replicação

---

### b) Failover entre AZs ou Regiões da AWS

- ✅ Serve para workloads já rodando dentro da AWS

---

### c) Migração para AWS (Lift-and-Shift)

**DRS pode ser usado como ferramenta de migração completa:**

1. Replica servidor on-prem →
2. Converte em EC2 executável →
3. Você faz cutover definitivo

---

## 🎁 Benefícios Importantes

- ✅ **RPO de segundos**
- ✅ **RTO de minutos**
- ✅ **Custo elástico** (só paga EC2 quando usa)
- ✅ **Replica todo o servidor**, incluindo SO
- ✅ **Protege contra ransomware** com PITR snapshots
- ✅ **Suporta on-prem, AWS e outras clouds**
- ✅ **Sem dependência de hypervisor**

---

## 📊 Em Síntese — Resumo Técnico

| Parte | O que faz |
|-------|-----------|
| **Replication Agent** | Captura blocos do disco no servidor original |
| **Replication Servers** | EC2 leves que escrevem esses blocos em EBS |
| **Staging Area** | Subnet onde ficam volumes replicados continuamente |
| **Launch Templates** | Template EC2 para criar instâncias de recovery |
| **Drill** | Teste de recuperação sem impacto |
| **Failover** | Recuperação real para a AWS |
| **Failback** | Retorno ao ambiente original |

---

## 🔄 Fluxo Completo de Operação

```
┌─────────────────────────────────────────────────────────┐
│  Servidor Fonte                                          │
│  (On-prem, EC2, Azure, GCP, etc.)                        │
│  ┌──────────────────────────────────────┐                │
│  │   AWS Replication Agent              │                │
│  │   • Captura blocos do disco          │                │
│  │   • Replicação contínua              │                │
│  └──────────────┬───────────────────────┘               │
│                 │                                         │
│                 │ TCP 1500                                │
│                 ▼                                         │
│  ┌──────────────────────────────────────┐                │
│  │   Replication Servers (EC2 leves)     │                │
│  │   • Staging Area Subnet               │                │
│  │   • Escreve em EBS volumes            │                │
│  └──────────────┬───────────────────────┘               │
│                 │                                         │
│                 │ Snapshots                               │
│                 ▼                                         │
│  ┌──────────────────────────────────────┐                │
│  │   EBS Volumes Replicados             │                │
│  │   • Retenção até 1 ano (PITR)         │                │
│  └──────────────┬───────────────────────┘               │
│                 │                                         │
│                 │ Drill / Failover                        │
│                 ▼                                         │
│  ┌──────────────────────────────────────┐                │
│  │   Launch Template                    │                │
│  │   • Subnet, SG, IAM Role             │                │
│  │   • Tipo de instância                │                │
│  └──────────────┬───────────────────────┘               │
│                 │                                         │
│                 ▼                                         │
│  ┌──────────────────────────────────────┐                │
│  │   Recovery Instances (EC2)            │                │
│  │   • Boot funcional                   │                │
│  │   • Aplicações rodando               │                │
│  └──────────────────────────────────────┘               │
└──────────────────────────────────────────────────────────┘
```

---

## 🎯 Quando Usar DRS

**Use quando:**

- ✅ Precisa RPO de segundos e RTO de minutos
- ✅ Quer eliminar custos de DR físico
- ✅ Precisa proteger contra ransomware (PITR)
- ✅ Quer migrar servidores para AWS (lift-and-shift)
- ✅ Precisa failover entre regiões/AZs
- ✅ Tem servidores em múltiplas plataformas (on-prem, outras clouds)

**Não use quando:**

- ❌ Precisa apenas backup de dados (use AWS Backup)
- ❌ Workloads já são nativos cloud (containers, serverless)
- ❌ Não precisa de DR tão agressivo

---

## 🔗 Recursos Adicionais

- [AWS Elastic Disaster Recovery - Página do Produto](https://aws.amazon.com/disaster-recovery/)
- [AWS DRS - Documentação](https://docs.aws.amazon.com/drs/)
- [AWS DRS - Guia do Usuário](https://docs.aws.amazon.com/drs/latest/userguide/)
- [AWS DRS - Pricing](https://aws.amazon.com/disaster-recovery/pricing/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito de DRS como DR elástico e sob demanda
- [ ] Compreender RPO de segundos e RTO de minutos
- [ ] Entender componentes principais (Replication Agent, Replication Servers, Staging Area)
- [ ] Conhecer processo de replicação em nível de bloco
- [ ] Entender diferença entre Drill e Failover
- [ ] Compreender processo de Failback
- [ ] Conhecer suporte a múltiplas plataformas (on-prem, EC2, outras clouds)
- [ ] Entender proteção contra ransomware com PITR
- [ ] Conhecer casos de uso (substituir DR físico, migração, failover)
- [ ] Entender configurações essenciais (replication servers, EBS types, rede)
- [ ] Compreender Launch Templates e right-sizing
- [ ] Saber quando usar DRS vs outros serviços de backup/DR

---

## 🏷️ Tags

`#aws` `#disaster-recovery` `#drs` `#elastic-disaster-recovery` `#rpo` `#rto` `#replication` `#failover` `#failback` `#migration` `#lift-and-shift` `#ransomware-protection` `#pitr`

---

## 🎯 Conclusão

**AWS Elastic Disaster Recovery (DRS)** oferece:

- ✅ **DR elástico e sob demanda** — paga apenas quando usa
- ✅ **RPO de segundos, RTO de minutos** — proteção de nível enterprise
- ✅ **Replicação em nível de bloco** — independente de SO e aplicações
- ✅ **Proteção contra ransomware** — com PITR e snapshots de até 1 ano
- ✅ **Suporte multiplataforma** — on-prem, AWS, outras clouds
- ✅ **Migração simplificada** — pode ser usado para lift-and-shift completo

**Ideal para** substituir ambientes de DR físicos caros por um modelo cloud-native, elástico e eficiente.

---

**Última atualização**: 📅 [DD/MM/YYYY]


