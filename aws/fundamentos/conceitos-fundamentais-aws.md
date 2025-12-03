# 📘 Conceitos Fundamentais da AWS — Explicação Direta e Técnica

> Conceitos essenciais que formam a base do conhecimento AWS

---

## 📌 Resumo

Este documento cobre os conceitos fundamentais da AWS que todo profissional precisa conhecer para trabalhar efetivamente na plataforma, desde contas e billing até instâncias EC2 e arquitetura global.

---

## 🏢 Account (AWS)

Uma **Account** é a unidade básica onde todos os serviços AWS existem.

**Ela pode ser:**

- ✅ **Master/Payer Account:** centraliza faturas, consolida billing
- ✅ **Linked Account:** conta vinculada que não possui billing próprio

Através de **AWS Organizations**, é possível gerenciar múltiplas contas, aplicar políticas (SCPs) e estruturar ambientes corporativos de forma isolada. Vários serviços da AWS conseguem operar atravessando fronteiras de contas.

---

## 💰 Reserved Instance (RI)

**Compromisso de uso** de um recurso específico por 1 ou 3 anos, recebendo um desconto relevante em troca dessa previsão de uso.

**Restrições comuns:**

- ⚠️ Tipo específico de instância
- ⚠️ Região fixada
- ⚠️ Tamanho e plataforma definidos

---

## 💰 Savings Plans (SP)

Parecidos com RIs, porém **mais flexíveis** e aplicáveis ao consumo de compute, principalmente EC2, Fargate e Lambda.

Em vez de reservar um tipo de instância, você se compromete com um **valor/hora**.

---

## 💳 AURI, PURI, NURI (formas de pagamento de RI/SP)

Você ouvirá esses termos:

- ✅ **AURI (All Upfront RI)** – pagamento total antecipado
- ✅ **PURI (Partial Upfront RI)** – pagamento parcial antecipado
- ✅ **NURI (No Upfront RI)** – sem pagamento antecipado

**Quanto maior o adiantamento, maior o desconto.**

---

## 🖥️ EC2 — Elastic Compute Cloud

Serviço de computação virtual da AWS.

É o **"servidor virtual" (VM) clássico** da nuvem da Amazon.

---

## 🏷️ Instância, Tipo, Família, Geração e Tamanho

Uma **instância EC2** é uma VM específica.

O **tipo completo** descreve:

- ✅ **Família** (ex: m, c, r, t, p …)
- ✅ **Geração** (número: 5, 6, 7…)
- ✅ **Sufixos** (ex: a para AMD, n para Nitro, g para Graviton)
- ✅ **Tamanho** (nano, micro, small, medium, large, xlarge, 2xlarge…)

**Exemplo:**

`m5a.4xlarge` → família M, geração 5, AMD, tamanho 4xlarge.

---

## 🔐 IAM — Identity and Access Management

Sistema de controle de identidade, autenticação e permissões da AWS.

**IAM define:**

- ✅ **Usuários**
- ✅ **Grupos**
- ✅ **Funções (Roles)**
- ✅ **Políticas (Policies)**

É o alicerce de segurança de qualquer ambiente AWS.

---

## 🏷️ Tags

**Metadados** aplicados a recursos AWS.

Servem para organizar, identificar e automatizar.

**Exemplos de uso:**

- ✅ Ambiente (dev, qa, prod)
- ✅ Owner
- ✅ Cost center
- ✅ Aplicação
- ✅ Governança (mandatory tags)

**Vantagens:**

- ✅ Permitem rastrear custos
- ✅ Servem como gatilho para automatizações
- ✅ Facilitam auditoria e organização

---

## 🖥️ Console (AWS Management Console)

Painel web oficial para gerenciar contas e serviços.

É a forma visual e intuitiva de navegar na AWS.

---

## 💰 Standard RI / Convertible RI

AWS define dois tipos de RIs:

- ✅ **Standard RI** → maior desconto, porém não pode ser convertida
- ✅ **Convertible RI** → permite ajustes (como mudar tipo de instância), mas o desconto é menor

---

## 🌍 Region

**Localização física** da AWS espalhada pelo mundo.

Cada região contém múltiplas Availability Zones.

**Características:**

- ✅ Distâncias planejadas entre regiões para atender DR (disaster recovery)
- ✅ Segregação geográfica e jurídica (importante para compliance)

---

## 🏢 Availability Zones (AZs)

**Subunidades** dentro de uma Região.

Cada AZ é composta por múltiplos data centers altamente conectados e com baixa latência.

**Uma boa prática crítica na AWS:**

→ **Distribuir workloads entre múltiplas AZs para garantir alta disponibilidade.**

---

## 🔄 DevOps

**DevOps** é um conjunto de práticas que elimina os silos tradicionais entre desenvolvimento e operações.

A ideia é simples: equipes que antes trabalhavam separadas passam a atuar em conjunto, automatizando, padronizando e acelerando o ciclo de entrega de software.

**O foco central é:**

- ✅ Integração contínua (CI)
- ✅ Entrega contínua (CD)
- ✅ Automação de pipelines
- ✅ Colaboração entre times historicamente separados

O objetivo final é entregar software de forma mais rápida, previsível e confiável.

---

## 🏗️ Enterprise Architecture (EA)

