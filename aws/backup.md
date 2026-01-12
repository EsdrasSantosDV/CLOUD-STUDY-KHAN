# 💾 AWS Backup — Visão de Arquiteto

> Serviço gerenciado de proteção de dados centralizado para recursos AWS e workloads híbridos

---

## 📌 Resumo

**AWS Backup** é um "orquestrador de proteção de dados" para seus recursos AWS e workloads híbridos. Em vez de scripts soltos de snapshot e backup, você cria políticas centrais e o serviço faz o trabalho sujo para você.

**Objetivo**: Centralizar agendamento, retenção, lifecycle, restore e cópias cross-region/cross-account, mantendo governança e compliance (PCI, ISO, HIPAA eligible).

**Analogia**: É como sair de "cada time faz seu script de backup" para um "policy engine corporativo de backup" com visibilidade única.

---

## 🎯 O que é o AWS Backup

Serviço gerenciado de proteção de dados para:

- ✅ **EC2, EBS, RDS, EFS, DynamoDB, Aurora**, etc.
- ✅ **Workloads on-premises** via AWS Storage Gateway (volumes)
- ✅ **Workloads VMware** on-prem ou VMware Cloud on AWS

### Características Importantes

**Policy-based:**

- ✅ Você define backup plans e associa recursos por tags (ex: `environment=production`)

**Criptografia:**

- ✅ Dados criptografados em trânsito e em repouso
- ✅ Se o recurso já usa KMS (EBS, RDS etc.), o AWS Backup **não recriptografa** por cima – mantém a chave atual

**Compliance:**

- ✅ PCI, ISO, HIPAA eligible
- ✅ Logs consolidados de atividade

**Criação/gestão:**

- ✅ Console, CLI ou API

---

## 🔄 Integração com Ambientes Híbridos

**Integra com AWS Storage Gateway (Volume Gateway):**

- ✅ Backups de volumes do Storage Gateway são armazenados em AWS
- ✅ **Compatíveis com EBS**: Você pode restaurar:
  - Para AWS (como volume EBS)
  - Para on-prem (via Storage Gateway)

**Permite mesma política de backup para:**

- ✅ Recursos nativos AWS
- ✅ Dados on-prem que estão em volumes do Storage Gateway

**Isso é o ponto chave de híbrido**: mesma política, em um único lugar.

---

## 🧩 Conceitos Fundamentais do AWS Backup

### 1. Backup Rule (Regra de Backup)

Define **"como tratar os backups"**:

- ✅ **Quando**: schedule (diário, horário, etc.)
- ✅ **Onde**: qual Backup Vault
- ✅ **Quanto tempo**: retention
- ✅ **Lifecycle**: quando mandar para cold storage
- ✅ **Cópia**: cross-region / cross-account copies

**Pense como uma "cláusula" de política**: frequência, retenção, destino.

---

### 2. Backup Plan (Plano)

Um plano é um **conjunto de uma ou mais regras**.

**Você associa recursos a esse plano:**

- ✅ Diretamente (lista de ARNs)
- ✅ Ou por **tags** (padrão recomendado)

**Na prática**: "Plano de backup de produção" com:

- ✅ Regra diária de 120 dias
- ✅ Talvez outra regra mensal de longo prazo

---

### 3. Resource (Recurso)

**Qualquer coisa que você quer proteger:**

- ✅ EC2, RDS, EFS, DynamoDB, volumes do Storage Gateway, etc.

**Normalmente selecionados via tags** (ex.: `environment=production`).

---

### 4. Vault (Cofre)

**Backup Vault** = cofre gerenciado e criptografado onde os backups ficam.

- ✅ Cada plano manda os backups para um vault
- ✅ Na maioria dos casos o **Default** já é suficiente

---

### 5. Vault Lock

Aplica política **WORM (Write Once Read Many)** ao vault.

**Objetivo:**

- ✅ Impedir que alguém reescreva ou apague backups (mitigar ransomware)
- ✅ Atender requisitos de compliance (retenção imutável)

**Isso é extremamente importante para ambientes regulados.**

**Importante**: Uma vez aplicado corretamente, você não "desfaz" facilmente (é o objetivo).

---

### 6. Job (Trabalho)

**Unidade de execução:**

- ✅ **Backup Job** – execução de backup
- ✅ **Restore Job** – execução de restore
- ✅ **Copy Job** – cópia cross-region/cross-account

**Usado para monitorar status** (sucesso, falha, progresso).

---

## 🔄 Fluxo Prático — Criando um Backup Plan

### 1. Criar Plano

**Ir ao console do AWS Backup**

**Criar Backup Plan:**

- ✅ Pode usar **template pronto** (ex.: diário com 35 dias)
- ✅ Ou **Build from scratch**

**Exemplo:**

- ✅ Nome: `ProductionBackupPlan`
- ✅ Regra: `daily-120-days-retention`
- ✅ Vault: Default (na maioria dos casos basta)

