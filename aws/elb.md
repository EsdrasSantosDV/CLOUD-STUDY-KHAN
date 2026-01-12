# ⚖️ AWS Elastic Load Balancer (ELB)

> Serviço gerenciado para distribuição de tráfego e alta disponibilidade

---

## 📌 Resumo

O **AWS Elastic Load Balancer (ELB)** é um serviço gerenciado responsável por receber tráfego de entrada e distribuí-lo de forma equilibrada entre múltiplos recursos computacionais (targets), como EC2, containers, funções Lambda ou IPs.

**Analogia de engenharia civil:**
O ELB funciona como um viaduto de múltiplas faixas, garantindo que o fluxo de veículos (requisições) nunca dependa de uma única pista (instância). Ele elimina gargalos, reduz pontos únicos de falha e permite que a aplicação suporte picos imprevisíveis de tráfego.

---

## 🎯 O que é

O Elastic Load Balancer oferece:

- ✅ **Distribuição uniforme** de requisições entre múltiplos targets
- ✅ **Alta disponibilidade** - elimina pontos únicos de falha
- ✅ **Resiliência** - falhas individuais não derrubam o sistema
- ✅ **Escalabilidade horizontal** - suporta crescimento automático
- ✅ **Totalmente gerenciado** pela AWS
- ✅ **Elasticamente escalável** - sem intervenção manual

> 💡 **Opinião profissional:** Confiar em ELB ao invés de load balancers autogerenciados é uma decisão madura — você terceiriza a complexidade que não gera vantagem competitiva.

---

## 🔹 Tipos de Elastic Load Balancer

### 1️⃣ Application Load Balancer (ALB)

**Camada:** 7 (HTTP/HTTPS)

**Características:**
- ✅ Roteamento avançado por:
  - Path (caminho)
  - Host (domínio)
  - Headers
  - Método HTTP
- ✅ TLS termination
- ✅ Ideal para arquiteturas modernas web e microserviços

**📌 Exemplo clássico:**
Roteamento `/api` para backend e `/app` para frontend, no mesmo domínio.

**📚 Documentação Detalhada:**
- [Guia Completo do ALB](./alb.md)
- [Certificados SSL/TLS no ALB](./alb-certificados.md)

---

### 2️⃣ Network Load Balancer (NLB)

**Camada:** 4 (TCP/UDP)

**Características:**
- ✅ Latência extremamente baixa
- ✅ Suporta milhões de requisições por segundo
- ✅ IP estático por AZ
- ✅ Ideal para protocolos customizados

**📌 Uso típico:** Sistemas financeiros, IoT, streaming, protocolos customizados.

**📚 Documentação Detalhada:**
- [Guia Completo do NLB](./nlb.md)

---

### 3️⃣ Classic Load Balancer (CLB)

**Camada:** 4 e 7 (legado)

**Status:** ⚠️ Legado (EC2-Classic)

**Características:**
- ⚠️ Uso desencorajado para novos projetos
- ⚠️ Mantido apenas por compatibilidade
- ❌ Sem target groups
- ❌ Sem roteamento avançado

> 💡 **Opinião direta:** CLB só deve existir por inércia histórica, nunca por escolha técnica atual.

**📚 Documentação Detalhada:**
- [Guia Completo do CLB](./clb.md) - Contexto histórico e limitações

---

## 🧩 Componentes do ELB

### 🔹 Listeners

**Definem:**
- Porta de entrada
- Protocolo (HTTP, HTTPS, TCP, UDP)
- Associação com regras e target groups

**Importante:** Todo ELB exige pelo menos um listener.

---

### 🔹 Target Groups

**Conjunto de recursos que recebem tráfego:**
- EC2 instances
- Containers (ECS/EKS)
- Lambda functions
- IPs privados

**Cada target group possui:**
- Health checks próprios
- Protocolo e porta definidos
- Configurações de roteamento

---

### 🔹 Rules (Regras)

**Estrutura lógica:**
```
IF (condições)
THEN (ação → target group)
```

**Exemplo conceitual:**
```
IF origem = 10.0.1.0/24
AND método = HTTP PUT
THEN → Target Group A
```

As regras determinam como o tráfego é roteado com base em:
- Path patterns
- Host headers
- HTTP headers
- Query strings
- Source IPs
- HTTP methods

---

### 🔹 Health Checks

**Mecanismo de validação contínua:**
- ✅ Verifica periodicamente a saúde dos targets
- ✅ Se o target não responde dentro do threshold → marcado como unhealthy
- ✅ O ELB automaticamente para de enviar tráfego para targets unhealthy
- ✅ Evita que falhas individuais derrubem o sistema inteiro

**Configurações:**
- Intervalo de verificação
- Timeout
- Threshold de falhas consecutivas
- Caminho de verificação (para ALB)

---


## 🎯 Casos de Uso

### Frontend Web Distribuído
- Distribuição de tráfego entre múltiplas AZs
- Alta disponibilidade para aplicações web

### Arquiteturas de Microserviços
- Roteamento baseado em path para diferentes serviços
- Separação de tráfego por domínio ou header

### Separação de Tráfego
- Tráfego interno vs externo
- Diferentes ambientes (dev, staging, prod)

### Picos Sazonais
- Black Friday, eventos esportivos
- Escalabilidade automática com Auto Scaling Groups

