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

## 📦 AWS Storage Gateway

O **AWS Storage Gateway** é diferente da Snow Family.

Ele não é um dispositivo físico — é um **appliance virtual** instalado no seu data center (VMware/Hyper-V).

**A função dele é integrar seu storage on-premises com serviços da AWS**, como:

- ✅ Amazon S3
- ✅ Amazon Glacier / S3 Glacier
- ✅ Amazon FSx

Ele permite expandir seu armazenamento de forma barata, segura e sem alterar aplicativos existentes.

---

### 1️⃣ File Gateway

Serve para armazenar arquivos como objetos no S3, mas acessados como se estivessem em um NFS da sua rede.

**Funciona assim:**

- ✅ Você monta a File Gateway como NFS v3/v4
- ✅ Grava arquivos normalmente
- ✅ Cada arquivo vira um objeto no S3 (1 arquivo = 1 objeto)
- ✅ Há um cache local para acelerar acesso recente
- ✅ Tráfego enviado para S3 é via HTTPS e com criptografia SSE-S3

**Também existe:**

- ✅ **FSx File Gateway** — para servidores Windows

---

### 2️⃣ Volume Gateway

Apresenta volumes iSCSI para seus servidores.

**Existem dois modos:**

---

#### 🔸 Stored Volumes

**Seus dados ficam on-premises**

- ✅ AWS mantém cópias assíncronas como snapshots no S3/EBS
- ✅ Latência mínima (dados lidos localmente)
- ✅ Serve para DR com armazenamento local

**Fluxo:**

```
Aplicação → Volume iSCSI → Storage local → Gateway → S3 (snapshots)
```

---

#### 🔸 Cached Volumes

**Seus dados ficam no S3**

- ✅ O storage local serve como cache
- ✅ Reduz a necessidade de storage on-premises
- ✅ Ideal para grandes volumes de dados

**Fluxo:**

```
Aplicação → Volume iSCSI → Cache local → S3 (dados primários)
```

Você ainda pode fazer snapshots das volumes no EBS.

---

### 3️⃣ Tape Gateway (Gateway-VTL)

Simula uma biblioteca de fitas usando:

- ✅ **S3** → para armazenamento quente
- ✅ **S3 Glacier** → para arquivamento de longo prazo

**Ele substitui:**

- ✅ Robôs de fita
- ✅ Fitotecas físicas
- ✅ Mídias magnéticas

E funciona com o mesmo software de backup que você já usa, só que agora virtualizado.

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

| Tipo | Como funciona | Ideal para |
|------|---------------|------------|
| **File Gateway** | NFS → objetos S3 | Migração de arquivos, backups |
| **Stored Volumes** | Dados locais, backup no S3 | Baixa latência + backup |
| **Cached Volumes** | Dados no S3, cache local | Reduz storage on-premises |
| **Tape Gateway** | Fitas virtuais (S3 + Glacier) | Substituir fitas físicas |

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

- ✅ **File Gateway**: Migração de arquivos, backups de file shares
- ✅ **Stored Volumes**: Aplicações que precisam baixa latência + backup na nuvem
- ✅ **Cached Volumes**: Reduzir storage local, dados primários na nuvem
- ✅ **Tape Gateway**: Substituir bibliotecas de fita físicas

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

- [AWS Snow Family](https://aws.amazon.com/snow/)
- [AWS Snowcone](https://aws.amazon.com/snowcone/)
- [AWS Snowball](https://aws.amazon.com/snowball/)
- [AWS Snowmobile](https://aws.amazon.com/snowmobile/)
- [AWS Storage Gateway](https://aws.amazon.com/storagegateway/)
- [File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/FileGateway.html)
- [Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/VolumeGateway.html)
- [Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/TapeGateway.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito da AWS Snow Family (dispositivos físicos)
- [ ] Conhecer os três dispositivos: Snowcone, Snowball, Snowmobile
- [ ] Entender quando usar Snow Family vs transferência via rede
- [ ] Conhecer AWS Storage Gateway e seus tipos
- [ ] Entender File Gateway (NFS → S3)
- [ ] Entender Volume Gateway (Stored vs Cached)
- [ ] Entender Tape Gateway (fitas virtuais)
- [ ] Saber quando usar cada tipo de Storage Gateway
- [ ] Compreender diferença entre Snow Family e Storage Gateway
- [ ] Entender casos de uso de cada serviço

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#snow-family` `#snowcone` `#snowball` `#snowmobile` `#storage-gateway` `#file-gateway` `#volume-gateway` `#tape-gateway` `#edge-computing`

---

## 🎯 Conclusão

**AWS Snow Family** e **AWS Storage Gateway** são serviços complementares:

- ✅ **Snow Family**: Para transferência offline de volumes massivos quando a rede não é viável
- ✅ **Storage Gateway**: Para integração híbrida contínua entre on-premises e AWS

Ambos são essenciais em diferentes cenários de migração e integração de dados.

---

**Última atualização**: 📅 [DD/MM/YYYY]

