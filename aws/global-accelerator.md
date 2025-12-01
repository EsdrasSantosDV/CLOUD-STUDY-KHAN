# 🚀 AWS Global Accelerator

> Serviço global que otimiza o caminho de tráfego TCP e UDP entre usuários e aplicações

---

## 📌 Resumo

**AWS Global Accelerator** é um serviço **global** (não regional) que otimiza o caminho de tráfego TCP e UDP entre usuários e sua aplicação, utilizando diretamente a infraestrutura privada da AWS — evitando a internet pública, reduzindo latência, aumentando confiabilidade e segurança.

Ele fornece **2 IPs estáticos globais** e um **DNS fixo**, que permanecem constantes mesmo que as regiões, endpoints ou balanceadores mudem.

---

## 🎯 O que é

O Global Accelerator é um serviço de rede que:

- ✅ Utiliza a infraestrutura privada da AWS (não a internet pública)
- ✅ Fornece 2 IPs estáticos globais que nunca mudam
- ✅ Reduz latência globalmente
- ✅ Aumenta confiabilidade e segurança
- ✅ Suporta failover automático multi-região
- ✅ Funciona com TCP e UDP

---

## ⚙️ Como Funciona

### Fluxo de Tráfego

```
Cliente → Edge Location AWS → Backbone Global AWS → Aplicação
```

Ao invés de trafegar pela internet pública, o tráfego do cliente:

1. Entra por um **edge location** da AWS mais próximo
2. Entra na **rede backbone global** da AWS
3. Segue até sua aplicação pelo **caminho mais rápido**

### Roteamento Inteligente

O serviço faz roteamento inteligente, considerando:

- 🎯 **Menor latência** - escolhe o caminho mais rápido
- 💚 **Saúde dos endpoints** - roteia apenas para endpoints saudáveis
- ⚖️ **Regiões ativas e pesos** - distribui tráfego conforme configurado
- 🔄 **Failover automático** - muda automaticamente se um endpoint falhar

### Benefícios

- ✅ Melhora alta disponibilidade
- ✅ Failover multi-região automático
- ✅ Aumenta resiliência da aplicação

---

## 🏗️ Arquitetura Básica

```
Global Accelerator
    ↓
Listener
    ↓
Endpoint Groups (um por região)
    ↓
Endpoints (ALB, NLB, EC2 ou EIP)
```

### Hierarquia de Componentes

1. **Accelerator** - Nome do acelerador
2. **Listener** - Recebe conexões (TCP/UDP)
3. **Endpoint Groups** - Agrupa endpoints por região
4. **Endpoints** - Destinos finais (ALB, NLB, EC2, EIP)

---

## 🎁 Benefícios Principais

### 1. IP Estático Global

- ✅ **2 IPv4 estáticos** que nunca mudam
- ✅ Útil para:
  - Firewalls e whitelists
  - Integrações com sistemas legados
  - Migrações sem mudança de IP

### 2. Failover Automático Multi-Região

- ✅ Roteia apenas para endpoints saudáveis
- ✅ Failover automático entre regiões
- ✅ Alta disponibilidade garantida

### 3. Menor Latência Global

- ✅ Conecta o usuário ao edge mais próximo
- ✅ Trafega pela rede interna AWS (não internet pública)
- ✅ Reduz latência significativamente

### 4. Suporte a TCP e UDP

- ✅ Ideal para:
  - APIs
  - Jogos online
  - VoIP
  - VPNs
  - Workloads interativas

### 5. Blue/Green Multi-Região

- ✅ Via **Traffic Dial** (percentual por região)
- ✅ Permite testes e migrações graduais

---

## 🧩 Componentes

### 1. Accelerator

Nome do acelerador que agrupa todos os componentes.

**Seleção de IPs:**
- **AWS IP pool** (padrão) - IPs gerenciados pela AWS
- **BYOIP** (Bring Your Own IP) - Traga seus próprios IPs

**Sempre fornece 2 IPv4 estáticos.**

### 2. Listener

Recebe conexões dos clientes.

**Configurações:**
- **Protocolo**: TCP ou UDP
- **Portas**: Portas que o listener escuta
- **Client Affinity**: 
  - `NONE` - Sem afinidade (padrão)
  - `SOURCE_IP` - Afinidade por IP de origem (útil para aplicações stateful)

### 3. Endpoint Groups

Um grupo por região AWS.

**Configurações:**
- **Traffic Dial** (0–100%) → Controla quanto tráfego vai para a região
- **Health Checks** → Determina se o endpoint é saudável
  - Protocolo (TCP, HTTP, HTTPS)
  - Intervalo de verificação
  - Threshold de falhas

### 4. Endpoints

Destinos finais que podem ser:

- **Application Load Balancer (ALB)**
- **Network Load Balancer (NLB)**
- **EC2 instance**
- **Elastic IP**

**Peso do Endpoint:**
- Cada endpoint tem um peso (0–255)
- Peso 0 = endpoint desabilitado
- Tráfego é distribuído proporcionalmente aos pesos

---

## 🔄 Ciclo de Roteamento

### Fluxo Completo

