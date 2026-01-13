# ⚠️ Classic Load Balancer (CLB)

> Load balancer legado mantido apenas para compatibilidade

---

## 📌 Resumo

O **Classic Load Balancer (CLB)** é o load balancer mais antigo da AWS.

Ele foi projetado em um período anterior à popularização de:
- VPC como padrão
- Microserviços
- Containers
- Roteamento inteligente de camada 7

**Analogia direta:**
O CLB é como uma ponte antiga: ainda segura alguns veículos, mas não foi projetada para o tráfego moderno nem para os padrões atuais de segurança e escalabilidade.

Hoje, ele existe principalmente por compatibilidade histórica.

---

## 🔹 Contexto Histórico – EC2-Classic

O CLB foi criado para funcionar com o **EC2-Classic**, um modelo de rede:

- ❌ Sem VPC
- ❌ Rede plana
- ❌ Compartilhada entre clientes AWS

**Importante:**
- ⚠️ Contas criadas após **12 de abril de 2013** não suportam EC2-Classic
- ⚠️ EC2-Classic está **descontinuado** para novos ambientes

> 💡 **Na prática:** Se você nunca trabalhou com EC2-Classic, isso é um bom sinal.

---

## 🔹 Protocolos Suportados

O Classic Load Balancer suporta:

- ✅ **TCP**
- ✅ **SSL / TLS**
- ✅ **HTTP**
- ✅ **HTTPS**

**Apesar disso, ele:**
- ❌ Não possui roteamento avançado
- ❌ Não entende contexto de aplicação
- ❌ Não possui target groups

---

## 🔹 Diferenças Arquiteturais Relevantes

### Ausência de Target Groups

**Diferente do ALB e do NLB:**

O CLB associa instâncias **diretamente**

**Não existe o conceito de target group**

**Isso gera:**
- ❌ Menor flexibilidade
- ❌ Arquitetura mais acoplada
- ❌ Dificuldade de evolução

---

### Funcionalidades Exclusivas do CLB

**Apesar de limitado, o CLB possui algumas características que o ALB não tem:**

- ✅ Suporte ao EC2-Classic
- ✅ Listeners TCP e SSL nativos
- ✅ Sticky sessions baseadas em cookies gerados pela aplicação

> ⚠️ **Observação honesta:** Essas "vantagens" raramente justificam o uso do CLB em arquiteturas modernas.

---

## 🔹 Cross-Zone Load Balancing

Assim como no NLB:

**Cross-Zone Load Balancing pode ser ativado ou desativado**

**Isso exige atenção ao balanceamento de capacidade entre AZs.**

**Comportamento:**

**Desabilitado:**
- Tráfego fica restrito à AZ
- Cada AZ distribui apenas para suas próprias instâncias

**Habilitado:**
- Tráfego é distribuído entre todas as instâncias
- Independente da AZ

---

## 🔹 Processo de Criação do Classic Load Balancer

**Fluxo resumido:**

1. **EC2 → Load Balancers → Create**

2. **Escolher Classic Load Balancer**

3. **Definir configurações básicas:**
   - Nome
   - VPC (ou EC2-Classic se disponível)
   - Listener (ex: HTTP :80)

4. **Selecionar AZs e subnets**

5. **Associar Security Group**

6. **Configurar Health Checks:**
   - Protocolo
   - Porta
   - Path (para HTTP/HTTPS)
   - Interval e timeout

7. **Selecionar instâncias diretamente**

8. **Criar o load balancer**

> ⚠️ **Nota prática:** A ausência de target groups deixa o processo aparentemente simples, mas arquiteturalmente pobre.

---

## 🔹 Comparação Geral entre ELBs

| Característica | CLB | ALB | NLB |
|----------------|-----|-----|-----|
| **Status** | ⚠️ Legado | ✅ Atual | ✅ Atual |
| **Camada OSI** | 4 e 7 | 7 | 4 |
| **Target Groups** | ❌ | ✅ | ✅ |
| **Roteamento avançado** | ❌ | ✅ | ❌ |
| **IP Estático** | ❌ | ❌ | ✅ |
| **EC2-Classic** | ✅ | ❌ | ❌ |
| **Microserviços** | ❌ | ✅ | ⚠️ Limitado |
| **Containers** | ❌ | ✅ | ✅ |
| **Lambda** | ❌ | ✅ | ❌ |
| **Performance** | Média | Alta | Extremamente alta |

> 💡 **Opinião técnica direta:** ALB é a escolha padrão, NLB é a escolha especializada, CLB é a escolha de legado.

---

## 🔹 Limitações do Classic Load Balancer

### Arquitetura

- ❌ **Sem target groups** - Instâncias associadas diretamente
- ❌ **Sem roteamento avançado** - Não entende paths, headers, etc.
- ❌ **Menos flexível** - Difícil evoluir arquitetura
- ❌ **Sem integração moderna** - Limitado com containers e Lambda

### Segurança

- ⚠️ **Menos recursos de segurança** - Comparado com ALB/NLB
- ⚠️ **Sem WAF nativo** - Integração limitada
- ⚠️ **Menos observabilidade** - Métricas menos detalhadas

### Performance

- ⚠️ **Performance inferior** - Comparado com ALB e NLB
- ⚠️ **Menos escalável** - Limitações arquiteturais

---

## 🔹 Quando (NÃO) Usar CLB

### ❌ Não Use CLB Para:

