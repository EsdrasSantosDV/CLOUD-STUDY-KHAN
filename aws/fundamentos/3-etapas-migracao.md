# 🛠️ Migração para AWS em 3 Etapas

> Processo estruturado: Assess → Mobilize → Migrate & Modernize

---

## 📌 Resumo

Quando você planeja migrar um data center on-premises para a AWS, o processo se torna muito mais eficiente quando dividido em **3 estágios**.

A AWS estrutura seus serviços de migração exatamente nessas fases para te guiar de forma organizada e estratégica.

---

## 🟦 1. Assess (Avaliar)

Primeiro, você avalia sua preparação e maturidade para migrar.

**Aqui você entende:**

- ✅ Como está seu ambiente atual
- ✅ Quais workloads existem
- ✅ Custos atuais
- ✅ Riscos
- ✅ Dependências
- ✅ E se sua equipe está pronta para a mudança

**O objetivo dessa fase é:**

- ✅ Entender profundamente o ambiente
- ✅ Definir metas claras
- ✅ Montar um business case sólido para a liderança aprovar o projeto

---

### 🔧 Serviços da AWS usados nessa fase:

#### Migration Evaluator

- ✅ Ajuda a estimar custos de rodar suas aplicações na AWS
- ✅ Gera um relatório TCO (Total Cost of Ownership)
- ✅ Excelente para construir o case financeiro para migração

#### AWS Migration Hub

- ✅ Centraliza informações sobre workloads, inventários e status da migração
- ✅ Funciona como "painel" inicial para o projeto

---

## 🟩 2. Mobilize (Mobilizar)

Após entender o ambiente atual, você começa a definir de forma detalhada:

- ✅ O plano de migração
- ✅ As estratégias por aplicação
- ✅ Os riscos
- ✅ As lacunas de habilidades
- ✅ E quais dos 7 Rs você usará em cada workload

**Os 7 Rs são:**

1. Relocate
2. Rehost (Lift & Shift)
3. Replatform
4. Refactor/Re-architect
5. Repurchase
6. Retire
7. Retain

**Nesta fase você cria o Migration Readiness & Planning (MRP)**, entendendo dependências técnicas e organizacionais.

---

### 🔧 Serviços da AWS envolvidos:

#### AWS Application Discovery Service

- ✅ Descobre servidores, aplicações, dependências e inventário automaticamente
- ✅ Ajuda a entender o que realmente existe no on-premises antes de migrar

#### AWS Control Tower

- ✅ Ajuda a estabelecer a governança inicial, criando múltiplas contas AWS com segurança e boas práticas embutidas
- ✅ Ideal para escalar para centenas de workloads

---

## 🟥 3. Migrate & Modernize (Migrar e Modernizar)

Agora começa o trabalho real: você move e moderniza os workloads.

**Aqui você:**

- ✅ Projeta como cada aplicação ficará na AWS
- ✅ Define quais serviços AWS serão usados
- ✅ Realiza testes
- ✅ Valida a arquitetura
- ✅ Executa a migração final

**A estratégia usada (um dos 7 Rs) vai definir qual serviço de migração será mais adequado.**

A AWS divide esses serviços em três categorias:

---

### 🖥️ Serviços para Migrar Servidores, Aplicações e VMs

#### AWS Application Migration Service (MGN)

- ✅ Principal ferramenta de Lift & Shift moderno
- ✅ Converte servidores on-premises em instâncias EC2 automaticamente
- ✅ Alta automação, baixo risco

#### AWS Database Migration Service (DMS)

- ✅ Migra bancos relacionais e não relacionais para a AWS
- ✅ Suporta replicação contínua
- ✅ Funciona tanto para migração homogênea (ex: MySQL → MySQL) quanto heterogênea (ex: SQL Server → Aurora)

---

### 📤 Serviços para Migrar Dados

#### AWS DataSync

- ✅ Move grandes volumes de dados rapidamente com otimização de rede
- ✅ Perfeito para migrações contínuas ou sincronização entre data centers e AWS

#### AWS Transfer Family

- ✅ Fornece protocolos SFTP, FTP e FTPS totalmente gerenciados
- ✅ Ideal para substituir servidores FTP antigos

#### AWS Snow Family

Inclui Snowcone, Snowball e Snowmobile:

- ✅ Usados quando você precisa mover petabytes ou exabytes de dados
- ✅ Ou quando a rede é lenta/difícil

#### AWS Storage Gateway

- ✅ Integra ambientes on-premises com storage AWS
- ✅ Ideal para cenários híbridos durante a migração

---

### 🎯 Serviços para Modernização

#### AWS Service Catalog

- ✅ Permite padronizar e disponibilizar produtos (infraestrutura e aplicações) já aprovados pela empresa
- ✅ Facilita modernização pós-migração

---

## 📊 Resumo Visual das 3 Etapas

| Etapa | Objetivo | Serviços Envolvidos |
|-------|----------|---------------------|
| **Assess** | Entender ambiente atual e criar business case | Migration Evaluator, Migration Hub |
| **Mobilize** | Definir plano detalhado, estratégias e governança | App Discovery Service, Control Tower |
| **Migrate & Modernize** | Migrar workloads e modernizar aplicações | MGN, DMS, DataSync, Snow, Transfer, Storage Gateway |

---

## 🔄 Fluxo Completo da Migração