1. **Cliente conecta** no IP estático do Global Accelerator
2. **Edge AWS mais próximo** recebe a conexão
3. **AWS decide a melhor região** baseado em:
   - Saúde dos endpoints
   - Latência
   - Pesos configurados
4. **Trafega pela rede interna AWS** até o endpoint
5. **Resposta retorna** pelo mesmo caminho otimizado

### Failover Automático

Caso um endpoint ou região fique indisponível:

- ⚠️ **Detecção automática** via health checks
- 🔄 **Failover imediato** para outro endpoint/região saudável
- ✅ **Zero downtime** para o usuário final

---

## 💡 Quando Usar Global Accelerator

### ✅ É Indicado Quando:

Sua aplicação:

- 🌍 **Atende clientes globalmente**
- ⚡ **Tolera muito pouco latência** de rede
- 🔌 **Usa protocolos UDP ou TCP** de longa duração
- 🔄 **Precisa de failover multi-região** imediato
- 🌐 **Não é baseada exclusivamente em HTTP/HTTPS** (caso em que CloudFront seria suficiente)

### 🎮 Casos de Uso Comuns:

- 🎮 **Jogos online** - Baixa latência crítica
- 📞 **VoIP** - Comunicação em tempo real
- 🎥 **Vídeo em tempo real** - Streaming interativo
- 🔌 **APIs globais** - Com tráfego sensível à latência
- 🏦 **Bancos/Fintechs** - Necessidade de IP fixo global

### ⚠️ Quando NÃO Usar:

- ❌ Aplicações apenas HTTP/HTTPS → Use **CloudFront**
- ❌ Aplicações regionais simples → Use **ALB/NLB** diretamente
- ❌ Orçamento limitado → Global Accelerator tem custo adicional

---

## 🔍 Comparação: Global Accelerator vs CloudFront

| Característica | Global Accelerator | CloudFront |
|----------------|-------------------|------------|
| **Protocolos** | TCP, UDP | HTTP, HTTPS |
| **IP Estático** | ✅ Sim (2 IPs) | ❌ Não |
| **Caso de Uso** | APIs, Jogos, VoIP | Websites, CDN |
| **Latência** | Reduzida | Reduzida (cache) |
| **Failover** | Multi-região automático | Via DNS |

---

## 📊 Exemplo de Arquitetura

```
                    ┌─────────────────┐
                    │  Global         │
                    │  Accelerator    │
                    │  (2 IPs fixos) │
                    └────────┬────────┘
                             │
                    ┌────────┴────────┐
                    │                 │
            ┌───────▼──────┐  ┌───────▼──────┐
            │   Listener   │  │   Listener   │
            │   (TCP:443)  │  │   (UDP:53)   │
            └───────┬──────┘  └───────┬──────┘
                    │                 │
        ┌───────────┴──────────┐     │
        │                      │     │
┌───────▼──────┐      ┌───────▼──────▼──────┐
│ Endpoint     │      │ Endpoint            │
│ Group        │      │ Group               │
│ (us-east-1)  │      │ (eu-west-1)         │
│ Traffic: 70% │      │ Traffic: 30%        │
└───────┬──────┘      └───────┬─────────────┘
        │                     │
┌───────▼──────┐      ┌───────▼─────────────┐
│ ALB          │      │ NLB                 │
│ (Weight: 100)│      │ (Weight: 100)       │
└──────────────┘      └─────────────────────┘
```

---

## 💰 Custos

- **Taxa fixa**: Por acelerador ativo
- **Taxa de transferência**: Por GB transferido
- **Health checks**: Custos adicionais para health checks

> 💡 **Dica**: Use o [AWS Pricing Calculator](https://calculator.aws/) para estimar custos

---

## 📝 Comandos Úteis

### AWS CLI

```bash
# Criar um Global Accelerator
aws globalaccelerator create-accelerator \
  --name my-accelerator \
  --ip-address-type IPV4

# Listar aceleradores
aws globalaccelerator list-accelerators

# Criar um Listener
aws globalaccelerator create-listener \
  --accelerator-arn arn:aws:globalaccelerator::account:accelerator/xxx \
  --protocol TCP \
  --port-ranges FromPort=443,ToPort=443

# Adicionar Endpoint Group
aws globalaccelerator create-endpoint-group \
  --listener-arn arn:aws:globalaccelerator::account:listener/xxx \
  --endpoint-group-region us-east-1 \
  --traffic-dial-percentage 100

# Adicionar Endpoint
aws globalaccelerator create-endpoint \
  --endpoint-group-arn arn:aws:globalaccelerator::account:endpoint-group/xxx \
  --endpoint-id alb-arn \
  --weight 100
```

---

## 🔗 Recursos Adicionais

- [Documentação Oficial AWS Global Accelerator](https://docs.aws.amazon.com/global-accelerator/)
- [AWS Global Accelerator - Página do Produto](https://aws.amazon.com/global-accelerator/)
- [AWS Well-Architected - Networking](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/networking.html)

---

## ✅ Checklist de Aprendizado


---

## 🏷️ Tags

`#aws` `#networking` `#global-accelerator` `#latency` `#high-availability` `#failover` `#tcp` `#udp`

---

**Última atualização**: 📅 [DD/MM/YYYY]

