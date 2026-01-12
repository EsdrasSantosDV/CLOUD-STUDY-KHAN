# 📈 EC2 Auto Scaling

> Mecanismo de elasticidade automática para instâncias EC2

---

## 📌 Resumo

**EC2 Auto Scaling** é o mecanismo da AWS responsável por aumentar ou reduzir automaticamente a quantidade de instâncias EC2 de acordo com a demanda do sistema, baseada em métricas e limites previamente definidos.

**Analogia de engenharia:**
Auto Scaling é como um sistema hidráulico inteligente: quando a pressão aumenta, mais válvulas se abrem; quando a pressão cai, elas se fecham automaticamente.

O objetivo central não é apenas escalar — é manter o equilíbrio entre desempenho, disponibilidade e custo.

---

## 🎯 O que é Auto Scaling

Auto Scaling permite:

- ✅ **Scale out** → adicionar instâncias quando a demanda aumenta
- ✅ **Scale in** → remover instâncias quando a demanda diminui

**Tudo isso ocorre:**
- ✅ Automaticamente
- ✅ Baseado em métricas (ex: CPU, memória, requisições)
- ✅ Sem intervenção humana direta

---

## 🔹 EC2 Auto Scaling vs Auto Scaling Service

**Na AWS existem dois contextos distintos:**

- **EC2 Auto Scaling** → gerencia frotas de instâncias EC2
- **Auto Scaling Service** → escala outros serviços (ECS, DynamoDB, Aurora)

**Neste contexto, o foco é exclusivamente EC2 Auto Scaling.**

---

## 🔹 Exemplo Prático de Funcionamento

### Cenário Inicial

- 1 EC2 atuando como web server
- Tráfego vindo da Internet
- CPU começa a subir conforme o uso aumenta

### Configuração Típica

**Scale out:**
- CPU ≥ 75% → lançar nova instância

**Scale in:**
- CPU ≤ 20% → encerrar instância excedente

### Resultado

- ✅ Carga distribuída
- ✅ Menor risco de degradação
- ✅ Melhor experiência do usuário

> 💡 **Opinião técnica:** CPU é uma métrica didática, mas nem sempre a melhor métrica em produção. Ainda assim, ela explica perfeitamente o conceito.

---

## 🔹 Auto Scaling + Load Balancer

**Quando o Auto Scaling é integrado a um Elastic Load Balancer:**

- ✅ Novas instâncias entram automaticamente no balanceamento
- ✅ Instâncias removidas deixam de receber tráfego
- ✅ O sistema se comporta como um organismo elástico

**Analogia clara:**
Load Balancer distribui o tráfego. Auto Scaling garante que existam "pulmões" suficientes para respirar.

**Separados, já são úteis. Juntos, são arquiteturalmente poderosos.**

---

## 🔹 Benefícios do EC2 Auto Scaling

### Automação

- ✅ Elimina ações manuais
- ✅ Reduz erro humano
- ✅ Padroniza resposta a picos de carga

### Experiência do Usuário

- ✅ Capacidade cresce antes da falha
- ✅ Menor latência sob carga
- ✅ Maior confiabilidade percebida

### Otimização de Custos

- ✅ Você paga apenas por instâncias em execução
- ✅ Escala para baixo quando não há demanda
- ✅ Modelo ideal para workloads variáveis

> 💡 **Na prática profissional:** Auto Scaling não é opcional — é controle financeiro automatizado.

---

## 🧩 Componentes do EC2 Auto Scaling

EC2 Auto Scaling é composto, essencialmente, por dois blocos de construção:

1. **Launch Configuration ou Launch Template**
2. **Auto Scaling Group (ASG)**

**Analogia de engenharia:**
Launch Template/Configuration é a planta da máquina (o "como construir"). Auto Scaling Group é o gerente da fábrica (quantas máquinas, onde ficam e quando ligar/desligar).

Sem a planta, o gerente não sabe o que fabricar. Sem o gerente, a planta não se materializa em capacidade elástica.

---

## 🔹 Launch Configuration / Launch Template

Ambos definem **como o ASG cria novas instâncias EC2** — respondem perguntas fundamentais como:

