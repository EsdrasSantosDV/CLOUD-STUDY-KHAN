# 🟩 Estágio Mobilize da Migração para AWS

> Serviços: AWS Application Discovery Service e AWS Control Tower

---

## 📌 Resumo

No estágio **Mobilize**, o foco está em planejar sua migração com mais detalhe, identificando dependências, requisitos e lacunas antes de executar a mudança.

Para isso, dois serviços da AWS são fundamentais:

- ✅ **AWS Application Discovery Service**
- ✅ **AWS Control Tower**

---

## 🟦 AWS Application Discovery Service

Esse serviço dá continuidade ao estágio Assess, permitindo aprofundar a análise dos seus workloads on-premises.

**Ele coleta informações como:**

- ✅ Uso dos recursos
- ✅ Configurações
- ✅ Comportamento das aplicações
- ✅ Dependências entre servidores e serviços

**Todo dado coletado é criptografado**, garantindo segurança. Além disso, essas informações podem ser enviadas para o AWS Migration Hub, ou exportadas para ferramentas como Amazon Athena e Amazon QuickSight, o que te ajuda a avaliar o TCO (Total Cost of Ownership) da migração.

---

### 🛠️ Como o Application Discovery Service coleta dados?

Existem duas formas de descoberta:

---

### 🔹 1. Descoberta com agente (Agent-based)

Você instala um agente da AWS diretamente nos servidores que deseja analisar.

**Ele pode ser instalado em:**

- ✅ Linux
- ✅ Windows
- ✅ Servidores físicos
- ✅ Máquinas virtuais

**O agente coleta:**

- ✅ Dados de configuração
- ✅ Performance do sistema
- ✅ Conexões de rede
- ✅ Processos em execução

Com isso, você consegue mapear dependências entre serviços.

#### 🔄 Funcionamento

Depois de instalado, o agente:

- ✅ Se registra no Application Discovery Service na região configurada
- ✅ Integra-se ao AWS Migration Hub
- ✅ Envia dados criptografados via TLS a cada 15 minutos

---

### 🔹 2. Descoberta sem agente (Agentless) — Discovery Connector

Esse método é exclusivo para ambientes VMware.

Você implanta um **OVA (appliance virtual)** no vCenter.

**Ele coleta:**

- ✅ Informações de cada VM
- ✅ Configuração
- ✅ IP e MAC
- ✅ Alocação de disco
- ✅ Utilização de CPU, RAM e I/O
- ✅ Picos médios de uso

#### 🔄 Funcionamento

Após implantado:

- ✅ O Connector integra-se ao Migration Hub e ao Discovery Service
- ✅ Envia dados criptografados via TLS a cada 60 minutos

---

## 🟩 AWS Control Tower

O **AWS Control Tower** é essencial quando a migração envolve múltiplas contas.

Ele simplifica a criação e padronização de um ambiente multi-conta com governança embutida.

A partir dele, você cria uma **Landing Zone** — um ambiente multi-conta baseado em boas práticas da AWS, especialmente nos pilares:

- ✅ Segurança
- ✅ Compliance
- ✅ Operações
- ✅ Governança

---

### 🏛️ O que é uma Landing Zone?

É uma arquitetura multi-conta construída automaticamente pelo Control Tower, seguindo o Well-Architected Framework e implementando governança desde o início.

**A Landing Zone cria três Organizational Units (OUs) dentro do AWS Organizations:**

---

### 1️⃣ Root OU

A OU "mãe", que contém todas as demais.

---

### 2️⃣ Core OU

Contém contas compartilhadas como:

- ✅ **Log Archive Account** → Armazena logs de todos os outros accounts
- ✅ **Audit Account** → Permite ao time de segurança consultar qualquer conta via roles

---

### 3️⃣ Custom OU

Onde ficam as contas usadas pelos times para desenvolvimento, produção etc.

---

## 🟨 Contas compartilhadas criadas automaticamente

### 🔹 Log Archive Account

**Armazena logs de:**

- ✅ API calls (CloudTrail)
- ✅ Configurações de recursos (Config)
- ✅ Outros serviços

---

### 🔹 Audit Account

- ✅ Acesso controlado para equipes de segurança
- ✅ Contém roles específicas para leitura/gravação
- ✅ Oferece acesso programático (via Lambda) para auditar contas
- ✅ Não permite login direto nas contas filhas

---

## 👥 Identity & Access

O Control Tower também configura automaticamente:

- ✅ **Single Sign-On (IAM Identity Center)**
- ✅ Atribuição de permissões por grupo
- ✅ Acesso federado às contas
- ✅ Integração opcional com Active Directory corporativo

---

## ⚠️ Atenção ao usar o Control Tower

A criação da Landing Zone gera muitos recursos automaticamente:

- ✅ Roles
- ✅ Buckets
- ✅ Config rules
- ✅ CloudTrail
- ✅ Guardrails
- ✅ SCPs
- ✅ Estrutura do Organizations

**➡️ Deletar ou alterar esses recursos manualmente pode quebrar o Control Tower.**

A AWS recomenda **nunca remover componentes críticos** criados pelo serviço.

---

## 📊 Comparação: Agent-based vs Agentless Discovery

