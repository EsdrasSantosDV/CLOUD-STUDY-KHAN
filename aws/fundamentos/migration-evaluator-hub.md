# 📘 Migration Evaluator e AWS Migration Hub — Explicação Completa

> Serviços essenciais para a fase Assess da migração para AWS

---

## 📌 Resumo

Nesta aula, vamos entender em detalhes o que são o **Migration Evaluator** e o **AWS Migration Hub**, e em quais cenários você utilizaria cada um deles durante a fase **Assess** do seu plano de migração para a AWS.

---

## 🟦 Migration Evaluator

O **Migration Evaluator** era anteriormente conhecido como **TSO Logic**, adquirido pela AWS em 2018.

Ele faz parte do estágio de avaliação da jornada de migração para a nuvem.

**A função principal do Migration Evaluator** é coletar dados do seu ambiente on-premises e criar um baseline realista do que existe hoje, incluindo:

- ✅ Compute
- ✅ Storage
- ✅ Licenças Microsoft

Com base nesse inventário, o serviço identifica quais seriam as opções mais otimizadas em custo na AWS para cada workload analisado.

---

### 🔍 O que o Migration Evaluator faz?

Ele avalia seu ambiente atual e recomenda tamanhos ideais de EC2 (right-sizing) considerando:

- ✅ Utilização de CPU
- ✅ Idade do processador
- ✅ Quantidade de RAM
- ✅ Sistema operacional
- ✅ Tipo de licença Microsoft

Com base nisso, ele calcula projeções de custos na AWS e pode apontar reduções de até **50%** em comparação ao ambiente atual.

---

### 🛠️ Como os dados são coletados?

O Migration Evaluator usa um **coletor agentless** que obtém:

- ✅ Inventário de servidores
- ✅ Métricas de performance (time-series)
- ✅ Configurações do ambiente

Ele também aceita dados vindos de ferramentas de terceiros.

**Importante:**

A AWS recomenda coletar dados por no mínimo **2 semanas** para garantir uma análise mais fiel.

Depois da coleta, os dados podem ser enviados automaticamente para o AWS Migration Hub.

---

### 📄 Entregáveis do Migration Evaluator

Após a ingestão dos dados, o serviço gera:

#### ✔️ Quick Insights (em algumas horas)

**Inclui:**

- ✅ Recomendações de otimização
- ✅ Projeções de custo pós-migração
- ✅ Alertas sobre gaps de recursos
- ✅ Avaliações de workloads

#### ✔️ Business Case completo

- ✅ Voltado ao público executivo

#### ✔️ Análises técnicas detalhadas por servidor

- ✅ Para arquitetos e engenheiros avaliarem cenários e custos

---

### 📬 Como solicitar o Migration Evaluator?

- ✅ Fale com seu gerente de conta AWS
- ✅ Ou solicite diretamente: https://aws.amazon.com/migration-evaluator/

---

## 🟩 AWS Migration Hub

O **AWS Migration Hub** é um painel central que permite planejar, acompanhar e gerenciar um projeto de migração.

**Ele oferece uma visão consolidada de:**

- ✅ Servidores
- ✅ Aplicações
- ✅ Localizações físicas
- ✅ Status de migração
- ✅ Ferramentas em uso

É especialmente útil em grandes migrações, onde há dezenas ou centenas de workloads distribuídos em vários data centers.

---

### 🧠 Como funciona o Migration Hub?

Ele funciona como o **centro de controle da migração**, integrando dados de descoberta e progresso vindos de diversas fontes:

---

### 📥 Fontes de inventário suportadas pelo Migration Hub:

#### Migration Hub Import

- ✅ Importação manual de dados de inventário da sua infraestrutura

#### Migration Evaluator Collector

- ✅ Os dados coletados pelo Migration Evaluator podem ser consumidos pelo Hub

#### AWS Agentless Discovery Connector

Uma appliance VMware que analisa todo o seu ambiente vSphere.

**Coleta:**

- ✅ Configurações de VMs
- ✅ Utilização
- ✅ Volume de discos
- ✅ Dependências

#### AWS Application Discovery Agent

Um agente instalado diretamente nos seus servidores físicos ou VMs.

**Coleta:**

- ✅ Configurações do sistema
- ✅ Performance
- ✅ Processos em execução
- ✅ Conexões de rede

---

### 🔎 O que o Migration Hub fornece com esses dados?

- ✅ Número e tipos de servidores
- ✅ Aplicações e seus grupos
- ✅ Dependências entre sistemas
- ✅ Análise técnica detalhada
- ✅ Agrupamento de servers por aplicação

**Isso é essencial para definir a estratégia correta** (Rehost, Refactor, etc).

---

### 🚀 Migração a partir do Migration Hub

Após a fase de descoberta, você escolhe a ferramenta de migração diretamente pelo Migration Hub:

**Para migrar servidores:**

- ✅ AWS Application Migration Service (MGN)

**Para migrar bancos de dados:**

- ✅ AWS Database Migration Service (DMS)

O Hub então monitora o progresso da migração e centraliza tudo em um único painel.

---

## 📊 Comparação: Migration Evaluator vs Migration Hub

| Aspecto | Migration Evaluator | AWS Migration Hub |
|---------|---------------------|-------------------|
| **Foco** | Análise de custos e right-sizing | Central de controle da migração |
| **Quando usar** | No começo do projeto, para montar business case | Durante todo o processo de migração |
| **Entregáveis** | Projeções de custo, Quick Insights, Business Case | Inventário consolidado, status de migração |
| **Coleta de dados** | Coletor agentless próprio | Múltiplas fontes (Import, Evaluator, Discovery Connector, Agent) |
| **Público-alvo** | Executivos (business case) e técnicos (análises) | Equipe de migração completa |
| **Integração** | Envia dados para Migration Hub | Recebe dados de múltiplas fontes |

---

## 🎯 Quando Usar Cada Serviço

### Migration Evaluator

**Use quando:**

- ✅ Precisa construir business case financeiro para aprovação executiva
- ✅ Quer estimar custos de migração antes de começar
- ✅ Precisa de recomendações de right-sizing para EC2
- ✅ Quer comparar custos on-premises vs AWS
- ✅ Precisa identificar oportunidades de economia (até 50%)

### AWS Migration Hub

**Use quando:**

- ✅ Está gerenciando migração de múltiplos workloads
- ✅ Precisa de visão centralizada do projeto de migração
- ✅ Quer consolidar dados de múltiplas fontes de descoberta
- ✅ Precisa acompanhar progresso de migração em tempo real
- ✅ Está trabalhando com grandes projetos (dezenas/centenas de servidores)
- ✅ Quer integrar diferentes ferramentas de migração em um único painel

---

## 🔄 Fluxo de Trabalho Integrado

```
┌─────────────────────────────────────────────────────────┐
│              FASE ASSESS                                 │
│                                                          │
│  ┌──────────────────────────────────────┐              │
│  │   Migration Evaluator                 │              │
│  │   • Coleta dados (2+ semanas)         │              │
│  │   • Análise de custos                 │              │
│  │   • Right-sizing                      │              │
│  │   • Business Case                     │              │
│  └──────────────┬───────────────────────┘              │
│                 │                                       │
│                 ▼                                       │
│  ┌──────────────────────────────────────┐              │
│  │   AWS Migration Hub                   │              │
│  │   • Recebe dados do Evaluator         │              │
│  │   • Consolida inventário              │              │
│  │   • Agrupa por aplicação              │              │
│  │   • Identifica dependências           │              │
│  │   • Centraliza visão do projeto       │              │
│  └──────────────┬───────────────────────┘              │
│                 │                                       │
│                 ▼                                       │
│  ┌──────────────────────────────────────┐              │
│  │   Escolha de Ferramenta de Migração   │              │
│  │   • MGN (servidores)                   │              │
│  │   • DMS (bancos de dados)             │              │
│  │   • Monitoramento no Hub               │              │
│  └───────────────────────────────────────┘              │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Recursos Adicionais

- [Migration Evaluator](https://aws.amazon.com/migration-evaluator/)
- [AWS Migration Hub](https://aws.amazon.com/migration-hub/)
- [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/)
- [AWS Application Migration Service (MGN)](https://aws.amazon.com/application-migration-service/)
- [AWS Database Migration Service (DMS)](https://aws.amazon.com/dms/)
- [Migration Hub Documentation](https://docs.aws.amazon.com/migrationhub/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o papel do Migration Evaluator na fase Assess
- [ ] Compreender como o Migration Evaluator coleta dados
- [ ] Conhecer os entregáveis do Migration Evaluator (Quick Insights, Business Case)
- [ ] Entender o papel do AWS Migration Hub como central de controle
- [ ] Conhecer as fontes de dados suportadas pelo Migration Hub
- [ ] Entender diferença entre Migration Evaluator e Migration Hub
- [ ] Saber quando usar cada serviço
- [ ] Compreender como os serviços se integram
- [ ] Entender como escolher ferramentas de migração a partir do Hub

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#migration-evaluator` `#migration-hub` `#assess` `#tco` `#right-sizing` `#business-case`

---

## 🎯 Resumo Final

| Serviço | Papel | Quando usar |
|---------|-------|-------------|
| **Migration Evaluator** | Análise de infraestrutura, right-sizing, projeções de custo | No começo do projeto, para montar business case e estimar custos |
| **AWS Migration Hub** | Central de controle da migração, inventário, progresso | Durante todo o processo de migração, especialmente em grandes projetos |

---

**Última atualização**: 📅 [DD/MM/YYYY]

