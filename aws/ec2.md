# 💻 Amazon EC2 — Elastic Compute Cloud

> Serviço de computação em nuvem escalável e flexível

---

## 📌 Resumo

O **Amazon EC2** (Elastic Compute Cloud) é o serviço de computação em nuvem da AWS que permite executar aplicações em servidores virtuais (instâncias) na nuvem. Oferece controle total sobre recursos de computação, permitindo escalar capacidade conforme necessário.

---

## 🎯 O que é

O Amazon EC2 oferece:

- ✅ **Instâncias virtuais**: servidores na nuvem configuráveis
- ✅ **Escalabilidade**: aumente ou diminua capacidade conforme necessário
- ✅ **Flexibilidade**: escolha entre diversos tipos de instâncias
- ✅ **Controle**: controle total sobre o ambiente de computação
- ✅ **Integração**: integra com outros serviços AWS

---

## 💾 EC2 Instance Store (Instance-Level Storage)

O **Instance Store** é o armazenamento local físico que vem acoplado ao host onde sua instância EC2 está rodando. Ele funciona como um disco local, oferecendo performance extremamente alta — mas sem persistência.

---

### 🔍 O que é o Instance Store?

- ✅ **Armazenamento efêmero** (não persistente)
- ✅ Os dados ficam no **host físico** que executa sua instância
- ✅ Funciona como **blocos de armazenamento local** (local SSD/NVMe)

---

### ❌ Quando os dados são perdidos?

**Dados são apagados quando:**

- ❌ A instância é **stopped**
- ❌ A instância é **terminated**
- ❌ O **host físico falha**

Quando essas ações acontecem, todos os blocos do volume são resetados — **não há recuperação**.

---

### ✔️ Dados permanecem quando:

- ✅ A instância é **rebooted** (simples reinicialização)

---

### 🔥 Por que usar algo que pode perder dados?

Mesmo sendo efêmero, o Instance Store tem vantagens muito fortes:

#### 1. Performance Extrema

Ideal para cargas de I/O muito intensas.

Instâncias otimizadas, como **I3**, chegam a:

- ✅ **3.3 milhões de IOPS** de leitura aleatória
- ✅ **1.4 milhões de IOPS** de escrita aleatória

Isso supera facilmente EBS em workloads específicos.

#### 2. Custo Incluso

- ✅ O storage já está **incluso no preço da EC2**
- ✅ Não há cobrança adicional por GB

---

### 🧩 Casos de Uso Ideais

- ✅ **Cache**
- ✅ **Buffer**
- ✅ **Dados temporários**
- ✅ **Armazenamento replicado** entre várias máquinas (ex.: ASG com web servers)
- ✅ **NoSQL de altíssima performance** (mas replicando para storage persistente)

---

### ⚠️ Casos em que você NÃO deve usar Instance Store

- ❌ **Persistência de dados**
- ❌ **Compartilhamento entre instâncias**
- ❌ **Armazenamento crítico**
- ❌ **Quando perda de dados é inaceitável**

**Nesses casos, use EBS ou EFS.**

---

### 📋 Compatibilidade

**Nem todas as instâncias suportam Instance Store**

Alguns tipos suportam, outros não. Sempre confira a documentação.

#### Observações

- ✅ O tamanho e o número de volumes variam conforme o tipo/tamanho da instância
- ✅ Famílias **"Storage-optimized"** costumam ter suporte (ex.: I3, I4, D2, H1 etc.)

---

### 🔒 Segurança

O Instance Store não tem mecanismos próprios de segurança, pois ele:

- ✅ Não é um serviço separado
- ✅ Faz parte da própria EC2
- ✅ Herda controles como políticas IAM e permissões de lançamento

**Se você precisa de criptografia gerenciada, snapshots e persistência: use EBS.**

---

### 🎯 Conclusão sobre Instance Store

O Instance Store é perfeito para:

