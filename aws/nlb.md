# 🚀 Network Load Balancer (NLB)

> Load balancer de camada 4 para alto desempenho e baixa latência

---

## 📌 Resumo

O **Network Load Balancer (NLB)** é o load balancer da AWS projetado para alto desempenho, baixa latência e simplicidade de decisão.

Enquanto o ALB "lê" a requisição, o NLB apenas encaminha conexões, com base em informações de rede.

**Analogia de engenharia:**
Se o ALB é um despachante que lê o conteúdo da carga, o NLB é uma central ferroviária: não importa o que está no vagão, apenas para qual trilho a composição deve seguir.

---

## 🔹 Camada 4 do Modelo OSI

O NLB opera na **camada 4 (Transport Layer)** do modelo OSI:

- ✅ **TCP**
- ✅ **UDP**
- ✅ **TLS** (pass-through ou terminação)

**Ele não analisa:**
- ❌ Headers HTTP
- ❌ Paths ou métodos
- ❌ Conteúdo da requisição

**A decisão é feita com base em:**
- ✅ Protocolo
- ✅ IP de origem
- ✅ Porta de origem
- ✅ IP de destino
- ✅ Porta de destino

---

## 🔹 Comparação ALB vs NLB

| Característica | ALB | NLB |
|----------------|-----|-----|
| **Camada OSI** | 7 | 4 |
| **Protocolos** | HTTP / HTTPS | TCP / UDP / TLS |
| **Análise de conteúdo** | Sim | Não |
| **Performance** | Alta | Extremamente alta |
| **IP estático** | Não | Sim |
| **Casos de uso** | Apps web, APIs | Infra, DNS, streaming |

> 💡 **Opinião técnica:** Sempre que alguém tenta usar ALB para um problema de camada 4, o erro não é técnico — é conceitual.

---

## 🔹 Características-Chave do NLB

### Ultra Alto Desempenho

- ✅ Suporta **milhões de requisições por segundo**
- ✅ **Latência extremamente baixa**
- ✅ Ideal para workloads sensíveis a tempo de resposta

---

### IP Estático

**O NLB fornece:**
- ✅ **Um IP estático por Availability Zone**

**Isso é crítico para sistemas que:**
- Exigem whitelisting
- Usam DNS, firewalls externos ou sistemas legados
- Precisam de IPs previsíveis para integração

---

### Persistência de Conexão

**Uma vez que a conexão é estabelecida:**
- ✅ Ela permanece com o mesmo target
- ✅ Até o encerramento da sessão

**Isso é essencial para:**
- ✅ Protocolos stateful
- ✅ UDP
- ✅ Sistemas de infraestrutura

---

## 🔹 Cross-Zone Load Balancing no NLB

**Diferente do ALB:**

No NLB, cross-zone pode ser **habilitado ou desabilitado**

**Comportamento:**

**Desabilitado:**
- Tráfego fica restrito à AZ
- Cada AZ distribui apenas para seus próprios targets
- Pode reduzir custos de transferência de dados

**Habilitado:**
- Tráfego é distribuído entre todos os targets
- Independente da AZ
- Melhor distribuição de carga

> ⚠️ **Observação prática:** Desabilitar cross-zone pode reduzir custo, mas exige simetria perfeita de capacidade entre AZs.

---

## 🔹 Algoritmo de Seleção de Target

O NLB utiliza um algoritmo baseado em:

- ✅ **Sequence** (sequência)
- ✅ **Protocolo**
- ✅ **Source IP / Port**
- ✅ **Destination IP / Port**

**Esse conjunto define qual target receberá a conexão.**

**Analogia:**
É como um hash de rede: mesma origem, mesmo destino, mesmo caminho.

---

## 🔹 Configuração do Network Load Balancer

### Etapas Principais

1. **Criar Load Balancer → Network Load Balancer**

2. **Definir configurações básicas:**
   - Nome
   - Internal ou Internet-facing
   - IP version (IPv4 ou dual-stack)

3. **Configurar listener:**
   - TCP / UDP / TLS
   - Porta (ex: 80, 443, 53)

4. **Associar Availability Zones e subnets:**
   - Selecionar pelo menos 2 AZs
   - Escolher subnets públicas (internet-facing) ou privadas (internal)

5. **Criar ou associar target group:**
   - Protocolo (TCP, UDP, TLS)
   - Porta de destino
   - Health check

6. **Registrar targets:**
   - EC2 instances
   - IPs privados
   - Containers

7. **Revisar e criar**

---

### Exemplo Prático (DNS)

**Cenário:** Load balancer para servidores DNS

**Configuração:**
- **Listener:** UDP :53
- **Tipo:** Internal NLB
- **Target group:**
  - Protocolo: UDP
  - Porta: 53
  - Health check: TCP

**Esse cenário é clássico de NLB, impossível de resolver corretamente com ALB.**

---

## 🔹 Casos de Uso do NLB

### Infraestrutura e Sistemas

**DNS:**
- Balanceamento de servidores DNS
- Protocolo UDP na porta 53
- Alta disponibilidade para resolução de nomes

**Firewalls e Proxies:**
- Balanceamento de tráfego de rede
- Protocolos customizados
- Integração com sistemas legados

---

### Streaming e Mídia

**Streaming de vídeo/áudio:**
- Protocolos UDP para streaming
- Baixa latência crítica
- Alta throughput

**Gaming:**
- Servidores de jogos online
- Latência mínima essencial
- Protocolos customizados

---