- ✅ Qual AMI usar
- ✅ Qual instance type
- ✅ Uso de Spot Instances (para reduzir custo)
- ✅ Instância terá public IP?
- ✅ User Data (bootstrapping no primeiro boot)
- ✅ Configuração de storage/volumes (EBS, tamanho, criptografia, delete-on-termination, IOPS)
- ✅ Quais Security Groups
- ✅ Key pair (acesso)
- ✅ Perfil/Role IAM (acesso a serviços AWS)

**Na prática, isso é "criar EC2", só que industrializado.**

---

## 🔹 Launch Template vs Launch Configuration

### Launch Template (Preferido) ✅

**Características:**
- ✅ Mais novo, mais completo
- ✅ Interface mais direta (tudo em uma tela, mais opções)
- ✅ Suporta versionamento (v1, v2, etc.)
- ✅ Mais recursos avançados

**Vantagens:**
- Versionamento permite rollback fácil
- Mais opções de configuração
- Melhor integração com outros serviços AWS
- Suporte a múltiplas instance types (mixed instances)

---

### Launch Configuration (Legado) ⚠️

**Características:**
- ⚠️ Mais antigo
- ⚠️ Menos opções avançadas
- ⚠️ Processo em várias telas
- ⚠️ Sem versionamento

**Status:**
Continua existindo, mas a AWS deixa claro o caminho: templates

> 💡 **Opinião técnica (direta):** Launch Configuration é como manter um script antigo porque "funciona". Você paga com rigidez e limitações. Em ambientes profissionais, Launch Template é a escolha correta.

---

## 🔹 Exemplo de Launch Template

### Conceitos Configurados

**Configurações básicas:**
- **AMI:** Amazon Linux
- **Instance type:** t3.micro (exemplo)
- **Network:** VPC
- **Security Groups:** selecionados via dropdown
- **Storage:** EBS gp3 (8 GiB) + opções de criptografia e delete on termination

**Tags:**
- Aplicadas à instância e volumes

**Advanced:**
- **Spot Instances** (opcional)
- **Instance profile** (role IAM)
- **Shutdown behavior** (stop/terminate)
- **User data:** comandos no boot (opcional)

**Resultado:** Template criado com Default version = 1 e Latest version = 1.

---

## 🔹 Exemplo de Launch Configuration

### Conceitos Configurados

- Escolha de AMI e instance type
- Opção de Spot Instances
- IAM role (aqui aparece como role; no template, via instance profile)
- User data
- Storage
- Security Groups
- Key pair selecionado ao final

**Resultado:** Launch configuration criada, porém com menos opções avançadas e menos flexibilidade.

---

## 🔹 Auto Scaling Group (ASG)

O **Auto Scaling Group** define:

### Capacidade

- ✅ **Desired capacity** - quantas instâncias você quer "agora"
- ✅ **Min capacity** - limite mínimo da frota
- ✅ **Max capacity** - limite máximo da frota

### Localização

- ✅ **Onde escalar:** VPC, subnets e AZs
- ✅ Múltiplas AZs para alta disponibilidade

### Integração

- ✅ **Load Balancer / Target Groups** (opcional na criação)
- ✅ Integração automática com ELB

### Proteções

- ✅ Service-linked role
- ✅ Scale-in protection (proteção contra remoção)

**Analogia:**
Se Launch Template é o "modelo do carro", o ASG é o controle de frota: quantos carros existem, onde circulam e quando comprar/vender carros conforme demanda.

---

## 🔹 Scaling Policies + CloudWatch Alarms

### Exemplo de Configuração

**ASG configurado para escalar entre:**
- **Mínimo:** 2 instâncias
- **Máximo:** 5 instâncias
- **Desired inicial:** 2 instâncias

**Políticas de escala do tipo simple/step scaling com alarmes:**

---

### Scale Out (Aumentar Grupo)

**Ação:** +1 instância

**Alarme:** CPU média ≥ 75% por 1 período de 5 minutos

**Comportamento:**
- Quando CPU atinge 75% por 5 minutos consecutivos
- ASG adiciona 1 instância automaticamente
- Nova instância entra no target group do ELB

---

### Scale In (Reduzir Grupo)

**Ação:** -1 instância

**Alarme:** CPU média ≤ 30% por 2 períodos de 5 minutos

**Comportamento:**
- Quando CPU fica abaixo de 30% por 10 minutos consecutivos (2 períodos)
- ASG remove 1 instância automaticamente
- Instância é removida do target group antes de terminar