- ✅ **Velocidade absurda**
- ✅ **Dados temporários**
- ✅ **Workloads distribuídas e replicadas**

Mas **nunca dependa dele para armazenamento permanente** — esse papel é do **Elastic Block Store (EBS)**.

---

## 💾 Amazon EBS (Elastic Block Store)

O **Amazon EBS** fornece armazenamento persistente e durável em nível de bloco para instâncias EC2. Diferente do Instance Store, o EBS não perde dados quando a instância é parada ou reiniciada.

---

### 🔹 Características Principais

#### 1. Persistência

O volume continua existindo mesmo que a EC2 seja:

- ✅ **Stopped**
- ✅ **Rebooted**
- ✅ **Terminated** (se você configurar para não deletar automaticamente)

#### 2. Independência

- ✅ O volume é um **recurso separado**
- ✅ Ele é logicamente **anexado à EC2**
- ✅ Pode ser **desmontado de uma instância** e ligado em outra (mesmo AZ)

---

### 📸 Snapshots

#### O que são

- ✅ **Backups point-in-time** do volume
- ✅ Armazenados no **S3** (durabilidade de S3: 11 "nines")

#### Propriedades

- ✅ **Incrementais** — só copiam o que mudou desde o último snapshot
- ✅ Podem ser **copiados para outras regiões**
- ✅ Permitem **recriar volumes rapidamente** após falhas

---

### 🛡️ Alta Disponibilidade e Resiliência

- ✅ Dados são **replicados dentro da mesma AZ**
- ⚠️ Se a AZ falhar → o volume fica indisponível

#### Recuperação Recomendada

1. Criar volume a partir de snapshot
2. Restaurar em outra AZ

---

### 📊 Tipos de Volumes EBS

O EBS tem duas categorias principais:

- ✅ **SSD** (otimizados para IOPS, blocos menores)
- ✅ **HDD** (otimizados para throughput, blocos maiores)

---

### 💿 SSD (IOPS)

#### gp3 / gp2 – Uso Geral

- ✅ Ideal para boot, banco OLTP, aplicações gerais
- ✅ Balanceia preço e performance

#### io2 / io1 – Provisioned IOPS

- ✅ Projetado para workloads **I/O intensivas**
- ✅ Permite especificar o **IOPS desejado**
- ✅ Entrega o IOPS dentro de **10% de variação, 99.9% do tempo**
- ✅ Ótimo para bancos críticos como Oracle, SQL Server, PostgreSQL

---

### 💾 HDD (Throughput)

#### st1 – Throughput Optimized

Ideal para:

- ✅ Big Data
- ✅ Log processing
- ✅ Stream de dados
- ⚠️ **Não pode ser volume de boot**

#### sc1 – Cold HDD

- ✅ Menor custo
- ✅ Para dados grandes acessados raramente
- ⚠️ **Também não pode ser boot**

---

### 🔒 Segurança – Criptografia EBS

#### Criptografia Nativa

- ✅ **AES-256**
- ✅ Integrada ao **KMS**
- ✅ Você apenas marca a opção "Encrypted" no volume

#### Importante

- ✅ Snapshot de volume criptografado → também é criptografado
- ✅ Volume criado a partir de snapshot criptografado → também é criptografado
- ✅ É possível definir **criptografia padrão por região**

---

### 🛠️ Criação e Gerenciamento

#### Durante o Lançamento da EC2

Você define:

- ✅ Tipo de volume
- ✅ Tamanho
- ✅ Snapshot base (opcional)
- ✅ Criptografia
- ✅ Se o volume será deletado ao terminar a instância

#### Criando Standalone (EC2 Dashboard → EBS → Volumes)

Você escolhe diretamente:

- ✅ AZ
- ✅ Tamanho
- ✅ Tipo de volume
- ✅ Criptografia

**EBS só pode ser anexado a instâncias na mesma AZ.**

---

