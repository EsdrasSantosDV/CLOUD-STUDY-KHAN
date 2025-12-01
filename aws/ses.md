# 📧 AWS SES — Simple Email Service

> Serviço gerenciado de envio e recebimento de e-mails em escala, com baixo custo

---

## 📌 Resumo

**AWS SES (Simple Email Service)** é um serviço **gerenciado** da AWS para **envio e recebimento de e-mails** com alta entrega, escalabilidade e baixo custo.  
É muito usado para **sistemas automatizados**, como:

- Confirmações de conta
- Notificações de pedidos
- Recuperação de senha
- Newsletters e campanhas de marketing

---

## 🎯 O que é o SES

O **SES** é um serviço de e-mail **transacional e de marketing** que:

- ✅ Envia e-mails via **SMTP** ou **SDK da AWS**
- ✅ Recebe e-mails e permite **processamento automático**
- ✅ Garante **autenticação e reputação** por meio de **DKIM/SPF**
- ✅ Oferece **monitoramento de bounces e complaints**
- ✅ Escala automaticamente conforme o volume de envio

---

## 📤 Envio de E-mails

### Formas de Integração

- **SMTP**
  - Uso com qualquer app que suporte SMTP (aplicações legadas, servidores de e-mail, etc.)
- **SDK da AWS**
  - Integração direta em código (Node, Python, Java, etc.)
- **AWS CLI / APIs**
  - Útil para automações e scripts

### Casos de Uso Comuns

- Confirmações de conta
- Notificações de pedidos
- Recuperação de senha
- Campanhas de marketing

---

## 📥 Recebimento de E-mails

O SES também pode **receber e-mails** e disparar ações automáticas, por exemplo:

- Processar pedidos de **unsubscribe**
- Criar **tickets automaticamente**
- Registrar **feedbacks de clientes**
- Armazenar e-mails em **S3** para análise posterior
- Disparar **funções Lambda** para qualquer lógica customizada

---

## 🧪 Sandbox vs Produção

Ao criar a conta, você inicia no **SES Sandbox**, onde:

- Só pode enviar para **e-mails verificados**
- Existem **limites de envio** mais restritos

Para operar em **produção**, é necessário:

- Abrir um **ticket no AWS Support**
- Solicitar a **remoção do sandbox** para o SES na região desejada

---

## 🔐 Verificação de Identidade

O SES exige **verificação de identidade** para garantir autenticidade e evitar abuso.

### Verificação de E-mail

Passos:

1. Console SES → `Email Addresses`
2. Clicar em **“Verify a new email address”**
3. Confirmar via **link enviado** para o e-mail

### Verificação de Domínio

Permite enviar e receber e-mails de **qualquer endereço daquele domínio**.

Necessário configurar registros DNS:

- **DKIM** → Assinatura criptográfica para autenticação
- **SPF** → Informa quais servidores podem enviar e-mail pelo domínio
- **MAIL FROM** (opcional) → Domínio customizado para o envelope sender

---

## 📊 Monitoramento

Depois de sair do sandbox, é possível acompanhar:

- **Bounces** → E-mails que não foram entregues
- **Complaints** → Reclamações de spam
- **Estatísticas de envio**, como:
  - Taxa de entrega
  - Volume diário/mensal

Geralmente é integrado com:

- **CloudWatch Metrics**
- **CloudWatch Alarms**
- **SNS** para notificações de eventos

---

## 🛡️ Segurança no Recebimento

O SES aplica **filtros de segurança nativos**:

- Anti-spam
- Anti-vírus
- Bloqueio de fontes suspeitas

Isso ajuda a proteger aplicações que recebem e-mails diretamente via SES.

---

## ⚙️ Controle de Recebimento

Existem duas principais estratégias de controle:

### 1. Recipient-Based Control

Controle baseado no **destinatário** do e-mail.

**Componentes:**

- **Conditions**
  - Lista de destinatários (ex: `suporte@dominio.com`, `contato@dominio.com`)
- **Receipt Rules**
  - Conjunto de ações executadas quando a condição é atendida

**Ações disponíveis:**