```
┌─────────────────────────────────────────────────────────┐
│                    ASSESS                                │
│  • Entender ambiente atual                               │
│  • Avaliar custos (TCO)                                  │
│  • Identificar riscos e dependências                     │
│  • Criar business case                                   │
│                                                          │
│  Serviços: Migration Evaluator, Migration Hub           │
└──────────────────┬──────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────────┐
│                   MOBILIZE                               │
│  • Criar plano de migração detalhado                    │
│  • Definir estratégias (7 Rs) por workload              │
│  • Estabelecer governança (Control Tower)               │
│  • Descobrir inventário (App Discovery)                 │
│                                                          │
│  Serviços: App Discovery Service, Control Tower         │
└──────────────────┬──────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────────┐
│              MIGRATE & MODERNIZE                         │
│  • Migrar servidores (MGN)                              │
│  • Migrar bancos de dados (DMS)                        │
│  • Migrar dados (DataSync, Snow, Transfer)             │
│  • Modernizar aplicações                                │
│  • Estabelecer padrões (Service Catalog)               │
│                                                          │
│  Serviços: MGN, DMS, DataSync, Snow, Transfer, etc.     │
└──────────────────────────────────────────────────────────┘
```

---

## 🎯 Quando Usar Cada Serviço

### Migration Evaluator
- ✅ Quando precisa construir business case financeiro
- ✅ Para estimar TCO antes de migrar
- ✅ Para comparar custos on-premises vs AWS

### Migration Hub
- ✅ Para centralizar visão de todos os workloads
- ✅ Para acompanhar status da migração
- ✅ Para gerenciar múltiplos projetos de migração

### Application Discovery Service
- ✅ Para descobrir automaticamente o que existe no ambiente
- ✅ Para entender dependências entre aplicações
- ✅ Para criar inventário preciso antes da migração

### Control Tower
- ✅ Para estabelecer governança multi-conta
- ✅ Para aplicar políticas de segurança e compliance
- ✅ Para escalar migração para centenas de workloads

### Application Migration Service (MGN)
- ✅ Para migração Lift & Shift de servidores
- ✅ Para converter VMs em instâncias EC2
- ✅ Quando precisa de alta automação e baixo risco

### Database Migration Service (DMS)
- ✅ Para migrar bancos relacionais (MySQL, PostgreSQL, Oracle, SQL Server)
- ✅ Para migração contínua com replicação
- ✅ Para migração homogênea ou heterogênea

### DataSync
- ✅ Para mover grandes volumes de dados pela rede
- ✅ Para sincronização contínua entre on-premises e AWS
- ✅ Quando a rede é adequada para transferência

### AWS Snow Family
- ✅ Para mover petabytes ou exabytes de dados
- ✅ Quando a rede é lenta ou cara
- ✅ Para migração de dados offline

### Transfer Family
- ✅ Para substituir servidores FTP/SFTP antigos
- ✅ Para migração de arquivos via protocolos padrão
- ✅ Quando clientes precisam manter protocolos existentes

### Storage Gateway
- ✅ Para integração híbrida durante migração
- ✅ Para cache local de dados S3
- ✅ Para backup on-premises para AWS

### Service Catalog
- ✅ Para padronizar infraestrutura aprovada
- ✅ Para facilitar self-service de recursos
- ✅ Para governança pós-migração

---

## 🔗 Recursos Adicionais

- [AWS Migration Hub](https://aws.amazon.com/migration-hub/)
- [Migration Evaluator](https://aws.amazon.com/migration-evaluator/)
- [Application Discovery Service](https://aws.amazon.com/application-discovery/)
- [AWS Control Tower](https://aws.amazon.com/controltower/)
- [Application Migration Service (MGN)](https://aws.amazon.com/application-migration-service/)
- [Database Migration Service (DMS)](https://aws.amazon.com/dms/)
- [AWS DataSync](https://aws.amazon.com/datasync/)
- [AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/)
- [AWS Snow Family](https://aws.amazon.com/snow/)
- [AWS Storage Gateway](https://aws.amazon.com/storagegateway/)
- [AWS Service Catalog](https://aws.amazon.com/servicecatalog/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender as 3 etapas da migração (Assess, Mobilize, Migrate & Modernize)
- [ ] Conhecer serviços da fase Assess (Migration Evaluator, Migration Hub)
- [ ] Entender fase Mobilize e criação do MRP
- [ ] Conhecer serviços da fase Mobilize (App Discovery, Control Tower)
- [ ] Entender fase Migrate & Modernize
- [ ] Conhecer serviços para migrar servidores (MGN)
- [ ] Conhecer serviços para migrar bancos de dados (DMS)
- [ ] Conhecer serviços para migrar dados (DataSync, Snow, Transfer)
- [ ] Entender quando usar cada serviço de migração
- [ ] Compreender relação entre 7 Rs e serviços de migração

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#assess` `#mobilize` `#migrate` `#modernize` `#mgn` `#dms` `#datasync` `#snow` `#migration-hub`

---

## 🎯 Conclusão

As 3 etapas da migração (Assess → Mobilize → Migrate & Modernize) fornecem uma estrutura clara e organizada para migrar workloads do on-premises para a AWS.

Cada etapa tem objetivos específicos e serviços AWS dedicados que facilitam o processo, reduzindo riscos e acelerando a transformação digital.

---

**Última atualização**: 📅 [DD/MM/YYYY]