---

### 2. Configurar Regra

**Frequência:**

- ✅ **Daily** (para web server, blog, etc.)
- ✅ **Hourly** ou mais agressivo para dados extremamente transacionais

**Janela de backup (backup window):**

- ✅ Escolher horário de menor carga (madrugada)

**Lifecycle:**

- ✅ Ex.: mover para cold storage após 15 dias
- ✅ Deletar após 120 dias (fim da retenção)

---

### 3. Sem Recursos Ainda

**Ao criar o plano, inicialmente:**

- ⚠️ Nenhum recurso está associado
- ✅ Isso é esperado — você vai associar depois (melhor com tags)

---

## 🏷️ Associações de Recursos (por Tags)

### Strategy Correta: Tag-based

**Criar um Resource Assignment:**

- ✅ Nome: `ProductionResources`
- ✅ Critério: Tag `environment=production`
- ✅ Qualquer recurso compatível com AWS Backup e com essa tag → entra no plano

---

### Exemplo em RDS

**Ir no RDS → Databases → selecionar DB**

**Em Tags:**

- ✅ `environment=production`

**Resultado**: esse DB será incluído no plano de produção.

---

### Exemplo em EC2

**Ir em EC2 → Instances → selecionar wordpress-blog**

**Em Tags:**

- ✅ `environment=production`

**Resultado**: essa instância (e o que o serviço suportar como recurso) será protegida pelo plano.

**Essa abordagem é escalável**: você não depende de lembrar de "adicionar manualmente" nas listas do plano. Basta padronizar tagging.

**Analogia**: Você define uma política corporativa ("tudo que é produção é backup diário por 120 dias, indo depois para cold storage") e só garante que os servidores estejam rotulados como "produção". O resto é automático.

---

## ⚙️ Janela, Frequência, Cold Storage e Retenção

### Frequência

- ✅ **Dados altamente transacionais** → talvez horário
- ✅ **Web / blog / sistemas com menos alteração** → diário costuma ser suficiente

### Janela de Backup

- ✅ Faixa de horário em que o job pode ser executado
- ✅ Normalmente em períodos de menor carga

### Cold Storage

- ✅ Camada mais barata para backups antigos
- ✅ Ex.: após 15 dias, mover para cold storage para reduzir custo

### Retenção

- ✅ Ex.: retention = 120 dias → após isso, o backup é deletado

---

## 🚀 On-Demand Backup

Além do cron automático, você pode disparar backups **"na hora"**:

**Ir ao Dashboard do AWS Backup**

**Create On-demand Backup**

**Escolher tipo de recurso:**

- ✅ Ex.: EC2

**Escolher o recurso** (My WordPress blog)

**Definir:**

- ✅ Retention (ex.: 30 dias)
- ✅ Vault (normalmente Default)

**Criar o backup**

> ⚠️ **Obs.**: "Now" significa: o job inicia até em 1 hora, não é instantâneo sempre.

**Uso típico**: Antes de uma mudança grande em produção (deploy arriscado, alteração de schema), você faz um backup on-demand como "save point".

---

## 📊 Monitoramento: Backup / Restore / Copy Jobs

Na aba **Jobs** você acompanha:

- ✅ **Backup jobs** – execuções de backup (de planos e on-demand)
- ✅ **Restore jobs** – execuções de restore
- ✅ **Copy jobs** – cópias cross-region / cross-account

**Isso vira o "log operacional" da sua estratégia de proteção.**

---

## 🌍 Cross-Region / Cross-Account — Disaster Recovery

Aqui está uma das partes mais fortes do serviço.

### Copy to Destination

**Dentro da Backup Rule:**

- ✅ Editar a regra → opção **"Copy to destination"**
- ✅ Você pode:
  - Escolher uma outra região (ex.: `ca-central-1`)
  - Escolher outro vault destino (na outra região)
  - Adicionar múltiplas cópias:
    - Ex.: uma cópia em Canadá, outra na Europa

---

### Cross-Account

**Com permissões de IAM corretas:**

- ✅ Você pode copiar backups para um vault em outro AWS Account

**Estratégia clássica de "backup isolado":**

- ✅ Produção em uma conta
- ✅ Cofre de DR em outra conta, com acesso bem mais restrito

---

### Desenho Típico de DR

**Conta A, Região X** → workload de produção

**AWS Backup:**

- ✅ Backup local (mesma região)
- ✅ Copy Job para:
  - Região Y (ex.: Europa)
  - Conta B (conta de DR)

**Em caso de desastre:**

- ✅ Você restaura os recursos a partir dos backups da outra região/conta

**Na minha opinião, esse modelo (cross-region + cross-account) é o mínimo aceitável para qualquer ambiente realmente crítico em produção.**

**Cenários típicos:**

