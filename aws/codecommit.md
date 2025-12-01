# 📁 AWS CodeCommit — Resumo Técnico

> Serviço totalmente gerenciado de controle de versão baseado em Git, hospedado na AWS, sem necessidade de manter servidores próprios.

---

## 📌 Resumo

**AWS CodeCommit** é um serviço de **versionamento de código baseado em Git**, totalmente gerenciado pela AWS.  
Ele elimina a necessidade de:

- Manter servidores Git próprios
- Cuidar de alta disponibilidade
- Gerenciar patches, backups e segurança de infraestrutura

É comumente o **ponto inicial** de pipelines **CI/CD** usando:

- **CodePipeline**
- **CodeBuild**
- **CodeDeploy**

---

## 🎯 Principais Benefícios

### ✔ Totalmente Serverless

- Nenhum servidor para:
  - Instalar
  - Escalar
  - Atualizar
- A AWS gerencia:
  - Disponibilidade
  - Escalabilidade
  - Patches de segurança

---

### ✔ Integração Profunda com AWS

Integra-se diretamente com:

- **CodePipeline**
- **CodeBuild**
- **CodeDeploy**
- **CloudWatch**
- **CloudTrail**
- **IAM**
- **Lambda** (via triggers de eventos do repositório)

Permite:

- Disparar pipelines automaticamente em `git push`
- Criar notificações e automações ao mudar o código

---

### ✔ Compatível com Git

Suporta os mesmos comandos e conceitos que qualquer repositório Git:

- `git clone`, `git pull`, `git push`, `git commit`
- Repositórios, branches, commits, pull requests (PRs)

Ou seja, você usa as **mesmas ferramentas Git** já conhecidas (CLI, VS Code, IDEs, etc.).

---

## 🗂 Criação de Repositórios

Para criar um repositório no CodeCommit, basta:

- Informar um **nome** (e opcionalmente uma descrição)

Após criado, você pode conectar ao repositório via:

- **HTTPS**
- **SSH**
- **HTTPS (GRC)** usando `git-remote-codecommit`

---

## 🔐 Métodos de Conexão

### 1. HTTPS

Características:

- Usa **porta 443**
- Funciona na maioria dos ambientes corporativos (firewalls permissivos para HTTPS)

Requisitos:

- Política **IAM** com permissões para CodeCommit
- Criação de **Git Credentials**:
  - Username + password gerados pela AWS (específicos para CodeCommit)

Ideal para:

- Usuários tradicionais
- Ambientes corporativos com restrições de rede

---

### 2. SSH

Características:

- Usa **porta 22**
- Pode ser bloqueado em alguns firewalls
- Geralmente considerado mais sólido para conexões seguras

Requisitos:

- Par de chaves **SSH**
- Upload da **chave pública** no IAM
- Configuração de `~/.ssh/config` com:
  - Chave privada
  - **SSH Key ID** associado no IAM

Vantagens:

- Alta segurança
- Não depende de senhas (usa chaves assimétricas)

---

### 3. HTTPS (GRC) — `git-remote-codecommit`

Recomendado quando se usa:

- **Credenciais temporárias**
- **Usuários federados**
- **Identity Providers**:
  - ADFS
  - Cognito
  - AWS SSO / IAM Identity Center

Requisitos:

- **Python** instalado
- Permissões **IAM** adequadas
- Instalação do helper:

```bash
pip install git-remote-codecommit
```

Benefícios:

- Integra-se bem com fluxos de **credenciais temporárias**
- Facilita uso com autenticação federada

---

## 🛡 Segurança

O CodeCommit traz segurança nativa, incluindo:

- **Criptografia em trânsito**:
  - Via HTTPS
- **Criptografia em repouso**:
  - Usando **KMS**
- **Controle de acesso por IAM**:
  - Policies aplicadas a:
    - Usuários
    - Grupos
    - Roles

Permite políticas de acesso granulares por:

- Ações:
  - `git pull`, `git push`, `GetRepository`, etc.
- Repositórios específicos:
  - Ex.: permitir acesso apenas a **dois repositórios definidos**

Isso possibilita separar permissões por:

- Times
- Projetos
- Ambientes

---

## 💰 Preço

A cobrança é baseada no número de **usuários ativos por mês**.

Para cada **usuário ativo**, você recebe:

- **10 GB-mês** de armazenamento
- **2.000 requisições Git** por mês

Custos excedentes:

- **US$ 0,06** por **GB-mês** adicional
- **US$ 0,001** por **requisição Git** adicional

Na prática:

- Para a maioria dos usos normais, o custo tende a ser **baixo**.

---

## ✅ Resumo Final

O **AWS CodeCommit** oferece um serviço Git:

- Totalmente gerenciado e **serverless**
- **Seguro**, com criptografia automática e IAM
- Altamente **integrado ao ecossistema AWS**

Ele:

- Suporta todos os comandos Git tradicionais
- Elimina a necessidade de servidores Git próprios
- Fornece múltiplos métodos de conexão (HTTPS, SSH, GRC)
- Criptografa dados em trânsito e em repouso
- Integra-se com **CodePipeline, CodeBuild e CodeDeploy**
- Usa um modelo de preços simples baseado em **usuários ativos**

É ideal para equipes que desejam um **repositório Git nativo na AWS**, com **alta disponibilidade, segurança e integração CI/CD**.

---

## ✅ Checklist de Aprendizado

- [ ] Sei o que é o **AWS CodeCommit** e que ele é baseado em **Git**
- [ ] Entendi as diferenças entre conexões **HTTPS**, **SSH** e **HTTPS (GRC)**
- [ ] Sei que a segurança é baseada em **IAM + KMS + HTTPS**
- [ ] Sei como o modelo de preço funciona (por **usuário ativo**)
- [ ] Entendi como o CodeCommit se integra com **CodePipeline, CodeBuild e CodeDeploy**

---

## 🏷️ Tags

`#aws` `#codecommit` `#git` `#version-control` `#cicd` `#devops`

---

**Última atualização**: 📅 [DD/MM/YYYY]


