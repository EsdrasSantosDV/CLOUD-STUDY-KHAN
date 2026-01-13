# 🎯 Application Load Balancer (ALB)

> Load balancer de camada 7 para aplicações HTTP/HTTPS modernas

---

## 📌 Resumo

O **Application Load Balancer (ALB)** é um load balancer que opera na **camada 7 do modelo OSI**, a camada de aplicação.

Isso significa que ele **entende o conteúdo da requisição**, e não apenas conexões ou pacotes.

**Analogia de engenharia:**
O ALB não é apenas um "porteiro". Ele é um despachante inteligente, que lê o conteúdo da encomenda antes de decidir para qual setor da fábrica ela deve ir.

Por isso, o ALB é indicado para aplicações HTTP/HTTPS modernas, especialmente:
- ✅ Microserviços
- ✅ Containers
- ✅ APIs REST
- ✅ Arquiteturas orientadas a rotas e regras

---

## 🔹 ALB e o Modelo OSI

**Camada 7 (Application Layer):**
- ✅ Atua na camada de aplicação
- ✅ Trabalha com protocolos HTTP e HTTPS
- ✅ Permite decisões de roteamento baseadas em:
  - Path (caminho da URL)
  - Host (domínio)
  - Headers HTTP
  - Source IP
  - Método HTTP (GET, POST, PUT, DELETE, etc.)

> 💡 **Opinião técnica:** É exatamente essa inteligência de camada 7 que faz o ALB ser o load balancer "padrão" para aplicações web modernas.

---

## 🔹 Quando Usar o ALB

A própria AWS recomenda o ALB quando você precisa de:

- ✅ **Roteamento avançado** baseado em conteúdo
- ✅ **Alta visibilidade de tráfego** e métricas detalhadas
- ✅ **Integração com arquiteturas de aplicação** (e não apenas infraestrutura)
- ✅ **TLS termination** centralizado
- ✅ **Integração com containers** (ECS/EKS) e Lambda

---

## 🔹 Target Groups no ALB

**O que é um Target Group?**

Um **target group** é um conjunto de recursos que receberá tráfego do ALB:

- ✅ EC2 instances
- ✅ IPs privados
- ✅ Containers (ECS/EKS)
- ✅ Lambda functions (em cenários específicos)

**Boa prática:**
Criar target groups **antes** do ALB, pois o ALB depende deles para encaminhar tráfego.

**Exemplo clássico:**
```
Target Group A → HTTP (porta 80) - Web Servers
Target Group B → HTTPS (porta 443) - Secure API
Target Group C → API específica - Microservice
```

---

## 🔹 Configuração de Health Checks

**Cada target group possui configurações de health check:**

- ✅ **Protocolo** (HTTP, HTTPS, TCP)
- ✅ **Path** (ex: `/health`, `/index.html`)
- ✅ **Porta** de verificação
- ✅ **Interval** (tempo entre verificações)
- ✅ **Timeout** (tempo máximo de espera)
- ✅ **Healthy threshold** (sucessos consecutivos para marcar como saudável)
- ✅ **Unhealthy threshold** (falhas consecutivas para marcar como não saudável)

**Isso permite que o ALB:**
- ✅ Detecte falhas automaticamente
- ✅ Pare de enviar tráfego para targets problemáticos
- ✅ Restaure tráfego quando o target voltar a responder

**Analogia:**
Health check é o batimento cardíaco da infraestrutura. Parou de responder, o ALB afasta o nó do sistema.

---

## 🔹 Criação do Application Load Balancer

**Principais etapas de configuração:**

1. **Escolher tipo:** Application Load Balancer

2. **Definir configurações básicas:**
   - Nome do ALB
   - Internet-facing ou internal
   - IP version (IPv4 ou dual-stack)

3. **Configurar listeners:**
   - HTTP :80 (para redirecionamento)
   - HTTPS :443 (recomendado para produção)

