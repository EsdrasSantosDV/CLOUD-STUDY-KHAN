# 🖥️ Amazon AppStream 2.0 e Amazon WorkSpaces — Resumo Técnico

> Serviços gerenciados da AWS para entrega de **aplicações** e **desktops virtuais** de forma segura, escalável e remota.

---

## 📌 Visão Geral

**Amazon AppStream 2.0** e **Amazon WorkSpaces** são dois serviços complementares da AWS voltados para **virtualização e acesso remoto**:

- **AppStream 2.0** → Focado em **streaming de aplicações**.
- **WorkSpaces** → Focado em **desktops completos e persistentes (VDI)**.

---

## 🧩 Amazon AppStream 2.0

### 🎯 O que é o Amazon AppStream 2.0

**Amazon AppStream 2.0** é um serviço **totalmente gerenciado** para **streaming seguro de aplicações e desktops virtuais**.

Principais capacidades:

- Streaming de **aplicações Windows e Linux**:
  - Windows Server
  - Amazon Linux 2
- Possibilita transformar **aplicações legadas em SaaS**, sem alterar o código.

---

### 🛠 Principais Características — AppStream 2.0

- **Modos de acesso**:
  - Cliente nativo (**AppStream Client**)
  - **Navegador web**
- **Autenticação federada**:
  - Suporte a **SAML 2.0**
- Uso de **golden images** contendo:
  - Sistema Operacional
  - Aplicativos
  - Configurações padrão
- **Selective persistence**:
  - Mesmo recebendo um desktop novo a cada sessão:
    - Preferências e dados do usuário podem ser **preservados** (quando configurado)

---

### 📌 Quando usar AppStream 2.0?

Use **AppStream 2.0** quando:

- Você quer **streaming de aplicações isoladas**, especialmente aplicações desktop antigas/legadas.
- Deseja fornecer acesso remoto a **um software específico**, sem entregar um desktop inteiro.
- Precisa transformar **apps tradicionais em SaaS**, mantendo controle de execução na nuvem.

---

## 🧩 Amazon WorkSpaces

### 🎯 O que é o Amazon WorkSpaces

**Amazon WorkSpaces** é um serviço de **Virtual Desktop (VDI) gerenciado** para entrega de **desktops persistentes completos**.

Ele substitui **ambientes tradicionais de VDI** sem necessidade de gerenciar:

- Servidores
- Storage
- Brokers de conexão

---

### 🛠 Principais Características — WorkSpaces

- Provisiona **desktops persistentes** baseados em:
  - **Windows**
  - **Amazon Linux**
  - **Ubuntu Linux**
- Oferece:
  - Instalação automática de **patches e updates**
  - Possibilidade de incluir **software adicional** (ex.: Microsoft Office)
- Redução de custos com:
  - **BYOL (Bring Your Own License)** para Windows

---

### 📌 Quando usar WorkSpaces?

Use **WorkSpaces** quando:

- Você precisa entregar **desktops completos**, não apenas uma aplicação.
- Quer **substituir VDI tradicional** com uma solução totalmente gerenciada.
- Deseja substituir **PCs físicos** por **desktops virtuais persistentes** para uso diário.

---

## ⚖️ Comparação Direta

### 🧱 Amazon AppStream 2.0

- Focado em **streaming de aplicações individuais**
- Sessões **não persistentes**, mas com **configurações persistentes** quando desejado
- Ideal para:
  - **Conversão de apps antigas em SaaS**
  - Workloads que exigem apenas **uma aplicação**, e não um desktop completo

---

### 💻 Amazon WorkSpaces

- Focado em **desktops completos e persistentes**
- Usuário mantém um desktop **“dedicado”**
- Ideal para:
  - **Substituir máquinas físicas** ou VDI
  - **Trabalho diário** em ambiente corporativo completo

---

## ✅ Resumo Final

- Use **Amazon AppStream 2.0** quando você precisar **servir aplicativos** a usuários remotos com **segurança e escalabilidade**, sem entregar um desktop completo.
- Use **Amazon WorkSpaces** quando quiser fornecer um **desktop completo, remoto e persistente**, substituindo PCs tradicionais e soluções de **VDI**.

Juntos, esses serviços permitem construir soluções flexíveis de **trabalho remoto**, **laboratórios virtuais**, **acesso a aplicações legadas** e **ambientes corporativos centralizados**.

---

## ✅ Checklist de Aprendizado

- [ ] Sei a diferença entre **AppStream 2.0** e **WorkSpaces**
- [ ] Entendi que o AppStream 2.0 é focado em **aplicações**, e o WorkSpaces em **desktops completos**
- [ ] Sei quando usar AppStream 2.0 (apps legadas, SaaS, acesso isolado)
- [ ] Sei quando usar WorkSpaces (substituição de VDI e PCs físicos)

---

## 🏷️ Tags

`#aws` `#appstream` `#workspaces` `#vdi` `#saas` `#remote-work`

---

**Última atualização**: 📅 [DD/MM/YYYY]