---

## 💡 Conceitos Importantes

### 🔹 Alta Disponibilidade

**Características:**
- ✅ ELB não é ponto único de falha
- ✅ Alta disponibilidade é responsabilidade da AWS
- ✅ Escala automaticamente conforme demanda
- ✅ Trabalha em conjunto com Auto Scaling Groups

---

### 🔹 Internal vs Internet-Facing

**Internet-Facing:**
- Possui DNS público
- Recebe tráfego da Internet
- Exposto publicamente

**Internal:**
- Acessível apenas dentro da VPC
- Sem exposição pública
- Para comunicação interna entre serviços

**📌 Arquitetura clássica:**
```
Internet → ALB Público → Web Servers → ALB Interno → App Servers
```

---

### 🔹 Cross-Zone Load Balancing

**Desabilitado:**
- Tráfego distribuído apenas dentro da AZ
- Cada AZ distribui apenas para seus próprios targets

**Habilitado:**
- Tráfego distribuído igualmente entre todos os targets
- Independente da AZ

> 💡 **Na prática:** Habilitar Cross-Zone é quase sempre a decisão correta, exceto em cenários de custo ou tráfego extremamente sensível.

---

## ❓ Perguntas Frequentes

### Qual ELB usar para APIs modernas?
**Resposta:** Application Load Balancer (ALB)
- Roteamento avançado por path
- Suporte a múltiplos protocolos HTTP/HTTPS
- Integração com containers e Lambda

### Qual ELB usar para latência mínima?
**Resposta:** Network Load Balancer (NLB)
- Latência extremamente baixa
- Milhões de requisições por segundo
- IP estático por AZ

### Quando usar NLB vs ALB?
- **NLB:** Quando precisa de latência mínima, protocolos customizados, ou IP estático
- **ALB:** Quando precisa de roteamento avançado baseado em conteúdo, TLS termination, ou integração com containers/Lambda

---

## ⚠️ Erros Comuns

### ❌ Esquecer de habilitar AZ no ELB
**Problema:** Tráfego não chega ao target se a AZ não estiver habilitada no ELB
**Solução:** Sempre habilitar todas as AZs onde os targets estão localizados

### ❌ Health check mal configurado
**Problema:** Instâncias saudáveis são removidas incorretamente
**Solução:** Configurar timeout, intervalo e threshold adequados para sua aplicação

### ❌ Security Groups mal configurados
**Problema:** Tráfego bloqueado entre ELB e targets
**Solução:** Permitir tráfego do security group do ELB nos security groups dos targets

---

## 🔗 Integrações

### Auto Scaling Groups
- ELB integra automaticamente com ASG
- Targets são adicionados/removidos automaticamente
- Health checks do ELB influenciam decisões de scaling
- Ver [EC2 Auto Scaling](./autoscaling.md) para detalhes completos sobre integração

### Route 53
- Integração com DNS para roteamento
- Health checks do Route 53 podem usar ELB como endpoint

### CloudWatch
- Métricas automáticas de performance
- Monitoramento de requisições, latência, erros
- Alertas configuráveis

### WAF (Web Application Firewall)
- ALB pode integrar com WAF
- Proteção contra ataques comuns na camada 7

---

## 📊 Resumo

### O que aprendi

O **Elastic Load Balancer** é o pilar central de disponibilidade e escalabilidade em arquiteturas AWS modernas, abstraindo falhas, distribuindo carga e permitindo crescimento seguro da aplicação.

**Principais benefícios:**
- ✅ Alta disponibilidade automática
- ✅ Distribuição inteligente de carga
- ✅ Escalabilidade elástica
- ✅ Integração nativa com outros serviços AWS
- ✅ Redução de complexidade operacional

### Documentação Detalhada

Para informações mais detalhadas sobre tipos específicos de Load Balancer:
- [Application Load Balancer (ALB) - Guia Completo](./alb.md)
- [Network Load Balancer (NLB) - Guia Completo](./nlb.md)
- [Classic Load Balancer (CLB) - Contexto Histórico](./clb.md)
- [Certificados SSL/TLS e HTTPS no ALB](./alb-certificados.md)

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [AWS Elastic Load Balancing Overview](https://docs.aws.amazon.com/elasticloadbalancing/)
- [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)
- [HTTPS Listeners for ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)
- [AWS Certificate Manager Overview](https://docs.aws.amazon.com/acm/)

### Artigos Recomendados
- ALB vs NLB: When to Use Each
- Best Practices for Elastic Load Balancing
- TLS Termination Patterns in AWS
- ALB Routing Patterns
- Application Load Balancer Overview

---

## ✅ Checklist de Prática

### Fundamentos
- [x] Compreendi o papel do ELB
- [x] Diferenciei ALB, NLB e CLB
- [x] Entendi listeners, rules e target groups
- [ ] Criar ELB com múltiplas AZs
- [ ] Testar failover na prática
- [ ] Configurar health checks adequados
- [ ] Integrar ELB com Auto Scaling Groups

### Documentação Detalhada
- [ ] Ler [Guia Completo do ALB](./alb.md)
- [ ] Ler [Certificados SSL/TLS no ALB](./alb-certificados.md)

---

**Última atualização:** 📅 09/01/2026

