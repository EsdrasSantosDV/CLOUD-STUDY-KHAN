# 🔒 Certificados SSL/TLS e HTTPS no ALB

> Guia completo sobre certificados e segurança HTTPS no Application Load Balancer

---

## 📌 Introdução

Quando utilizamos HTTPS em um Application Load Balancer, estamos introduzindo criptografia no canal de comunicação entre o cliente e a infraestrutura AWS.

Diferente do HTTP simples (porta 80), o HTTPS (porta 443) exige configuração adicional, pois o load balancer precisa ser capaz de terminar conexões criptografadas.

**Analogia clássica de engenharia:**
O ALB atua como um posto alfandegário criptográfico: ele recebe a comunicação lacrada, valida o selo (certificado), abre o conteúdo com segurança e encaminha a carga para dentro da infraestrutura.

---

## 🔹 HTTPS, SSL e TLS

**HTTPS:** HTTP encapsulado em um túnel criptografado

**SSL (Secure Sockets Layer) e TLS (Transport Layer Security):**
- Protocolos criptográficos
- TLS é o sucessor moderno do SSL
- Na prática, os termos ainda são usados de forma intercambiável

O ALB utiliza **certificados X.509**, que funcionam como uma identidade digital do serviço perante o cliente.

---

## 🔹 Certificado de Servidor no ALB

**O certificado é usado para:**
- ✅ Estabelecer confiança com o cliente
- ✅ Criptografar o tráfego
- ✅ Terminar a conexão TLS no Load Balancer

**Após a terminação:**
- A requisição é descriptografada
- O tráfego segue para o target group (normalmente via HTTP interno)

> 💡 **Opinião técnica:** Terminar TLS no ALB é uma decisão arquitetural excelente. Centraliza segurança, reduz custo computacional nos targets e simplifica observabilidade.

---

## 🔹 Listener HTTPS no Application Load Balancer

Ao configurar um **listener HTTPS (porta 443)**, o ALB exige:

- ✅ **Um certificado de servidor**
- ✅ **Uma política de segurança** (cipher suites, versões TLS)

O listener HTTP (porta 80) não exige certificados e possui configuração mais simples.

---

## 🔹 Opções de Certificados no ALB

Quando se escolhe HTTPS, existem **quatro opções de certificado:**

### Opções Baseadas em ACM (Recomendadas) ✅

**Opções:**
- Selecionar certificado existente no ACM
- Importar um certificado para o ACM

**O AWS Certificate Manager (ACM):**
- ✅ Cria e gerencia certificados SSL/TLS
- ✅ Faz renovação automática
- ✅ Integra-se nativamente ao ALB
- ✅ Reduz erros operacionais

> 💡 **Na prática profissional:** ACM é a escolha padrão sempre que disponível. Menos risco, menos manutenção, menos falhas humanas.

---

### Opções Baseadas em IAM (Legado / Exceção) ⚠️

**Opções:**
- Selecionar certificado armazenado no IAM
- Importar certificado para o IAM

**Essas opções são usadas quando:**
- A região AWS não suporta ACM
- Há dependência de certificados de terceiros legados

**Limitação clara:**
- ❌ Não há renovação automática
- ❌ Gestão mais manual e sujeita a falhas

---

## 🔹 Terminação TLS no ALB

**Fluxo resumido:**

1. Cliente inicia conexão HTTPS
2. ALB apresenta o certificado
3. Canal criptografado é estabelecido
4. ALB descriptografa a requisição
5. Requisição segue para o target group

**Isso significa que:**
- ✅ Targets não precisam lidar com TLS
- ✅ Comunicação interna pode ser HTTP ou HTTPS (decisão arquitetural)

---

## 🔹 Casos de Uso para HTTPS no ALB

**Sites públicos com HTTPS obrigatório:**
- Compliance e segurança
- Melhor ranking em mecanismos de busca
- Confiança do usuário

**APIs REST seguras:**
- Proteção de dados sensíveis
- Autenticação e autorização
- Integração com sistemas externos

**Centralização de certificados:**
- Gerenciamento simplificado
- Renovação automática (com ACM)
- Redução de complexidade operacional

**Simplificação de segurança em microserviços:**
- TLS termination centralizado
- Targets não precisam gerenciar certificados
- Redução de overhead computacional

---

## ❓ Perguntas Frequentes sobre Certificados

**O ALB suporta HTTPS sem certificado?**
**Resposta:** ❌ Não. HTTPS exige obrigatoriamente um certificado de servidor.

**TLS termina no target?**
**Resposta:** ❌ Não, por padrão termina no ALB. O tráfego interno pode ser HTTP ou HTTPS, dependendo da configuração.

**Posso usar certificado auto-assinado?**
**Resposta:** ⚠️ Tecnicamente sim, mas não é recomendado para produção. Certificados auto-assinados geram avisos de segurança nos navegadores.

**ACM funciona em todas as regiões?**
**Resposta:** ✅ Sim, ACM está disponível em todas as regiões AWS.

---

## ⚠️ Erros Comuns com Certificados

**❌ Usar IAM quando ACM está disponível**
**Problema:** Complexidade desnecessária, sem renovação automática
**Solução:** Sempre preferir ACM quando disponível

**❌ Esquecer renovação de certificados fora do ACM**
**Problema:** Downtime evitável quando certificado expira
**Solução:** Usar ACM para renovação automática ou implementar alertas para certificados gerenciados manualmente

**❌ Configurar política de segurança TLS muito restritiva**
**Problema:** Incompatibilidade com clientes legados
**Solução:** Usar políticas TLS recomendadas pela AWS ou testar compatibilidade

**❌ Não configurar redirecionamento HTTP → HTTPS**
**Problema:** Tráfego não criptografado ainda acessível
**Solução:** Configurar listener HTTP que redireciona para HTTPS

---

## 🔗 Integrações com Certificados

**AWS Certificate Manager (ACM):**
- Integração nativa com ALB
- Renovação automática
- Validação via DNS ou email

**Route 53:**
- Validação DNS automática para certificados ACM
- Integração para domínios gerenciados

**CloudWatch:**
- Métricas de conexões TLS
- Alertas para falhas de handshake

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [AWS Certificate Manager Overview](https://docs.aws.amazon.com/acm/)
- [HTTPS Listeners for ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)

### Artigos Recomendados
- TLS Termination Patterns in AWS
- Best Practices for SSL/TLS on AWS

---

## ✅ Checklist de Prática

- [x] Entendi o papel do HTTPS no ALB
- [x] Sei quando usar ACM ou IAM para certificados
- [ ] Configurar listener HTTPS no ALB
- [ ] Testar terminação TLS
- [ ] Avaliar política de segurança TLS
- [ ] Configurar redirecionamento HTTP → HTTPS
- [ ] Criar certificado no ACM
- [ ] Validar certificado via DNS
- [ ] Configurar renovação automática

---

## 📊 Resumo

O uso de **certificados SSL/TLS no Application Load Balancer** é o mecanismo central de segurança para tráfego HTTPS, permitindo:
- ✅ Criptografia end-to-end
- ✅ Confiança e autenticação
- ✅ Simplificação operacional por meio da terminação TLS
- ✅ Renovação automática com ACM
- ✅ Centralização de gerenciamento de certificados

**Próximos Passos:**
- Ver [Application Load Balancer (ALB)](./alb.md) para configuração completa
- Integrar com Route 53 para validação DNS automática
- Configurar políticas de segurança TLS adequadas

---

**Última atualização:** 📅 09/01/2026