**Novos projetos:**
- Sempre prefira ALB ou NLB
- CLB não oferece vantagens técnicas

**Arquiteturas modernas:**
- Microserviços → Use ALB
- Containers → Use ALB ou NLB
- APIs REST → Use ALB

**Aplicações que precisam de:**
- Roteamento avançado → Use ALB
- Alta performance → Use NLB
- Integração com Lambda → Use ALB

---

### ⚠️ Use CLB Apenas Se:

**Migração de sistemas legados:**
- Sistema já usa CLB e migração é complexa
- Temporário durante migração para ALB/NLB

**EC2-Classic (raro):**
- Ambiente legado em EC2-Classic
- Não há opção de migrar para VPC

> 💡 **Recomendação:** Mesmo nesses casos, planeje migração para ALB ou NLB.

---

## 🔹 Migração de CLB para ALB/NLB

### Por Que Migrar?

**Benefícios:**
- ✅ Target groups para maior flexibilidade
- ✅ Roteamento avançado (ALB)
- ✅ Melhor performance
- ✅ Integração com containers e Lambda
- ✅ Mais recursos de segurança
- ✅ Melhor observabilidade

### Processo de Migração

1. **Avaliar arquitetura atual**
   - Identificar dependências do CLB
   - Mapear instâncias e configurações

2. **Escolher destino (ALB ou NLB)**
   - ALB para aplicações HTTP/HTTPS
   - NLB para protocolos TCP/UDP

3. **Criar novo load balancer**
   - Configurar target groups
   - Configurar listeners e regras

4. **Testar em ambiente isolado**
   - Validar funcionalidade
   - Comparar performance

5. **Migração gradual (se possível)**
   - Blue/Green deployment
   - Canary releases

6. **Atualizar DNS**
   - Route 53 ou DNS externo
   - TTL baixo para rollback rápido

7. **Monitorar e validar**
   - Métricas no CloudWatch
   - Health checks

8. **Descomissionar CLB**
   - Após validação completa

---

## ❓ Perguntas Frequentes sobre CLB

**Devo usar CLB em um projeto novo?**
**Resposta:** ❌ **Não.** Sempre prefira ALB ou NLB. CLB não oferece vantagens técnicas para novos projetos.

**Existe algum ganho técnico real hoje?**
**Resposta:** ⚠️ **Praticamente nenhum.** As únicas "vantagens" são compatibilidade com EC2-Classic e alguns casos muito específicos de sticky sessions.

**CLB é mais simples que ALB?**
**Resposta:** ⚠️ Aparentemente sim, mas a falta de target groups torna a arquitetura menos flexível e mais difícil de evoluir.

**Posso migrar CLB para ALB facilmente?**
**Resposta:** ✅ Sim, mas requer planejamento. Crie target groups, configure listeners e regras, e atualize DNS.

**CLB ainda recebe atualizações?**
**Resposta:** ⚠️ Recebe correções de segurança e bugs críticos, mas não recebe novos recursos. AWS recomenda migração para ALB/NLB.

---

## ⚠️ Erros Comuns com CLB

**❌ Usar CLB por desconhecimento das opções modernas**
**Problema:** Perda de funcionalidades e flexibilidade
**Solução:** Sempre avaliar ALB e NLB primeiro

**❌ Migrar sistemas novos para CLB por "simplicidade"**
**Problema:** Dívida técnica e limitações futuras
**Solução:** Usar ALB ou NLB desde o início

**❌ Ignorar limitações de arquitetura e evolução**
**Problema:** Dificuldade de escalar e evoluir
**Solução:** Planejar migração para ALB/NLB

**❌ Não considerar custos de longo prazo**
**Problema:** CLB pode ter custos similares com menos funcionalidades
**Solução:** Avaliar custo-benefício de ALB/NLB

---

## 🔗 Recursos Adicionais

### Documentação Oficial
- [Elastic Load Balancing – Comparison of Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
- [Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)

### Artigos Recomendados
- Migrating from Classic Load Balancer
- Best Practices for Load Balancing

---

## ✅ Checklist de Prática

- [x] Entendi o papel histórico do CLB
- [x] Sei por que ele é considerado legado
- [x] Diferenciei CLB, ALB e NLB
- [x] Compreendi as limitações do CLB
- [ ] Avaliar migração de CLB para ALB/NLB (se aplicável)
- [ ] Planejar estratégia de migração
- [ ] Testar migração em ambiente isolado

---

## 📊 Resumo

O **Classic Load Balancer** é um componente historicamente importante, mas tecnicamente superado, mantido apenas para compatibilidade com arquiteturas antigas baseadas em EC2-Classic.

**Principais pontos:**
- ⚠️ **Status:** Legado - não recomendado para novos projetos
- ❌ **Sem target groups** - Menos flexível que ALB/NLB
- ❌ **Sem roteamento avançado** - Limitado para arquiteturas modernas
- ⚠️ **EC2-Classic** - Suporte a ambiente descontinuado
- ✅ **Migração recomendada** - Para ALB ou NLB

**Próximos Passos:**
- Ver [Application Load Balancer (ALB)](./alb.md) para alternativa moderna
- Ver [Network Load Balancer (NLB)](./nlb.md) para alta performance
- Planejar migração se estiver usando CLB
- Estudar Auto Scaling Groups e integração com ELB

---

**Última atualização:** 📅 09/01/2026