> 💡 **Opinião técnica:** O ponto forte aqui é o conceito de **histerese** (thresholds diferentes para subir e descer + períodos diferentes). Isso evita "serrote" (scale out/in repetindo em loop).

---

## 🔹 Tipos de Scaling Policies

### Simple Scaling

**Características:**
- ✅ Ação simples baseada em um alarme
- ✅ Adiciona ou remove capacidade fixa
- ✅ Cooldown period após ação

**Exemplo:**
- CPU > 75% → adicionar 2 instâncias
- CPU < 25% → remover 1 instância

---

### Step Scaling

**Características:**
- ✅ Múltiplas ações baseadas em níveis de métrica
- ✅ Mais granular e flexível
- ✅ Diferentes ações para diferentes níveis

**Exemplo:**
- CPU 75-85% → adicionar 1 instância
- CPU 85-95% → adicionar 2 instâncias
- CPU > 95% → adicionar 3 instâncias

---

### Target Tracking Scaling

**Características:**
- ✅ Mantém métrica em um valor alvo
- ✅ Mais simples de configurar
- ✅ AWS calcula automaticamente quantas instâncias são necessárias

**Exemplo:**
- Manter CPU médio em 50%
- AWS ajusta automaticamente o número de instâncias

---

### Scheduled Scaling

**Características:**
- ✅ Escala baseado em horário/cronograma
- ✅ Útil para padrões previsíveis de tráfego
- ✅ Pode ser combinado com outras políticas

**Exemplo:**
- 9h → aumentar para 5 instâncias (início do expediente)
- 18h → reduzir para 2 instâncias (fim do expediente)

---

## 🔹 Notificações (SNS)

**Foram configuradas notificações via SNS topic para eventos do ASG, como:**

- ✅ **Instância lançada** - quando nova instância é criada
- ✅ **Instância terminada** - quando instância é removida
- ✅ **Falha ao lançar** - quando há erro ao criar instância
- ✅ **Falha ao terminar** - quando há erro ao remover instância
- ✅ **Teste de health check falhou** - quando instância não passa no health check

**Isso cria um canal de auditoria operacional:** você sabe quando a frota está mudando e por quê.

---

## 🔹 Conceitos-Chave de Escala

**Terminologia importante:**

- ✅ **Scale out:** adicionar capacidade horizontal
- ✅ **Scale in:** remover capacidade ociosa
- ✅ **Threshold:** limite que dispara ação
- ✅ **Metric:** sinal que representa carga real
- ✅ **Histerese:** diferença entre thresholds de scale out e scale in (evita flapping)
- ✅ **Cooldown:** período de espera após ação de scaling

**Esses elementos formam o sistema nervoso da infraestrutura.**

---

## 🎯 Casos de Uso

### Web Apps com Tráfego Variável

**Cenário:**
- Horários de pico (manhã, almoço, fim do dia)
- Tráfego baixo durante madrugada

**Solução:**
- Auto Scaling reage automaticamente
- Scheduled scaling para padrões previsíveis
- Dynamic scaling para picos inesperados

---

### Ambientes que Precisam Balancear Custo vs Performance

**Cenário:**
- Desenvolvimento e staging
- Não precisa estar sempre no máximo

**Solução:**
- Min capacity baixo (1-2 instâncias)
- Scale out apenas quando necessário
- Reduz custos operacionais

---

### Backends com Processamento Elástico

**Cenário:**
- Jobs em fila (SQS)
- Workers processando tarefas
- Processamento em lote

**Solução:**
- Scale baseado em tamanho da fila
- Adiciona workers quando fila cresce
- Remove workers quando fila está vazia

---

### Alta Disponibilidade via Múltiplas Subnets/AZs

**Cenário:**
- Aplicação crítica
- Não pode ter downtime

**Solução:**
- ASG em múltiplas AZs
- Min capacity garante sempre instâncias rodando
- Distribuição automática entre AZs

---

## ❓ Perguntas Frequentes

**Auto Scaling substitui planejamento?**
**Resposta:** ❌ Não, ele executa o planejamento. Você ainda precisa definir min/max, métricas e thresholds.

**Auto Scaling evita falhas totalmente?**
**Resposta:** ⚠️ Reduz drasticamente, não elimina. Falhas podem ocorrer, mas Auto Scaling ajuda a recuperar rapidamente.