### 📈 Resize (Elasticidade)

Você pode aumentar o tamanho do volume de duas formas:

1. **Modificando o volume diretamente**
   - Console → Modify Volume

2. **Via snapshot**
   - Snapshot → Create Volume (especificando tamanho maior)

---

### 🎯 Quando Usar EBS vs Outros Storages

| Serviço | Tipo | Quando usar |
|---------|------|-------------|
| **S3** | Object Storage | Arquivos grandes, imutáveis, acessos de leitura alta; vídeos, backups, sites estáticos. |
| **EBS** | Block Storage | Disco de uma única EC2; banco OLTP; boot; baixa latência; leitura/gravação constante. |
| **EFS** | File Storage | Acesso compartilhado por várias EC2; baixa latência; necessidade de sistema de arquivos POSIX. |
| **Instance Store** | Ephemeral Storage | Dados temporários, cache, altíssima performance local. |

---

## 📁 Amazon EFS (Elastic File System)

O **Amazon EFS** é um serviço de file-level storage totalmente gerenciado, projetado para ser compartilhado entre múltiplas instâncias EC2 ao mesmo tempo.

Ele funciona como um sistema de arquivos de rede (NFS), semelhante a um NAS/SAN tradicional, mas com toda a elasticidade e alta disponibilidade do ambiente AWS.

---

### 🎯 Visão Geral

- ✅ **File system compartilhado** e gerenciado
- ✅ **Escala automática** até petabytes
- ✅ **Latência baixa** e throughput elástico
- ✅ Suporta **NFS 4.1 e 4.0**
- ✅ **Sem servidores para administrar** (100% gerenciado)
- ✅ Pode ser **montado em diversas EC2 simultaneamente**
- ✅ **Consistência forte** e suporte a locking
- ✅ **Replicado automaticamente** em múltiplas AZs dentro da mesma região

---

### ⚙️ Como Funciona o EFS

#### Paralelo com Ambientes Tradicionais

**On-premises:**

- Usuários acessam arquivos via:
  - Mapeamento de drive (Windows)
  - NFS (Linux)
  - Servidores de arquivos, SAN, NAS

**Na AWS:**

- Múltiplas EC2 montam o mesmo EFS:
  - Cria-se um **mount target por AZ**
  - Cada instância conecta via **NFS**
  - A visualização é idêntica a um sistema de arquivos normal

**Resultado:**

- A experiência do usuário permanece a mesma, mas com toda a elasticidade e disponibilidade da AWS

---

### 🧩 Casos de Uso

- ✅ **Aplicações distribuídas** acessando os mesmos arquivos
- ✅ **Microserviços** que compartilham assets
- ✅ **Ambientes de machine learning** que usam diretórios compartilhados
- ✅ **CMS, aplicações web e ERP** que precisam de file locking
- ✅ Ambientes com **centenas ou milhares de instâncias** acessando dados simultaneamente
- ✅ **Substituição natural de NAS/SAN** on-premises

---

### 🏗️ Arquitetura e Resiliência

- ✅ **EFS é regional** → acessível em qualquer AZ daquela região
- ✅ Dados **replicados automaticamente** entre várias AZs
- ✅ **Alta disponibilidade nativa**
- ✅ Sem preocupação com manutenção, patching ou dimensionamento de file servers

---

### 🎁 Benefícios Principais

#### 1. Elasticidade Real

- ✅ Aumenta e diminui automaticamente conforme a quantidade de arquivos
- ✅ **Não existe "tamanho do volume"**

#### 2. Acesso Simultâneo

- ✅ Centenas ou milhares de instâncias acessando ao mesmo tempo

#### 3. File System POSIX

Perfeito para aplicações que exigem:

- ✅ Locking
- ✅ Diretórios
- ✅ Permissões
- ✅ Operações de leitura e escrita concorrente

#### 4. Consistência Forte

