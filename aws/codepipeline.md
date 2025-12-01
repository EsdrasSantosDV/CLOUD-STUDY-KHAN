# 🔄 AWS CodePipeline — Resumo Técnico

> Serviço gerenciado de **orquestração CI/CD** que automatiza todo o fluxo de entrega de software, do commit ao deploy.

---

## 📌 Resumo

**AWS CodePipeline** é o serviço responsável por **orquestrar e automatizar** todo o fluxo de **CI/CD** na AWS.  
Ele conecta e coordena:

- **Source** (CodeCommit, GitHub, Bitbucket, S3, imagens Docker, etc.)
- **Build** (CodeBuild ou ferramentas externas)
- **Test** (opcional)
- **Deploy** (CodeDeploy, CloudFormation, ECS, Lambda, entre outros)

Sempre que ocorre uma **mudança na fonte configurada**, o pipeline executa todas as etapas automaticamente, seguindo a ordem dos **stages** definidos.

---

## 🧱 Funcionamento Geral do Pipeline

Um **pipeline** é composto por:

- **Stages** (estágios)
- Cada stage contém uma ou mais **actions** (ações)

Exemplo típico de pipeline:

1. **Source**
   - Detecta mudança no repositório
2. **Build**
   - Executa CodeBuild, gera imagem Docker e/ou artefatos
3. **Deploy**
   - Usa CodeDeploy, CloudFormation, ECS ou Lambda para atualizar o ambiente

Você pode adicionar:

- **Testes automatizados**
- Vários ambientes (ex.: `dev → stage → prod`)
- **Aprovações manuais** entre estágios

---

## 🔗 Integração com Outras Ferramentas AWS

### ✔ EventBridge

- Responsável por detectar **mudanças na fonte** (ex.: push no CodeCommit)
- Dispara a execução do pipeline automaticamente.

---

### ✔ CodeCommit / GitHub / Bitbucket / S3

Podem servir como **fonte (Source)** do pipeline:

- Repositórios Git (CodeCommit, GitHub, Bitbucket)
- Arquivos versionados em **S3**
- Imagens Docker em **ECR**

---

### ✔ CodeBuild

Stage de **build** e testes:

- Compila a aplicação
- Executa testes
- Empacota artefatos
- Cria imagens Docker
- Gera **BuildArtifacts** para os próximos stages

---

### ✔ CodeDeploy

Stage de **deploy**:

- Realiza deploy usando estratégias como:
  - **Blue/Green**
  - **Canary**
  - **Linear**
  - **AllAtOnce**
- Suporta:
  - Substituição de tasks em **ECS**
  - **Traffic shifting** em **Lambda**
  - Deploy em **EC2 / on-premises**

---

## 🔁 Fluxo Real de Execução (Exemplo)

Exemplo baseado em pipeline com **CodeCommit + CodeBuild + ECS (Blue/Green)**:

1. **Commit no CodeCommit**
   - O código muda → **EventBridge** dispara o pipeline.

2. **Stage Source**
   - Pega a **branch configurada**
   - Gera **SourceArtifacts**

3. **Stage Build (CodeBuild)**
   - `SourceArtifacts` entram como input
   - CodeBuild gera:
     - **Docker image**
     - `imageDetail.json`
     - `taskdef.json`
     - `appspec.yaml`
   - Output: **BuildArtifacts**

4. **Stage Deploy (ECS Blue/Green)**
   - Consome **BuildArtifacts** (não os SourceArtifacts)
   - CodeDeploy injeta dinamicamente:
     - `IMAGE1_NAME` ← valor de `imageDetail.json`
   - Cria **nova task definition**
   - Sobe novo serviço ECS / nova versão
   - Faz **traffic shifting** para a nova tarefa
   - Pode aguardar **período de análise** (mínimo ~5 minutos)
   - Após validação (ou clique em “Terminate original”):
     - Ambiente antigo é encerrado
     - CodeDeploy marca o deploy como **succeeded**
   - Pipeline finaliza com sucesso

