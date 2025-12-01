# 🧱 AWS CodeBuild — Resumo Técnico

> Serviço totalmente gerenciado de **build contínuo** para compilar código, executar testes e gerar artefatos prontos para deploy, sem gerenciar servidores de build.

---

## 📌 Resumo

**AWS CodeBuild** é um serviço de **CI (Continuous Integration)** que:

- Compila seu código-fonte
- Executa testes (ex.: unitários)
- Gera artefatos prontos para deploy

Tudo isso sem que você precise:

- Provisionar servidores
- Escalar workers de build
- Gerenciar infraestrutura de compilação

É amplamente usado em **pipelines CI/CD**, frequentemente integrado a:

- **CodeCommit**
- **CodePipeline**
- **CodeDeploy**
- **GitHub / Bitbucket**

---

## 🧱 Conceitos Fundamentais

### ✔ 1. CodeBuild Project

É o elemento central da configuração do CodeBuild.  
Um **Project** define:

- **Origem do código**:
  - S3
  - CodeCommit
  - GitHub / GitHub Enterprise
  - Bitbucket
- **Ambiente de build**:
  - Imagem Docker que será usada para compilar/testar
- **Buildspec**:
  - Arquivo ou comandos que definem as fases de build
- **Output artifacts**:
  - Onde os artefatos gerados serão armazenados (ex.: S3)

---

### ✔ 2. Build Environment

É o ambiente onde o build é executado, baseado em **contêineres Docker**.

Opções principais:

- **Imagens gerenciadas pela AWS**, incluindo:
  - Amazon Linux 2
  - Ubuntu
  - Windows Server Core 2019 (em regiões selecionadas)
- Suporte a vários runtimes:
  - Java
  - .NET Core
  - Ruby
  - Python
  - Go
  - Node.js
  - Android
- **Imagens customizadas**, por exemplo:
  - ARM
  - Linux custom
  - GPU
  - Windows Server 2019
  - Armazenadas no **ECR** ou **Docker Hub**

O contêiner de build contém tudo o que é necessário para:

- Compilar o código
- Executar testes
- Empacotar artefatos

---

### ✔ 3. Buildspec File

O **buildspec** é um arquivo **YAML** que descreve:

- As **fases do build**
- Os **comandos** de cada fase
- Os **artefatos** gerados
- As **variáveis de ambiente**, relatórios, etc.

Fases comuns:

- `install`
- `pre_build`
- `build`
- `post_build`

Benefícios:

- **Versionável** (fica no repositório Git)
- **Reprodutível** (mesma definição em qualquer ambiente)
- **Padronizado** (mesmo padrão entre projetos)
- Fácil de **evoluir e revisar** em code review

Exemplo de caso:

- Rodar `mvn install` em um projeto Java
- Gerar `messageUtil-1.0.jar`
- Enviar o JAR para um bucket S3

---

### ✔ 4. Build Commands

Há duas formas principais de definir comandos de build:

1. **Comandos inseridos diretamente no Console**
   - Simples
   - Útil para POCs ou builds pequenos
2. **Arquivo buildspec YAML (recomendado)**
   - Versão controlada via Git
   - Suporta pipelines complexos
   - Permite manipular múltiplos artefatos, relatórios, etc.

Na prática, o **buildspec.yaml** é o padrão preferido para ambientes profissionais.

---

### ✔ 5. Output Artifacts

Após o build, o CodeBuild pode:

#### ✔ Upload para S3

- Enviar artefatos (ZIPs, JARs, pacotes, bundles) para **Amazon S3**
- Usar S3 como:
  - **Repositório oficial de artefatos**
  - Fonte para etapas posteriores (CodeDeploy, Lambda, etc.)

Isso permite:

- Criar **triggers** por eventos de S3
- Executar **Lambdas** automaticamente após upload
- Integrar com outras ferramentas que leem de S3

#### ✔ Outputs alternativos via buildspec

Exemplo comum:

- Construir uma **imagem Docker**
- Fazer `docker push` para **ECR**

Ou seja, você não fica limitado ao S3, desde que o buildspec implemente os passos desejados.

---

## 📡 Monitoramento

O CodeBuild integra nativamente com:

- **CloudWatch Logs**
- **CloudWatch Events / EventBridge**

Isso permite:

- Acompanhar logs de execução em tempo real
- Criar **alertas** em caso de falha no build
- Executar **automação pós-build** (ex.: notificar via SNS/Slack, disparar outros workflows)

---

## ✅ Fluxo Geral de um Build com CodeBuild

Um pipeline típico com CodeBuild envolve:

1. **Criar o Project**
   - Definir **fonte do código**
   - Escolher **ambiente Docker** (imagem de build)
2. **Configurar o `buildspec.yaml`**
   - Fases (`install`, `pre_build`, `build`, `post_build`)
   - Comandos de build/teste
   - Artefatos e outputs
3. **Executar o build**
   - Manualmente, via CodePipeline ou trigger de repositório
4. **Monitorar logs**
   - Via CloudWatch Logs / console do CodeBuild
5. **Enviar artefatos**
   - Para **S3**, **ECR** ou outro destino via buildspec

---

## ✅ Resumo Final

O **AWS CodeBuild** oferece:

- **Build contínuo escalável**, sem gerenciar servidores
- Integração nativa com **CodeCommit, CodePipeline, CodeDeploy, GitHub, Bitbucket**
- Suporte a múltiplos runtimes e imagens customizadas
- Flexibilidade para gerar artefatos em **S3, ECR** e outros destinos

É uma peça fundamental em pipelines **CI/CD na AWS**, permitindo compilar, testar e empacotar aplicações de forma **automática, reprodutível e integrada** ao ecossistema AWS.

---

## ✅ Checklist de Aprendizado

- [ ] Sei o que é o **AWS CodeBuild** e seu papel em CI/CD
- [ ] Entendi o conceito de **Project**, **Build Environment** e **buildspec.yaml**
- [ ] Sei como o CodeBuild gera **artefatos** (S3, ECR, etc.)
- [ ] Sei que ele integra com **CloudWatch Logs** e **EventBridge** para monitoramento
- [ ] Consigo descrever o fluxo básico de um **build com CodeBuild**

---

## 🏷️ Tags

`#aws` `#codebuild` `#cicd` `#devops` `#build` `#automation`

---

**Última atualização**: 📅 [DD/MM/YYYY]