**Quando escolher Launch Configuration?**
**Resposta:** ⚠️ Apenas por compatibilidade com legados. Em projetos novos, Launch Template.

**Por que usar períodos diferentes para scale in/out?**
**Resposta:** Para evitar flapping e tornar o sistema estável. Histerese previne oscilações constantes.

**Posso usar Auto Scaling sem Load Balancer?**
**Resposta:** ✅ Sim, mas não é recomendado para aplicações web. ELB distribui tráfego entre instâncias do ASG.

**Qual métrica usar para scaling?**
**Resposta:** Depende do workload. CPU é comum, mas considere: requisições por segundo, latência, tamanho de fila, memória, etc.

---

## ⚠️ Erros Comuns

**❌ Definir thresholds muito agressivos**
**Problema:** Sistema escala/desescala constantemente (flapping)
**Solução:** Usar histerese adequada e períodos de cooldown

**❌ Usar métricas que não representam carga real**
**Problema:** Scaling baseado em métrica incorreta
**Solução:** Escolher métricas que realmente representam demanda (ex: requisições, não apenas CPU)

**❌ Não definir scale in (ambiente cresce e nunca encolhe)**
**Problema:** Custos desnecessários
**Solução:** Sempre configurar políticas de scale in

**❌ Usar o mesmo threshold para subir e descer**
**Problema:** Causa instabilidade e flapping
**Solução:** Usar thresholds diferentes (ex: 75% para scale out, 30% para scale in)

**❌ Não configurar min/max adequadamente**
**Problema:** Ou cresce demais ou não cresce nunca
**Solução:** Definir limites baseados em análise de carga real

**❌ Criar ASG em uma subnet/AZ só**
**Problema:** Reduz resiliência e alta disponibilidade
**Solução:** Sempre usar múltiplas AZs

**❌ Não integrar com ELB/Target Groups quando o workload é web**
**Problema:** Tráfego não é distribuído entre instâncias
**Solução:** Sempre integrar ASG com ELB para aplicações web

---

## 🔗 Integração ELB + Auto Scaling

### 📌 Introdução

**Elastic Load Balancers e EC2 Auto Scaling foram projetados para trabalhar juntos.**

Separados, cada um resolve apenas metade do problema.

Juntos, entregam performance, elasticidade e eficiência de custo.

**Analogia de engenharia:**
O ELB é o sistema circulatório que distribui fluxo. O Auto Scaling é o sistema respiratório, que cria ou remove capacidade. Um sem o outro gera esforço manual e risco operacional.

---

### 🔹 Papéis Claros de Cada Serviço

#### Elastic Load Balancer (ELB)

**Responsável por:**
- ✅ Distribuir tráfego
- ✅ Encaminhar requisições para targets
- ✅ Detectar instâncias saudáveis (health checks)
- ✅ Aplicar regras e políticas de roteamento

**Sozinho, ele não cria nem remove capacidade.**

---

#### EC2 Auto Scaling

**Responsável por:**
- ✅ Criar e terminar instâncias EC2
- ✅ Manter capacidade mínima, desejada e máxima
- ✅ Reagir a métricas e alarmes
- ✅ Otimizar custo automaticamente

**Sozinho, ele não distribui tráfego.**

> 💡 **Opinião técnica direta:** Usar apenas um dos dois é aceitar trabalho manual perpétuo.

---

### ❌ Problemas de Usar Apenas Um Serviço

#### ELB sem Auto Scaling

**Você precisa:**
- ❌ Monitorar carga manualmente
- ❌ Criar/terminar instâncias manualmente
- ❌ Registrar/desregistrar targets manualmente
- ❌ Alto risco de erro humano
- ❌ Escalabilidade lenta e reativa

---

#### Auto Scaling sem ELB

**Instâncias sobem e descem corretamente, mas:**
- ❌ O tráfego não é distribuído
- ❌ Pode sobrecarregar instâncias individuais
- ❌ Falta de balanceamento = desperdício de elasticidade
- ❌ Sem alta disponibilidade real

---

### ✅ Benefício da Integração ELB + Auto Scaling

**Quando integrados:**

- ✅ Novas instâncias são automaticamente registradas
- ✅ Instâncias removidas são automaticamente desregistradas
- ✅ Tráfego flui apenas para recursos saudáveis
- ✅ A infraestrutura se ajusta sozinha à demanda