- **S3 Action** → Salva o e-mail em um bucket S3
- **SNS Action** → Publica notificação em um tópico SNS
- **Lambda Action** → Dispara uma função Lambda
- **Bounce Action** → Rejeita o e-mail (gera bounce)
- **Stop Action** → Interrompe o processamento de regras subsequentes
- **Add Header** → Adiciona headers personalizados ao e-mail
- **WorkMail Action** → Encaminha para Amazon WorkMail

---

### 2. IP-Based Control

Filtra com base no **IP de origem** do e-mail:

- Listas de **allow** (permitidos)
- Listas de **block** (bloqueados)

> 🔐 Importante: por padrão, **e-mails vindos de instâncias EC2 são bloqueados**, para evitar abuso (SPAM).  
> Boas práticas: usar SES como serviço gerenciado de envio, não rodar MTA próprio em EC2.

---

## 🔄 Fluxo de Processamento de um E-mail Recebido

### 1. IP-Based Control

- Se o IP estiver **bloqueado** → e-mail é **descartado**
- Se o IP estiver **permitido** → segue para a próxima etapa

### 2. Recipient-Based Control

- SES procura uma **condição correspondente** ao destinatário:
  - Se **não encontrar** → e-mail é **descartado**
  - Se **encontrar** → executa as **ações configuradas** (S3, SNS, Lambda, etc.)

---

## 🔗 Integrações Comuns

- **Sistemas internos via SMTP**
  - ERPs, CRMs, aplicações legadas
- **AplicaçÕes modernas com SDKs da AWS**
  - Microserviços, backends serverless, etc.
- **Pipelines automatizados** usando:
  - **Lambda + S3 + SNS**
    - Recebe e-mail → salva no S3 → dispara Lambda → envia notificação via SNS

---

## 💰 Custos (Visão Geral)

- Pagamento conforme o uso:
  - Quantidade de e-mails enviados
  - Quantidade de e-mails recebidos
  - Dados transferidos
- Possibilidade de descontos com altos volumes

> 💡 **Dica**: Use o `AWS Pricing Calculator` para estimar custos.

---

## 📊 Exemplo de Fluxo de Processamento

```text
Cliente envia e-mail → SES recebe → 
  1. Verifica IP (IP-Based Control) → 
  2. Verifica destinatário (Recipient-Based Control) → 
  3. Executa ações (S3, SNS, Lambda, WorkMail, Bounce, etc.)
```

---

## ✅ Resumo Final

O **AWS SES** fornece uma plataforma **poderosa e econômica** para envio e recebimento de e-mails em escala, com:

- **Autenticação forte**: DKIM e SPF
- **Integração flexível**: SMTP, SDKs e APIs
- **Automação de regras**: Lambda, S3, SNS, Receipt Rules
- **Filtragem de segurança nativa**
- **Controles finos** por **destinatário** e **IP**

É ideal para aplicações que exigem **comunicação automática**, **alta confiabilidade** e **controle detalhado** de envio e recebimento de e-mails.

---

## 📝 Comandos Úteis

### Exemplo com AWS CLI (Envio Simples)

```bash
aws ses send-email \
  --from "no-reply@seu-dominio.com" \
  --destination "ToAddresses=email@destino.com" \
  --message "Subject={Data=Teste SES},Body={Text={Data=Corpo do e-mail de teste}}"
```

---

## 🔗 Recursos Adicionais

- Documentação Oficial: `https://docs.aws.amazon.com/ses/`
- Página do Produto: `https://aws.amazon.com/ses/`

---

## ✅ Checklist de Aprendizado

- [ ] Entendi a diferença entre **sandbox** e **produção**
- [ ] Sei como **verificar um e-mail** no SES
- [ ] Sei como **verificar um domínio** e configurar **DKIM/SPF**
- [ ] Sei os principais **casos de uso** de envio e recebimento
- [ ] Entendi o fluxo de **Recipient-Based Control** e **IP-Based Control**
- [ ] Sei como integrar com **Lambda, S3 e SNS**

---

## 🏷️ Tags

`#aws` `#ses` `#email` `#serverless` `#integrations`

---

**Última atualização**: 📅 [DD/MM/YYYY]