- ✅ Leitura sempre refletindo a escrita mais recente

---

### ⚠️ Quando NÃO Usar EFS

- ❌ **Alta transferência sequencial e extremamente pesada** → EBS st1/sc1 pode ser melhor
- ❌ **Arquivos raramente acessados e muito grandes** → S3
- ❌ **Necessidade de custo extremamente baixo** → S3 Glacier ou S3 Standard-IA
- ❌ **Precisão e performance de banco de dados relacional** → EBS io2/io1

---

### 📦 Storage Classes do EFS

O EFS oferece duas classes de armazenamento:

#### 1. Standard

- ✅ **Classe padrão**
- ✅ **Latência menor**
- ✅ Cobrança somente por **GB armazenado por mês**
- ✅ Ideal para **dados acessados com frequência**

#### 2. Infrequent Access (IA)

- ✅ Para **dados raramente acessados**
- ✅ **Custo de armazenamento menor**
- ✅ Custo adicional por **read e write**
- ✅ **Maior latência no primeiro byte**
- ✅ Bom para dados históricos, auditorias e análises esporádicas

#### Características em Comum

- ✅ Mesma **disponibilidade e durabilidade** em todas as regiões
- ✅ IA **não move**:
  - Arquivos menores que **128 KB**
  - **Metadados** (sempre ficam em Standard)

---

### 🔄 EFS Lifecycle Management

Funciona de forma semelhante ao lifecycle do S3.

- ✅ Move arquivos automaticamente de **Standard → IA** quando não acessados por:
  - **14, 30, 60 ou 90 dias**
- ✅ Assim que um arquivo é acessado:
  - O **timer é resetado**
  - O arquivo **volta para Standard**
- ✅ Disponível em sistemas criados após **13/02/2019**

---

### ⚡ Performance Modes

Definidos apenas na **criação do file system**.

#### 1. General Purpose (padrão)

- ✅ **Menor latência**
- ✅ Suporta até **7.000 operações por segundo**
- ✅ Ideal para:
  - Home directories
  - File sharing
  - Aplicações comuns

#### 2. Max I/O

- ✅ **Throughput e IOPS praticamente ilimitados**
- ✅ **Latência maior**
- ✅ Indicado quando:
  - Muitas milhares de EC2 acessam o mesmo sistema
  - O limite de **7.000 ops/s é ultrapassado**

#### Métrica Útil

- ✅ **CloudWatch: PercentIOLimit**
  - Permite visualizar quanto das **7.000 ops/s** está sendo usada

---

### 🚀 Throughput Modes

Indicam como o throughput é alocado no EFS.

#### 1. Bursting Throughput (padrão)

- ✅ Throughput cresce conforme o **tamanho do file system**
- ✅ **Regras principais**:
  - **Baseline**: 50 MiB/s por TiB armazenado
  - **Burst**: 100 MiB/s por TiB
- ✅ **Exemplo**:
  - 5 TiB armazenados → burst de até **500 MiB/s**
- ✅ Durante períodos de baixa atividade, o sistema **acumula créditos**
- ✅ **Métrica em CloudWatch**: BurstCreditBalance

#### 2. Provisioned Throughput

- ✅ Quando o sistema exige throughput maior do que o tamanho suportaria
- ✅ É **configurado manualmente**
- ✅ Tem **custo adicional**
- ✅ Indicado quando:
  - Você tem **pouco armazenamento**
  - Mas precisa de **throughput alto e consistente**

---

### 📊 Resumo Rápido: EFS Configurações

| Categoria | Opções | Quando usar |
|-----------|--------|-------------|
| **Storage Class** | Standard / IA | Acesso frequente / raro |
| **Performance Mode** | General Purpose / Max I/O | Latência baixa / milhares de instâncias |
| **Throughput Mode** | Bursting / Provisioned | Crescimento automático / throughput alto garantido |

---

## 🏗️ Componentes do EC2

### Instâncias