A área de **Arquitetura Corporativa** define a "planta baixa" dos sistemas da empresa.

**Funções típicas:**

- ✅ Desenhar como sistemas se conectam
- ✅ Especificar componentes, padrões e tecnologias
- ✅ Garantir que soluções sigam uma visão estratégica

Assim como arquitetos civis definem materiais, estruturas e normas, a EA define a base tecnológica para que os times construam sistemas consistentes ao longo dos anos.

---

## 🚚 Lift & Shift

**Modelo clássico de migração:**

👉 Você move o sistema exatamente como está do on-premises para a nuvem, sem reescrever e sem adotar serviços nativos de cloud.

**Vantagens:**

- ✅ Velocidade
- ✅ Baixo impacto inicial
- ✅ Permite fechar um data center rapidamente

**Desvantagens:**

- ⚠️ Custos maiores na nuvem
- ⚠️ Baixa eficiência operacional
- ⚠️ Desperdício de recursos
- ⚠️ Perde-se grande parte do valor do cloud

Por isso, sempre deve existir um período de remediação após o Lift & Shift para corrigir problemas, otimizar recursos e começar a usar recursos realmente nativos da nuvem.

---

## 💼 Workload

**Workload** é um termo genérico que designa qualquer aplicação ou sistema rodando em algum ambiente.

**Exemplo clássico de um site tradicional:**

- ✅ Web server
- ✅ Application server
- ✅ Database server

Cada componente é um workload operando em sua própria VM ou servidor físico.

---

## 🏢 On-Premises (On-Prem)

Infraestrutura própria ou controlada pela empresa — racks, servidores, switches, armazenamento, virtualização, energia, climatização, toda a operação física.

Representa anos de investimento, processos e ferramentas construídas internamente.

Por isso, migrar para a nuvem frequentemente causa atrito — as práticas e ferramentas criadas para on-prem muitas vezes não funcionam ou não se encaixam no modelo de cloud.

---

## ⚖️ Rightsizing

**Processo de ajuste fino** dos recursos usados por uma aplicação para alcançar um equilíbrio entre:

- ✅ Desempenho
- ✅ Custo
- ✅ Eficiência

Envolve coletar métricas ao longo do tempo e selecionar instâncias, tamanhos e configurações adequadas para cada workload.

É uma das ações mais eficazes em otimização de custos, mas precisa de supervisão técnica para não degradar performance.

---

## 🏃 Agile

**Metodologia de desenvolvimento** baseada em:

- ✅ Ciclos curtos (sprints)
- ✅ Entregas incrementais
- ✅ Priorização contínua
- ✅ Adaptação rápida

Normalmente começa com **MVP (Minimum Viable Product)** e evolui o produto através de um backlog vivo, atualizado conforme feedback dos usuários e prioridades de negócio.

Agile funciona bem em conjunto com DevOps, acelerando tanto construção quanto entrega.

---

## 📊 Famílias de Instâncias EC2

### Família M (General Purpose)

- ✅ Balanceamento entre CPU, memória e rede
- ✅ Uso geral para aplicações variadas

### Família C (Compute Optimized)

- ✅ Alto desempenho de CPU
- ✅ Ideal para processamento intensivo

### Família R (Memory Optimized)

- ✅ Alta capacidade de memória RAM
- ✅ Ideal para bancos de dados e análises em memória

### Família T (Burstable Performance)

- ✅ Performance burstável com baseline
- ✅ Ideal para workloads variáveis

### Família P (Accelerated Computing)

- ✅ GPUs para processamento paralelo
- ✅ Ideal para ML, renderização, HPC

---

## 🔗 Recursos Adicionais

- [Documentação AWS - Conceitos Fundamentais](https://docs.aws.amazon.com/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Organizations](https://aws.amazon.com/organizations/)
- [AWS Pricing Calculator](https://calculator.aws/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito de Account AWS e tipos (Master/Payer, Linked)
- [ ] Compreender Reserved Instances e Savings Plans
- [ ] Entender formas de pagamento (AURI, PURI, NURI)
- [ ] Conhecer diferença entre Standard RI e Convertible RI
- [ ] Entender estrutura de instâncias EC2 (família, geração, tamanho)
- [ ] Compreender IAM e seus componentes (usuários, grupos, roles, policies)
- [ ] Entender o uso de Tags para organização e governança
- [ ] Conhecer AWS Management Console
- [ ] Entender conceito de Region e Availability Zones
- [ ] Compreender importância de distribuir workloads entre múltiplas AZs
- [ ] Entender conceito de DevOps e suas práticas (CI/CD)
- [ ] Compreender Enterprise Architecture e seu papel
- [ ] Entender modelo de migração Lift & Shift e suas limitações
- [ ] Compreender conceito de Workload
- [ ] Entender On-Premises e desafios de migração
- [ ] Compreender processo de Rightsizing
- [ ] Entender metodologia Agile e sua relação com DevOps

---

## 🏷️ Tags

`#aws` `#fundamentos` `#conceitos-basicos` `#account` `#ec2` `#iam` `#regions` `#availability-zones` `#reserved-instances` `#savings-plans` `#tags` `#devops` `#agile` `#lift-and-shift` `#workload` `#on-premises` `#rightsizing` `#enterprise-architecture`

---

**Última atualização**: 📅 [DD/MM/YYYY]