- ✅ **DR regional**: Região primária `us-east-1` → Cópia automática para `ca-central-1`
- ✅ **DR continental / compliance**: Cópia adicional para uma região na Europa
- ✅ **Separação de responsabilidades**: Produção na Conta A → Backups de DR replicados na Conta B (restrita, só para recuperação em desastre)

**É aqui que o AWS Backup realmente se paga**: você padroniza backup + cópia cross-region + cópia cross-account usando configuração declarativa, em vez de scripts Frankenstein.

---

## ⚠️ Detalhes Operacionais Importantes

### Vault Lock

- ✅ Use em ambientes que precisam de proteção contra alteração (ransomware, fraudes internas)
- ✅ Uma vez aplicado corretamente, você não "desfaz" facilmente (é o objetivo)

### KMS / Criptografia

- ✅ AWS Backup **não adiciona outra camada de criptografia**
- ✅ Se EBS/RDS já estão com KMS, o backup usa essas configurações

### Complexidade de Planos

- ✅ Você pode adicionar múltiplas regras no mesmo plano:
  - Ex.: diário 30 dias + semanal 1 ano + mensal 7 anos, cada uma com política de cold storage diferente

### Tagging é Crítico

- ⚠️ Sem tagging consistente (`environment`, `app`, `owner`), fica fácil esquecer recurso sem backup
- ✅ Em ambiente corporativo, eu usaria uma **política obrigatória de tags** para que o recurso possa entrar em produção

---

## 🎯 Quando AWS Backup Faz Mais Sentido

Em vez de pensar "qual serviço suporta snapshot nativo", pense:

**Você precisa de:**

- ✅ Padrões corporativos de retenção e agendamento?
- ✅ Auditoria centralizada?
- ✅ Mesma política para múltiplos serviços (EC2, RDS, EFS, Dynamo, Storage Gateway)?
- ✅ DR cross-region/cross-account?
- ✅ Imutabilidade (Vault Lock / WORM)?

**Se a resposta for sim para boa parte disso**, AWS Backup vira o "control plane de proteção de dados" e seus scripts de snapshot viram apenas exceções específicas.

---

## 📊 Resumo Rápido

| Conceito | Descrição |
|----------|-----------|
| **Backup Rule** | Define quando, onde, retenção e lifecycle |
| **Backup Plan** | Conjunto de regras aplicadas a recursos |
| **Resource** | O que será protegido (EC2, RDS, EFS, etc.) |
| **Vault** | Cofre criptografado onde backups ficam |
| **Vault Lock** | WORM - proteção imutável para compliance |
| **Job** | Execução de backup/restore/copy |

---

## 🔗 Recursos Adicionais

- [AWS Backup - Página do Produto](https://aws.amazon.com/backup/)
- [AWS Backup - Documentação](https://docs.aws.amazon.com/aws-backup/)
- [AWS Backup - Guia do Usuário](https://docs.aws.amazon.com/aws-backup/latest/devguide/)
- [AWS Backup - Recursos Suportados](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html#supported-resources)
- [AWS Backup - Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito de AWS Backup como orquestrador centralizado
- [ ] Conhecer recursos suportados (EC2, RDS, EFS, DynamoDB, Storage Gateway, etc.)
- [ ] Entender conceitos fundamentais (Backup Rule, Plan, Resource, Vault, Vault Lock, Job)
- [ ] Compreender estratégia tag-based para associação de recursos
- [ ] Entender configuração de frequência, janela de backup e retenção
- [ ] Conhecer cold storage e lifecycle policies
- [ ] Entender on-demand backups e quando usar
- [ ] Compreender integração com Storage Gateway (workloads híbridos)
- [ ] Entender cross-region e cross-account copies para DR
- [ ] Conhecer Vault Lock e proteção WORM
- [ ] Entender modelo de criptografia (mantém KMS existente)
- [ ] Saber quando usar AWS Backup vs snapshots nativos
- [ ] Compreender importância de tagging consistente

---

## 🏷️ Tags

`#aws` `#backup` `#disaster-recovery` `#data-protection` `#compliance` `#vault-lock` `#cross-region` `#cross-account` `#storage-gateway` `#hybrid-cloud` `#governance`

---

## 🎯 Conclusão

**AWS Backup** é o "control plane de proteção de dados" da AWS, permitindo:

- ✅ Centralizar políticas de backup em vez de scripts espalhados
- ✅ Aplicar mesma política para recursos AWS e workloads híbridos
- ✅ Automatizar DR com cópias cross-region e cross-account
- ✅ Atender compliance com Vault Lock e logs consolidados
- ✅ Escalar facilmente com estratégia tag-based

**Para ambientes críticos em produção**, o modelo mínimo aceitável é backup + cópia cross-region + cópia cross-account, tudo configurado de forma declarativa através do AWS Backup.

---

**Última atualização**: 📅 [DD/MM/YYYY]