Servidores virtuais que executam suas aplicações.

### AMIs (Amazon Machine Images)

Templates pré-configurados que contêm o sistema operacional e software necessário.

### Security Groups

Firewalls virtuais que controlam tráfego de entrada e saída.

### Elastic IPs

Endereços IP estáticos que podem ser associados a instâncias.

---

## 📊 Tipos de Instâncias

### Famílias de Instâncias

- ✅ **General Purpose**: uso geral (T3, T4g, M5, M6i)
- ✅ **Compute Optimized**: workloads intensivos em CPU (C5, C6i)
- ✅ **Memory Optimized**: workloads intensivos em memória (R5, R6i)
- ✅ **Storage Optimized**: workloads intensivos em I/O (I3, I4i, D2)
- ✅ **Accelerated Computing**: GPUs e FPGAs (P3, P4, G4)

---

## 💡 Casos de Uso Comuns

### ✅ Aplicações Web

- ✅ Websites e APIs
- ✅ Aplicações escaláveis
- ✅ Load balancing

### ✅ Processamento de Dados

- ✅ Big Data
- ✅ Processamento batch
- ✅ Análise de dados

### ✅ Desenvolvimento e Testes

- ✅ Ambientes de desenvolvimento
- ✅ Testes automatizados
- ✅ CI/CD

### ✅ HPC (High Performance Computing)

- ✅ Simulações científicas
- ✅ Renderização
- ✅ Modelagem

---

## 🔗 Integração com Outros Serviços

O EC2 integra com diversos serviços AWS:

- ✅ **EBS**: armazenamento persistente
- ✅ **EFS**: sistema de arquivos compartilhado
- ✅ **S3**: armazenamento de objetos
- ✅ **VPC**: rede virtual privada
- ✅ **ELB**: balanceamento de carga
- ✅ **Auto Scaling**: escalabilidade automática
- ✅ **CloudWatch**: monitoramento e logs

---

## 📊 Exemplo de Arquitetura

```
┌─────────────────────────────────────────┐
│  Internet                                │
│         │                                │
│         ▼                                │
│  ┌──────────────────────────────────┐  │
│  │   Application Load Balancer      │  │
│  └──────────────┬───────────────────┘  │
│                 │                       │
│         ┌───────┴────────┐             │
│         │                │             │
│  ┌──────▼──────┐  ┌──────▼──────┐     │
│  │   EC2       │  │   EC2       │     │
│  │ Instance 1  │  │ Instance 2  │     │
│  │             │  │             │     │
│  │ Instance    │  │ Instance    │     │
│  │ Store       │  │ Store       │     │
│  │ (Ephemeral) │  │ (Ephemeral) │     │
│  └──────┬──────┘  └──────┬──────┘     │
│         │                │             │
│         │                │             │
│  ┌──────▼──────┐  ┌──────▼──────┐     │
│  │   EBS       │  │   EBS       │     │
│  │ Volume 1    │  │ Volume 2    │     │
│  │ (Persistent)│  │ (Persistent)│     │
│  └──────┬──────┘  └──────┬──────┘     │
│         │                │             │
│         └────────┬───────┘             │
│                  │                     │
│         ┌────────┴────────┐            │
│         │                 │            │
│  ┌──────▼──────┐  ┌──────▼──────┐    │
│  │   EFS       │  │   EFS       │    │
│  │ (Shared)    │  │ (Shared)    │    │
│  │ Mount       │  │ Mount       │    │
│  │ Target AZ1  │  │ Target AZ2  │    │
│  └──────┬──────┘  └──────┬──────┘    │
│         │                │            │
│         └────────┬───────┘            │
│                  ▼                     │
│  ┌──────────────────────────────────┐  │
│  │   Amazon EFS                     │  │
│  │   • Multi-AZ                     │  │
│  │   • Shared File System           │  │
│  │   • NFS 4.1/4.0                  │  │
│  └──────────────────────────────────┘  │
│                  │                     │
│                  ▼                     │
│  ┌──────────────────────────────────┐  │
│  │   Snapshots (S3)                 │  │
│  │   • Incrementais                 │  │
│  │   • Cross-region                 │  │
│  └──────────────────────────────────┘  │
└──────────────────────────────────────────┘
```