---

## 📦 Orquestração de Artefatos

Cada stage define:

- **Input artifacts** (artefatos de entrada)
- **Output artifacts** (artefatos de saída)

Fluxo típico:

- **Source** → produz `SourceArtifacts`
- **Build**:
  - Consome `SourceArtifacts`
  - Gera `BuildArtifacts`
- **Deploy**:
  - Consome `BuildArtifacts`

Benefícios:

- Passagem automática de artefatos entre stages
- Elimina necessidade de:
  - `git checkout` manual
  - Scripts para mover arquivos
  - Manipulação manual de pacotes

---

## ⚙️ Configurações Adicionais

### ✔ Aprovações Manuais

- Permitem que **deploys sejam liberados manualmente** entre ambientes:
  - Ex.: aprovação antes de ir de `stage` para `prod`

---

### ✔ Variáveis de Ambiente

Podem ser enviadas, por exemplo, para o **CodeBuild**:

- Hardcoded no pipeline
- Vindo do **Parameter Store (SSM)**
- Vindo do **Secrets Manager**

Permite configurar:

- Secrets
- Endpoints
- Feature flags de build

---

### ✔ Múltiplos Métodos de Source

Além de CodeCommit, é possível usar:

- **GitHub / GitHub Enterprise**
- **Bitbucket**
- **Imagens Docker em ECR**
- **Arquivos em S3**
- Artifact stores diversos

Isso torna o CodePipeline flexível para diferentes estratégias de versionamento.

---

## 🧩 Uso de CodeDeploy com ECS (Blue/Green)

Quando usado com **ECS Blue/Green**:

- CodeDeploy cria a **replacement task** (nova versão)
- ECS faz o **rolling out** do novo container
- O **load balancer** move o tráfego progressivamente para a nova versão
- Após verificação:
  - O ambiente antigo é **encerrado**
  - O deploy é marcado como **bem-sucedido**

---

## 👀 Visualização e Logs

Durante cada etapa:

- **CodeBuild**:
  - Mostra logs linha a linha da execução (compilação, testes, etc.)
- **CodeDeploy**:
  - Exibe cada **hook do `appspec`** e o status de execução
- **ECS**:
  - Mostra novas tasks iniciando e parando
- **Load Balancer**:
  - Mostra redirecionamento de tráfego entre versões

Isso garante **observabilidade** detalhada em cada parte do pipeline.

---

## ✅ Resumo Final

O **AWS CodePipeline** é o serviço que **une tudo no CI/CD AWS**:

- Monitora o código
- Dispara builds
- Gera artefatos
- Orquestra deploys seguros
- Integra profundamente com:
  - **CodeCommit**
  - **CodeBuild**
  - **CodeDeploy**
  - **ECS**
  - **Lambda**
  - **S3**

É a **espinha dorsal da automação** na AWS, permitindo:

- **Zero intervenção manual** (quando desejado)
- **Deploys replicáveis e auditáveis**
- Fluxos **multiambiente**
- Estratégias avançadas de release
- **Segurança e rastreabilidade total**

---

## ✅ Checklist de Aprendizado

- [ ] Sei o que é o **AWS CodePipeline** e seu papel na orquestração CI/CD
- [ ] Entendi o conceito de **stages** e **actions**
- [ ] Sei como ele integra com **CodeCommit, CodeBuild, CodeDeploy, ECS e Lambda**
- [ ] Entendi a diferença entre **SourceArtifacts** e **BuildArtifacts**
- [ ] Sei que é possível usar **aprovações manuais** e múltiplas fontes (GitHub, S3, ECR, etc.)

---

## 🏷️ Tags

`#aws` `#codepipeline` `#cicd` `#devops` `#automation` `#orchestration`

---

**Última atualização**: 📅 [DD/MM/YYYY]