| Aspecto | Agent-based | Agentless (Connector) |
|---------|-------------|----------------------|
| **Ambiente** | Linux, Windows, físicos, VMs | Apenas VMware vSphere |
| **Instalação** | Agente em cada servidor | OVA no vCenter |
| **Frequência de coleta** | A cada 15 minutos | A cada 60 minutos |
| **Dados coletados** | Configuração, performance, rede, processos | Configuração, IP/MAC, disco, CPU/RAM/I/O |
| **Complexidade** | Maior (instalar em cada servidor) | Menor (uma appliance) |
| **Cobertura** | Servidores específicos | Todo ambiente vSphere |

---

## 🎯 Quando Usar Cada Serviço

### AWS Application Discovery Service

**Use quando:**

- ✅ Precisa descobrir servidores e aplicações no ambiente on-premises
- ✅ Quer mapear dependências entre sistemas
- ✅ Precisa coletar métricas de performance e utilização
- ✅ Quer preparar dados para o Migration Hub
- ✅ Precisa definir estratégias de migração (7 Rs) baseadas em dados reais
- ✅ Quer avaliar TCO usando Athena/QuickSight

**Escolha do método:**

- ✅ **Agent-based**: Quando precisa de dados detalhados de servidores específicos
- ✅ **Agentless (Connector)**: Quando tem ambiente VMware e quer descoberta rápida de todo o ambiente

---

### AWS Control Tower

**Use quando:**

- ✅ A organização terá múltiplas contas AWS
- ✅ Precisa estabelecer governança desde o início
- ✅ Quer seguir Well-Architected Framework automaticamente
- ✅ Precisa de estrutura multi-conta padronizada
- ✅ Quer centralizar logs e auditoria
- ✅ Precisa de SSO e controle de acesso federado
- ✅ Está fazendo grandes migrações (dezenas/centenas de workloads)

**Não use quando:**

- ⚠️ Tem apenas uma conta AWS simples
- ⚠️ Não precisa de estrutura multi-conta
- ⚠️ Já tem estrutura de Organizations configurada manualmente (pode conflitar)

---

## 🔄 Fluxo de Trabalho no Estágio Mobilize

```
┌─────────────────────────────────────────────────────────┐
│              ESTÁGIO MOBILIZE                            │
│                                                          │
│  ┌──────────────────────────────────────┐               │
│  │   Application Discovery Service      │               │
│  │   • Instalar agentes ou connector    │               │
│  │   • Coletar dados (15-60 min)        │               │
│  │   • Mapear dependências              │               │
│  │   • Enviar para Migration Hub        │               │
│  └──────────────┬───────────────────────┘               │
│                 │                                        │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐               │
│  │   Análise e Planejamento             │               │
│  │   • Agrupar por aplicação            │               │
│  │   • Definir estratégias (7 Rs)       │               │
│  │   • Identificar lacunas               │               │
│  │   • Criar MRP (Migration Readiness)  │               │
│  └──────────────┬───────────────────────┘               │
│                 │                                        │
│                 ▼                                        │
│  ┌──────────────────────────────────────┐               │
│  │   AWS Control Tower                   │               │
│  │   • Criar Landing Zone                │               │
│  │   • Configurar OUs                    │               │
│  │   • Estabelecer governança            │               │
│  │   • Configurar SSO                    │               │
│  └───────────────────────────────────────┘              │
│                                                          │
│  Pronto para Migrate & Modernize                        │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Recursos Adicionais

- [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/)
- [AWS Control Tower](https://aws.amazon.com/controltower/)
- [Application Discovery Agent](https://docs.aws.amazon.com/application-discovery/latest/userguide/discovery-agents.html)
- [Discovery Connector for VMware](https://docs.aws.amazon.com/application-discovery/latest/userguide/discovery-connector.html)
- [AWS Control Tower Landing Zone](https://docs.aws.amazon.com/controltower/latest/userguide/landing-zone.html)
- [AWS Organizations](https://aws.amazon.com/organizations/)
- [IAM Identity Center](https://aws.amazon.com/iam/identity-center/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o papel do estágio Mobilize na migração
- [ ] Conhecer AWS Application Discovery Service e suas funcionalidades
- [ ] Entender diferença entre descoberta agent-based e agentless
- [ ] Saber quando usar cada método de descoberta
- [ ] Compreender como os dados são coletados e enviados ao Migration Hub
- [ ] Entender AWS Control Tower e Landing Zone
- [ ] Conhecer as três OUs criadas pelo Control Tower
- [ ] Entender contas compartilhadas (Log Archive, Audit)
- [ ] Compreender Identity & Access configurado pelo Control Tower
- [ ] Saber quando usar Control Tower vs não usar
- [ ] Entender precauções ao usar Control Tower

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#mobilize` `#application-discovery` `#control-tower` `#landing-zone` `#governance` `#multi-account`

---

## 🎯 Conclusão: quando usar cada serviço?

| Serviço | Para que serve | Quando usar |
|---------|----------------|-------------|
| **Application Discovery Service** | Descobrir servidores, dependências, métricas, inventário | Para entender workloads, definir estratégias (7Rs) e preparar o Migration Hub |
| **Control Tower** | Criar estrutura multi-conta com governança | Quando a organização terá várias contas AWS, principalmente em grandes migrações |

---

**Última atualização**: 📅 [DD/MM/YYYY]