---

## 💰 Custos

### EC2

O EC2 cobra por:

- ✅ **Instâncias**: por hora ou segundo (dependendo do tipo)
- ✅ **Transferência de dados**: saída de dados
- ✅ **Elastic IPs**: quando não associados a instâncias ativas

### EBS

- ✅ **Armazenamento**: por GB provisionado (varia por tipo de volume)
- ✅ **IOPS**: para volumes io1/io2 (Provisioned IOPS)
- ✅ **Snapshots**: por GB armazenado no S3

### EFS

- ✅ **Armazenamento**: por GB armazenado (pay-as-you-go)
- ✅ **Throughput**: por modo de throughput escolhido
- ✅ **Transferência**: entre regiões

> 💡 **Dica**: Use Reserved Instances ou Savings Plans para reduzir custos de EC2

---

## 📝 Comandos Úteis

### AWS CLI - EC2

```bash
# Listar instâncias
aws ec2 describe-instances

# Criar instância
aws ec2 run-instances \
  --image-id ami-xxx \
  --instance-type t3.micro \
  --key-name minha-chave

# Parar instância
aws ec2 stop-instances --instance-ids i-xxx

# Iniciar instância
aws ec2 start-instances --instance-ids i-xxx

# Terminar instância
aws ec2 terminate-instances --instance-ids i-xxx

# Listar tipos de instâncias
aws ec2 describe-instance-types
```

### AWS CLI - EBS

```bash
# Listar volumes EBS
aws ec2 describe-volumes

# Criar volume EBS
aws ec2 create-volume \
  --availability-zone us-east-1a \
  --size 100 \
  --volume-type gp3

# Anexar volume a instância
aws ec2 attach-volume \
  --volume-id vol-xxx \
  --instance-id i-xxx \
  --device /dev/sdf

# Desanexar volume de instância
aws ec2 detach-volume --volume-id vol-xxx

# Criar snapshot
aws ec2 create-snapshot \
  --volume-id vol-xxx \
  --description "Backup diário"

# Listar snapshots
aws ec2 describe-snapshots --owner-ids self

# Criar volume a partir de snapshot
aws ec2 create-volume \
  --snapshot-id snap-xxx \
  --availability-zone us-east-1a

# Modificar tamanho do volume
aws ec2 modify-volume --volume-id vol-xxx --size 200
```

### AWS CLI - EFS

```bash
# Criar file system EFS (General Purpose + Bursting - padrão)
aws efs create-file-system \
  --creation-token meu-efs \
  --performance-mode generalPurpose \
  --throughput-mode bursting

# Criar file system EFS (Max I/O + Provisioned Throughput)
aws efs create-file-system \
  --creation-token meu-efs-maxio \
  --performance-mode maxIO \
  --throughput-mode provisioned \
  --provisioned-throughput-in-mibps 100

# Listar file systems EFS
aws efs describe-file-systems

# Criar mount target (por AZ)
aws efs create-mount-target \
  --file-system-id fs-xxx \
  --subnet-id subnet-xxx \
  --security-groups sg-xxx

# Listar mount targets
aws efs describe-mount-targets --file-system-id fs-xxx

# Criar política de lifecycle (mover para IA após 30 dias)
aws efs put-lifecycle-configuration \
  --file-system-id fs-xxx \
  --lifecycle-policies "TransitionToIA=AFTER_30_DAYS"

# Montar EFS em instância EC2 (Linux)
sudo mount -t efs fs-xxx:/ /mnt/efs

# Montar EFS com EFS helper (recomendado)
sudo mount -t efs -o tls fs-xxx:/ /mnt/efs
```