4. **Associar Availability Zones e subnets:**
   - Selecionar pelo menos 2 AZs
   - Escolher subnets públicas (internet-facing) ou privadas (internal)

5. **Configurar Security Group:**
   - Permitir tráfego HTTP/HTTPS de entrada
   - Permitir comunicação com targets

6. **Associar target groups:**
   - Selecionar target groups existentes
   - Configurar regras de roteamento

7. **Revisar e criar**

> ⚠️ **Observação prática:** Em produção, HTTP puro raramente é aceitável para internet-facing ALBs. Sempre use HTTPS com certificados. Veja [Certificados SSL/TLS no ALB](./alb-certificados.md).

---

## 🔹 Listeners e Rules - Detalhamento

### Listeners

**O que é um Listener?**

Um **listener** define:
- ✅ **Porta** de entrada (80, 443, etc.)
- ✅ **Protocolo** (HTTP, HTTPS, TCP)
- ✅ **Conjunto de regras** de roteamento

**Importante:** Todo ALB precisa de pelo menos um listener.

---

### Rules (Regras)

**As regras determinam como o tráfego é roteado.**

**Estrutura lógica:**
```
IF (condição)
THEN (ação)
```

**Exemplos de condições:**
- ✅ **Source IP** - IP de origem da requisição
- ✅ **Path** - Caminho da URL (ex: `/api/*`, `/app/*`)
- ✅ **Host header** - Domínio da requisição
- ✅ **HTTP headers** - Headers customizados
- ✅ **Query strings** - Parâmetros da URL
- ✅ **Método HTTP** - GET, POST, PUT, DELETE, etc.

**Exemplo prático:**
```
IF source IP = 10.0.1.0/24
THEN forward → Target Group B (Internal API)
```

**Exemplo de roteamento por path:**
```
IF path = /api/*
THEN forward → Target Group API
ELSE IF path = /app/*
THEN forward → Target Group Frontend
ELSE
THEN forward → Target Group Default
```

> 💡 **Minha opinião:** Rules bem definidas transformam o ALB em um router de aplicação, não apenas um balanceador.

---

## 🔹 Monitoramento do ALB

**O ALB integra-se diretamente com CloudWatch Metrics:**

**Métricas disponíveis:**
- ✅ **Targets saudáveis** - Número de targets healthy
- ✅ **Targets não saudáveis** - Número de targets unhealthy
- ✅ **Latência** - Tempo de resposta (média, p50, p99)
- ✅ **Número de requisições** - Total de requisições processadas
- ✅ **Códigos HTTP** - Distribuição de códigos de resposta (2xx, 4xx, 5xx)
- ✅ **Request count por target** - Distribuição de carga

**Isso facilita:**
- ✅ Troubleshooting de problemas
- ✅ Decisões de scaling
- ✅ Análise de performance
- ✅ Identificação de gargalos

---

## 🔹 Casos de Uso do ALB

### Separação de Frontend e Backend
```
/ → Frontend (React, Angular, Vue)
/api/* → Backend (REST API)
```

### Roteamento por Path
```
/api/users → User Service
/api/products → Product Service
/api/orders → Order Service
```

### Ambientes Multi-Tenant
```
tenant1.example.com → Tenant 1 Resources
tenant2.example.com → Tenant 2 Resources
```

### Canary Releases
- Distribuir pequena porcentagem de tráfego para nova versão
- Monitorar métricas antes de fazer rollout completo

### Blue/Green Deployments
- Target Group Blue → Versão atual (produção)
- Target Group Green → Nova versão (teste)
- Trocar roteamento gradualmente

---

## ❓ Perguntas Frequentes sobre ALB

**ALB substitui API Gateway?**
**Resposta:** ❌ Não, mas pode complementar. API Gateway oferece features adicionais como rate limiting, autenticação, transformação de requests, e integração com serviços AWS.

**Posso ter múltiplos target groups?**
**Resposta:** ✅ Sim, e é comum. Cada regra pode rotear para um target group diferente.

