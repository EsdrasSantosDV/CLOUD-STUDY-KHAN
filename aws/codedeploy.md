# 🚀 AWS CodeDeploy — Resumo Técnico

> Serviço gerenciado de **deploy automatizado** para aplicações em EC2, servidores on-premises, ECS e Lambda, reduzindo downtime e complexidade operacional.

---

## 📌 Resumo

**AWS CodeDeploy** é um serviço da AWS que automatiza o **deploy de aplicações** em diferentes plataformas:

- **EC2 / Servidores on-premises**
- **ECS**
- **AWS Lambda**

Ele facilita:

- Entrega contínua (CD)
- Deploys repetíveis e padronizados
- Redução de erros manuais
- Minimização de downtime

É normalmente utilizado como parte de um pipeline **CI/CD** junto com:

- **CodeCommit / GitHub**
- **CodeBuild**
- **CodePipeline**

---

## 🧱 Terminologia Essencial

### ✔ Application

Representa o **“container lógico”** do deploy no CodeDeploy.

Ao criar uma Application, você escolhe a **plataforma alvo**:

- `EC2/On-premises`
- `ECS`
- `Lambda`

Essa escolha define:

- Como os deployments serão configurados
- Quais tipos de Deployment Groups estarão disponíveis

---

### ✔ Deployment Group

Define **onde** o deploy será aplicado.

Para cada plataforma:

- **EC2 / On-premises**
  - Seleção de instâncias via:
    - **Tags**
    - **Grupos de tags**
    - **Auto Scaling Groups**
- **ECS**
  - Define:
    - **Cluster**
    - **Serviço**
    - **Load balancer / Target groups**
- **Lambda**
  - Configura:
    - **Função Lambda**
    - **Roteamento de tráfego** entre versões/aliases

---

### ✔ Deployment

É a **execução do deploy** em si.

Depende de:

- **Deployment Group**
- **Deployment Configuration**
- **Revision** (artefato + arquivo `appspec`)

Durante um Deployment, o CodeDeploy:

- Aplica a estratégia de rollout
- Executa hooks definidos no `appspec`
- Atualiza instâncias/containers/funções

---

## 🎛 Deployment Configuration

Define:

- **Velocidade**
- **Volume**
- **Estratégia** de rollout

### EC2 / On-Premises

Suporta:

- `OneAtATime`
- `HalfAtATime`
- `AllAtOnce`

E também:

- **In-place deployments**
- **Blue/Green deployments**

### ECS e Lambda

Suporta estratégias como:

- **Canary**
- **Linear**
- **AllAtOnce**

#### Exemplos:

- **AllAtOnce**
  - EC2/ECS: atualiza todos os servidores/containers de uma só vez
  - Lambda: redireciona **100% do tráfego** imediatamente para a nova versão
- **Canary**
  - Redireciona uma **pequena porcentagem** do tráfego para a nova versão
  - Se estiver estável → envia o restante
- **Linear**
  - Incrementos fixos (ex.: **10% a cada 10 minutos**)
  - Mais seguro que um canary simples em alguns cenários

Exemplo de preset:

- `CodeDeploy.LambdaCanary10Percent10Minutes`

---

## 🖥 CodeDeploy Agent (somente EC2/On-premises)

Para **EC2 / servidores on-premises**, o CodeDeploy precisa de um **agent** instalado:

- Instalação via:
  - **AWS CLI**
  - **AWS Systems Manager**
- Suporte a:
  - **Amazon Linux**
  - **Ubuntu**
  - **RHEL**
  - **Windows**
- Comunicação com CodeDeploy:
  - Via **HTTPS (porta 443)**

O agent é **obrigatório** para que o CodeDeploy:

- Copie arquivos
- Execute scripts/hooks
- Controle o ciclo do deploy na instância

> Não é necessário **agent** para:
> - **ECS**
> - **Lambda**

---

## 📦 Revision (Artefato de Deploy)

Cada Deployment usa uma **Revision**, que é um pacote contendo:

- Seu **código/artefato**
- Um arquivo **`appspec`**:
  - Formato **YAML** ou **JSON**

O `appspec` instrui o CodeDeploy sobre:

- **Onde** instalar arquivos
- **Como iniciar/parar serviços**
- **Hooks antes/depois** do deploy
- **Rotinas de validação** (health checks, testes, etc.)

Isso traz:

- Padronização do processo de deploy
- Possibilidade de rollback consistente

---

## 🔁 Fluxo Geral do CodeDeploy

Um fluxo típico de uso do CodeDeploy:

1. **Criar Application**
2. **Criar Deployment Group**
3. **Instalar CodeDeploy Agent** (se EC2/on-premises)
4. **Empacotar artefato + `appspec`**
5. **Criar Deployment**
6. CodeDeploy executa:
   - Hooks do `appspec`
   - Estratégia de rollout
   - Atualizações de instâncias/containers/funções
7. **Monitorar eventos** via:
   - CloudWatch
   - SNS

Esse fluxo muitas vezes é orquestrado automaticamente pelo **CodePipeline**.

---

## 📡 Monitoramento e Notificações

O CodeDeploy integra com:

### ✔ CloudWatch

- Publica:
  - Métricas
  - Eventos de deploy
- Permite:
  - Criar **alertas** para falhas
  - Acionar automações (por exemplo, via EventBridge)

---

### ✔ SNS

- Pode enviar notificações para:
  - Início de deploy
  - Sucesso
  - Falha
  - Rollback
- Permite:
  - Alertar times via e-mail, SMS ou integrações externas

---

## ✅ Resumo Final

O **AWS CodeDeploy** é uma peça fundamental em pipelines **CI/CD** para garantir **deploys confiáveis e automatizados** em:

- **EC2 / on-premises**
- **ECS**
- **Lambda**

Ele oferece:

- Deploy **seguro e eficiente**
- Estratégias avançadas:
  - **Canary**
  - **Linear**
  - **Blue/Green**
- Controle detalhado via **`appspec`**
- Monitoramento e rollback integrados
- Integração nativa com:
  - **CodePipeline**
  - **CloudWatch**
  - **SNS**

É ideal para equipes que buscam **entrega contínua robusta**, com **segurança** e **mínima indisponibilidade** em produção.

---

## ✅ Checklist de Aprendizado

- [ ] Sei o que é o **AWS CodeDeploy** e as plataformas suportadas (EC2, ECS, Lambda, on-premises)
- [ ] Entendi os conceitos de **Application**, **Deployment Group**, **Deployment** e **Deployment Configuration**
- [ ] Sei o papel do **CodeDeploy Agent** em EC2/on-premises
- [ ] Entendi o que é uma **Revision** e o arquivo **`appspec`**
- [ ] Sei como o CodeDeploy integra com **CloudWatch** e **SNS** para monitoramento e notificações

---

## 🏷️ Tags

`#aws` `#codedeploy` `#cicd` `#devops` `#deploy` `#bluegreen` `#canary`

---

**Última atualização**: 📅 [DD/MM/YYYY]