### Sistemas Financeiros

**Trading de alta frequência:**
- Latência extremamente baixa
- Milhões de transações por segundo
- Protocolos customizados

---

### APIs de Alta Performance

**Quando performance > features:**
- APIs que não precisam de roteamento por path
- Protocolos não-HTTP
- Necessidade de IP estático

---

## 🔹 Monitoramento do NLB

**O NLB se integra ao CloudWatch, oferecendo métricas como:**

- ✅ **Conexões ativas** - Número de conexões simultâneas
- ✅ **Targets saudáveis / não saudáveis** - Status dos targets
- ✅ **Fluxo de tráfego por AZ** - Distribuição de tráfego
- ✅ **Bytes processados** - Volume de dados
- ✅ **Latência** - Tempo de resposta

**Mesmo sendo "simples", o NLB é totalmente observável.**

---

## ❓ Perguntas Frequentes sobre NLB

**Posso usar NLB para APIs REST?**
**Resposta:** ⚠️ Tecnicamente sim, arquiteturalmente não. NLB não entende HTTP, então você perderia features como roteamento por path, headers, etc. Use ALB para APIs REST.

**NLB substitui ALB?**
**Resposta:** ❌ Não, são ferramentas diferentes. ALB é para camada 7 (HTTP/HTTPS), NLB é para camada 4 (TCP/UDP).

**Quando usar NLB ao invés de ALB?**
**Resposta:** Use NLB quando:
- Precisa de latência extremamente baixa
- Trabalha com protocolos não-HTTP (UDP, TCP customizado)
- Precisa de IP estático
- Performance é mais importante que features de aplicação

**NLB suporta HTTPS?**
**Resposta:** ✅ Sim, NLB suporta TLS (camada 4), mas não faz terminação TLS como o ALB. Você pode fazer pass-through ou terminação TLS no NLB.

**Qual a diferença entre NLB e Classic Load Balancer?**
**Resposta:** NLB é mais moderno, oferece IP estático, melhor performance e suporta apenas TCP/UDP/TLS. CLB é legado e suporta HTTP/HTTPS também, mas não é recomendado para novos projetos.

---

## ⚠️ Erros Comuns com NLB

**❌ Usar NLB quando se precisa de roteamento por path**
**Problema:** NLB não entende HTTP, não pode rotear por path
**Solução:** Use ALB para roteamento baseado em conteúdo

**❌ Esquecer de avaliar cross-zone load balancing**
**Problema:** Distribuição desigual de carga entre AZs
**Solução:** Avaliar se cross-zone deve ser habilitado baseado em custo e simetria de capacidade

**❌ Tentar aplicar lógica de aplicação em camada 4**
**Problema:** NLB não tem acesso a headers HTTP, paths, etc.
**Solução:** Se precisa de lógica de aplicação, use ALB ou adicione camada adicional

**❌ Não configurar health checks adequados**
**Problema:** Targets podem ser marcados como unhealthy incorretamente
**Solução:** Configurar health checks apropriados para o protocolo usado (TCP, UDP, TLS)

**❌ Usar NLB para aplicações web simples**
**Problema:** Complexidade desnecessária, perda de features do ALB
**Solução:** Use ALB para aplicações web HTTP/HTTPS, a menos que tenha requisito específico de performance

---

## 🔗 Integrações do NLB

### Auto Scaling Groups
- NLB integra automaticamente com ASG
- Targets são adicionados/removidos automaticamente
- Health checks do NLB influenciam decisões de scaling

### ECS (Elastic Container Service)
- Integração nativa com serviços ECS
- Target groups podem conter containers
- Suporte a service discovery

### EKS (Elastic Kubernetes Service)
- Integração com Kubernetes services
- Suporte a NetworkPolicies
- Roteamento para pods

### Route 53
- Health checks do Route 53 podem usar NLB como endpoint
- Integração para failover automático

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [Network Load Balancer Overview](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)
- [NLB vs ALB Comparison](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancers.html)

### Artigos Recomendados
- Choosing Between ALB and NLB
- Best Practices for Network Load Balancer
- High Performance Networking with NLB

---

## ✅ Checklist de Prática

- [x] Entendi a diferença entre camada 4 e 7
- [x] Compreendi quando usar NLB vs ALB
- [ ] Configurar um NLB
- [ ] Associar target group e targets
- [ ] Configurar health checks adequados
- [ ] Testar cross-zone ligado e desligado
- [ ] Comparar latência com ALB
- [ ] Configurar NLB para DNS (UDP :53)
- [ ] Configurar NLB para TLS
- [ ] Monitorar métricas no CloudWatch
- [ ] Integrar NLB com Auto Scaling Groups

---

## 📊 Resumo

O **Network Load Balancer** é a escolha correta quando desempenho, previsibilidade e simplicidade de rede são mais importantes do que inteligência de aplicação. Ele resolve problemas que o ALB não deve tentar resolver.

**Principais benefícios:**
- ✅ Latência extremamente baixa
- ✅ Milhões de requisições por segundo
- ✅ IP estático por AZ
- ✅ Suporte a TCP, UDP e TLS
- ✅ Persistência de conexão
- ✅ Ideal para infraestrutura e protocolos customizados

**Próximos Passos:**
- Estudar TLS no NLB
- Comparar NLB vs Classic Load Balancer
- Avaliar custos e métricas em workloads reais
- Ver [Application Load Balancer (ALB)](./alb.md) para comparação

---

**Última atualização:** 📅 09/01/2026