**Resultado:**
- ✅ Menor custo
- ✅ Melhor performance
- ✅ Alta disponibilidade real
- ✅ Zero intervenção manual

---

### 🔹 Como Funciona a Associação

#### ALB ou NLB

**A associação ocorre via Target Groups:**

1. O Auto Scaling Group é vinculado ao target group
2. O ELB encaminha tráfego para os targets ativos
3. Novas instâncias são automaticamente registradas no target group
4. Instâncias removidas são automaticamente desregistradas

**Fluxo:**
```
Client → ELB → Target Group → ASG Instances
```

**Vantagens:**
- ✅ Desacoplamento entre ELB e ASG
- ✅ Flexibilidade arquitetural
- ✅ Suporte a múltiplos target groups (ALB)

---

#### Classic Load Balancer (CLB)

**Associação direta:**

- O ASG registra as instâncias diretamente no CLB
- Não há target groups
- Acoplamento mais rígido

> ⚠️ **Nota arquitetural:** Esse acoplamento direto é mais um motivo pelo qual o CLB é legado.

---

### 🔹 Demonstração – Associação na Prática

#### Passos Resumidos

1. **EC2 → Auto Scaling Groups**

2. **Selecionar o ASG existente**

3. **Actions → Edit**

4. **Em Classic Load Balancers and Target Groups:**
   - Selecionar CLB (para Classic Load Balancer) ou
   - Selecionar Target Group (para ALB/NLB)

5. **Salvar**

**Resultado imediato:**
- ✅ O ASG passa a gerenciar o registro de instâncias no ELB automaticamente
- ✅ Novas instâncias entram no target group após health check
- ✅ Instâncias removidas saem do target group antes de terminar

---

### 🔹 Fluxo Completo de Integração

#### Quando uma Nova Instância é Criada

1. **ASG cria instância** baseado em Launch Template
2. **Instância inicia** e executa User Data (se configurado)
3. **Instância é registrada** automaticamente no Target Group
4. **ELB inicia health check** na instância
5. **Após passar no health check**, instância recebe tráfego
6. **Tráfego é distribuído** pelo ELB para a nova instância

---

#### Quando uma Instância é Removida

1. **ASG decide remover instância** (scale in)
2. **Instância é desregistrada** do Target Group
3. **ELB para de enviar tráfego** para a instância
4. **Draining period** (opcional) - aguarda conexões existentes terminarem
5. **Instância é terminada** pelo ASG
6. **Tráfego continua** sendo distribuído para instâncias restantes

---

### 🔹 Health Checks na Integração

#### Health Checks do ELB

**O ELB verifica:**
- ✅ Se a instância responde na porta configurada
- ✅ Se o path de health check retorna código HTTP válido (ALB)
- ✅ Se a conexão TCP é estabelecida (NLB/CLB)

**Se health check falha:**
- ❌ ELB para de enviar tráfego
- ⚠️ Instância pode ser marcada como unhealthy no ASG

---

#### Health Checks do ASG

**O ASG verifica:**
- ✅ Se a instância está rodando
- ✅ Status do health check do ELB (se integrado)
- ✅ Status do EC2 health check

**Se health check falha:**
- ❌ ASG pode terminar e substituir a instância
- ✅ Nova instância é criada automaticamente

---

### ❓ Perguntas Frequentes sobre Integração

**Posso associar o ELB depois de criar o ASG?**
**Resposta:** ✅ Sim, a associação é dinâmica e simples. Basta editar o ASG e adicionar o target group ou CLB.

**Instâncias novas entram automaticamente no balanceamento?**
**Resposta:** ✅ Sim, após passarem no health check do ELB. O processo é automático.

**Posso associar um ASG a múltiplos target groups?**
**Resposta:** ✅ Sim, um ASG pode ser associado a múltiplos target groups (especialmente útil com ALB).

**O que acontece se o health check do ELB falhar?**
**Resposta:** O ELB para de enviar tráfego. Se configurado, o ASG pode terminar e substituir a instância.

**Posso usar ASG sem ELB?**
**Resposta:** ✅ Tecnicamente sim, mas não é recomendado para aplicações web. Sem ELB, não há distribuição de tráfego.

---

### ⚠️ Erros Comuns na Integração