**ALB funciona com Lambda?**
**Resposta:** ✅ Sim, ALB pode rotear diretamente para funções Lambda, sem necessidade de API Gateway.

**Qual a diferença entre ALB e NLB para APIs?**
**Resposta:** ALB é melhor para APIs HTTP/HTTPS que precisam de roteamento baseado em conteúdo. NLB é melhor quando precisa de latência mínima ou protocolos customizados.

---

## ⚠️ Erros Comuns com ALB

**❌ Criar ALB sem planejar target groups**
**Problema:** Dependência circular ou configuração incorreta
**Solução:** Sempre criar e configurar target groups antes de criar o ALB

**❌ Health check mal configurado derrubando instâncias saudáveis**
**Problema:** Timeout muito baixo ou path incorreto
**Solução:** Configurar health checks adequados para sua aplicação, testar antes de produção

**❌ Usar apenas regra default em arquiteturas complexas**
**Problema:** Não aproveita o potencial de roteamento avançado
**Solução:** Planejar regras baseadas em path, host ou headers para diferentes serviços

**❌ Não configurar redirecionamento HTTP → HTTPS**
**Problema:** Tráfego não criptografado ainda acessível
**Solução:** Configurar listener HTTP que redireciona para HTTPS

**❌ Security Groups bloqueando comunicação ALB → Targets**
**Problema:** Tráfego não chega aos targets
**Solução:** Permitir tráfego do security group do ALB nos security groups dos targets

---

## 🔗 Integrações Específicas do ALB

**Auto Scaling Groups:**
- ALB integra automaticamente com ASG
- Targets são adicionados/removidos automaticamente
- Health checks do ALB influenciam decisões de scaling

**ECS (Elastic Container Service):**
- Integração nativa com serviços ECS
- Target groups podem conter containers diretamente
- Service discovery automático

**EKS (Elastic Kubernetes Service):**
- Integração com Kubernetes services
- Suporte a ingress controllers
- Roteamento para pods

**Lambda:**
- Roteamento direto para funções Lambda
- Sem necessidade de API Gateway
- Suporte a content-based routing

**WAF (Web Application Firewall):**
- Proteção contra ataques comuns na camada 7
- Rate limiting e filtros customizados
- Integração nativa com ALB

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [Application Load Balancer Overview](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [ALB Routing Patterns](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)

### Artigos Recomendados
- ALB Routing Patterns
- Best Practices for Application Load Balancer

---

## ✅ Checklist de Prática

- [x] Compreendi o ALB na camada 7 do modelo OSI
- [x] Sei quando usar o ALB
- [ ] Criar target groups
- [ ] Configurar health checks adequados
- [ ] Criar um ALB internet-facing
- [ ] Criar um ALB internal
- [ ] Associar listeners e target groups
- [ ] Criar regras avançadas (path-based routing)
- [ ] Implementar roteamento por host header
- [ ] Configurar HTTPS com certificados (ver [Certificados SSL/TLS](./alb-certificados.md))
- [ ] Integrar ALB com Auto Scaling Groups
- [ ] Testar failover na prática
- [ ] Monitorar métricas no CloudWatch
- [ ] Integrar ALB com ECS/EKS
- [ ] Configurar roteamento para Lambda

---

## 📊 Resumo

O **Application Load Balancer** é o coração do roteamento de aplicações HTTP/HTTPS na AWS, oferecendo:
- ✅ Inteligência de camada 7 (entende conteúdo das requisições)
- ✅ Alta disponibilidade automática
- ✅ Flexibilidade arquitetural com roteamento avançado
- ✅ Integração nativa com containers, Lambda e microserviços
- ✅ Health checks automáticos e failover inteligente

**Próximos Passos:**
- Configurar HTTPS com certificados (ver [Certificados SSL/TLS no ALB](./alb-certificados.md))
- Integrar ALB com Auto Scaling Groups
- Explorar regras avançadas (path e host-based routing)
- Implementar blue/green deployments
- Configurar canary releases

---

**Última atualização:** 📅 09/01/2026