---

## 🔗 Recursos Adicionais

- [Documentação Oficial Amazon EC2](https://docs.aws.amazon.com/ec2/)
- [Amazon EC2 - Página do Produto](https://aws.amazon.com/ec2/)
- [Tipos de Instâncias EC2](https://aws.amazon.com/ec2/instance-types/)
- [EC2 Instance Store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
- [Amazon EBS - Documentação](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [Tipos de Volumes EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)
- [Amazon EFS - Documentação](https://docs.aws.amazon.com/efs/)
- [Amazon EFS - Página do Produto](https://aws.amazon.com/efs/)
- [EFS Storage Classes](https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html)
- [EFS Performance Modes](https://docs.aws.amazon.com/efs/latest/ug/performance.html)
- [EFS Throughput Modes](https://docs.aws.amazon.com/efs/latest/ug/performance.html#throughput-modes)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o que é EC2 e instâncias
- [ ] Compreender AMIs (Amazon Machine Images)
- [ ] Entender Security Groups e controle de tráfego
- [ ] Conhecer Elastic IPs
- [ ] Entender Instance Store e suas características
- [ ] Saber quando os dados do Instance Store são perdidos
- [ ] Compreender quando usar Instance Store vs EBS
- [ ] Conhecer casos de uso ideais do Instance Store
- [ ] Entender limitações e riscos do Instance Store
- [ ] Saber quais tipos de instâncias suportam Instance Store
- [ ] Compreender diferença entre Instance Store e EBS
- [ ] Entender Amazon EBS e suas características principais
- [ ] Compreender persistência e independência dos volumes EBS
- [ ] Entender snapshots EBS (incrementais, armazenados no S3)
- [ ] Conhecer tipos de volumes SSD (gp3, gp2, io1, io2)
- [ ] Conhecer tipos de volumes HDD (st1, sc1)
- [ ] Entender quando usar cada tipo de volume EBS
- [ ] Compreender criptografia EBS (AES-256, integração com KMS)
- [ ] Entender que EBS só pode ser anexado a instâncias na mesma AZ
- [ ] Saber como aumentar tamanho de volumes EBS (resize)
- [ ] Entender Amazon EFS e suas características principais
- [ ] Compreender que EFS é um file system compartilhado (NFS)
- [ ] Entender que EFS é regional e replicado em múltiplas AZs
- [ ] Conhecer casos de uso ideais do EFS (aplicações distribuídas, microserviços, ML)
- [ ] Entender benefícios do EFS (elasticidade real, acesso simultâneo, POSIX)
- [ ] Saber quando NÃO usar EFS (alta transferência sequencial, arquivos grandes raramente acessados)
- [ ] Entender mount targets e como montar EFS em instâncias EC2
- [ ] Conhecer Storage Classes do EFS (Standard e Infrequent Access)
- [ ] Entender EFS Lifecycle Management (movimento automático Standard → IA)
- [ ] Compreender Performance Modes (General Purpose vs Max I/O)
- [ ] Entender Throughput Modes (Bursting vs Provisioned)
- [ ] Saber quando usar cada configuração de Performance e Throughput Mode
- [ ] Conhecer métricas CloudWatch importantes (PercentIOLimit, BurstCreditBalance)
- [ ] Compreender diferença entre EBS, Instance Store, S3 e EFS
- [ ] Conhecer famílias de instâncias (General Purpose, Compute Optimized, etc.)
- [ ] Entender integração com outros serviços AWS

---

## 🏷️ Tags

`#aws` `#compute` `#ec2` `#instances` `#storage` `#instance-store` `#ebs` `#elastic-block-store` `#efs` `#elastic-file-system` `#nfs` `#virtualization` `#cloud-computing`

---

**Última atualização**: 📅 [DD/MM/YYYY]