**❌ Criar ASG sem ELB para workloads web**
**Problema:** Tráfego não é distribuído, desperdício de elasticidade
**Solução:** Sempre integrar ASG com ELB para aplicações web

**❌ Associar ASG ao ELB errado ou ao target group errado**
**Problema:** Tráfego vai para lugar errado ou não chega
**Solução:** Validar target group/ELB antes de associar

**❌ Health checks inconsistentes entre ASG e ELB**
**Problema:** Instâncias podem ser removidas incorretamente
**Solução:** Alinhar configurações de health check entre ASG e ELB

**❌ Usar CLB por hábito e não por necessidade real**
**Problema:** Perda de flexibilidade e features modernas
**Solução:** Preferir ALB/NLB com target groups

**❌ Não configurar draining period**
**Problema:** Conexões ativas podem ser interrompidas abruptamente
**Solução:** Configurar draining period adequado para sua aplicação

---

## 🔗 Integrações

### Elastic Load Balancer (ELB)

- ✅ ASG integra automaticamente com ELB
- ✅ Novas instâncias entram automaticamente no target group
- ✅ Instâncias removidas saem do target group
- ✅ Health checks do ELB influenciam decisões do ASG
- ✅ Ver seção [Integração ELB + Auto Scaling](#-integração-elb--auto-scaling) acima para detalhes completos

### CloudWatch

- ✅ Métricas automáticas do ASG
- ✅ Alarmes para disparar scaling policies
- ✅ Logs e monitoramento

### SNS (Simple Notification Service)

- ✅ Notificações de eventos do ASG
- ✅ Alertas quando instâncias são criadas/removidas
- ✅ Integração com email, SMS, etc.

### IAM

- ✅ Service-linked role para ASG
- ✅ Instance profile para instâncias criadas
- ✅ Permissões para acessar outros serviços AWS

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [EC2 Auto Scaling Overview](https://docs.aws.amazon.com/autoscaling/)
- [EC2 Auto Scaling: Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html)
- [EC2 Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html)
- [CloudWatch Alarms for Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/cloudwatch-alarms.html)

### Artigos Recomendados
- Designing Scalable Architectures on AWS
- Best Practices for Auto Scaling
- Choosing the Right Scaling Policy

---

## ✅ Checklist de Prática

### Fundamentos
- [x] Entendi o conceito de Auto Scaling
- [x] Sei o que é scale out e scale in
- [x] Compreendi o papel das métricas
- [x] Entendi a diferença entre Launch Template e Launch Configuration

### Configuração
- [ ] Criar Launch Template com AMI, tipo, SG, storage e tags
- [ ] Criar ASG com desired/min/max e subnets
- [ ] Configurar scale out e scale in via alarmes de CPU
- [ ] Configurar notificações via SNS
- [ ] Integrar ASG com ELB/Target Groups

### Testes e Validação
- [ ] Testar scale out/in gerando carga real
- [ ] Validar histerese (thresholds diferentes)
- [ ] Monitorar métricas no CloudWatch
- [ ] Verificar notificações SNS
- [ ] Testar failover entre AZs

---

## 📊 Resumo

**EC2 Auto Scaling** é o mecanismo central de elasticidade da AWS, permitindo que a infraestrutura cresça e encolha automaticamente conforme a demanda, equilibrando desempenho, disponibilidade e custo.

**Componentes principais:**
- ✅ **Launch Template/Configuration** - Define como criar instâncias
- ✅ **Auto Scaling Group** - Gerencia capacidade e localização
- ✅ **Scaling Policies** - Define quando e como escalar
- ✅ **CloudWatch Alarms** - Dispara ações de scaling
- ✅ **SNS Notifications** - Fornece visibilidade operacional

**Principais benefícios:**
- ✅ Automação completa
- ✅ Otimização de custos
- ✅ Alta disponibilidade
- ✅ Melhor experiência do usuário

**Próximos Passos:**
- Ver seção [Integração ELB + Auto Scaling](#-integração-elb--auto-scaling) para detalhes completos
- Integrar com [Elastic Load Balancer](./elb.md)
- Explorar tipos avançados de scaling policies
- Implementar scheduled scaling para padrões previsíveis
- Otimizar métricas e thresholds baseado em dados reais

---

**Última atualização:** 📅 09/01/2026

