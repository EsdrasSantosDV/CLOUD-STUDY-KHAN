# 📡 AWS IoT Core e AWS IoT Greengrass — Resumo Técnico

> Plataforma gerenciada da AWS para **conectividade IoT em nuvem (IoT Core)** e **computação na borda (Greengrass)**, formando um ecossistema completo para dispositivos inteligentes.

---

## 📌 Visão Geral

**AWS IoT Core** e **AWS IoT Greengrass** são serviços complementares para soluções de **Internet das Coisas (IoT)**:

- **IoT Core** → Conectividade segura em nuvem, registro de dispositivos e roteamento de mensagens.
- **IoT Greengrass** → Execução de aplicações IoT na borda (**edge computing**), com capacidade de operar mesmo sem conexão constante com a nuvem.

Usados juntos, formam uma plataforma IoT **completa, escalável e robusta**.

---

## 🌐 AWS IoT Core — Conectividade e Mensageria

### 🎯 O que é o AWS IoT Core

**AWS IoT Core** é uma plataforma totalmente gerenciada que permite **conectar dispositivos IoT diretamente à AWS**, sem gerenciar infraestrutura própria.

Exemplos de dispositivos:

- Sensores
- Wearables
- Eletrodomésticos inteligentes
- Máquinas industriais
- Dispositivos automotivos, etc.

---

### 🧩 Principais Recursos do IoT Core

#### ✔ Thing Registry (Registro de Dispositivos)

- Cadastro, identificação e rastreamento de dispositivos (**Things**)
- Armazena:
  - Identidade
  - Metadados
  - Atributos de cada dispositivo

---

#### ✔ Conectividade Segura em Larga Escala

- Suporte a **milhões de dispositivos** e **trilhões de mensagens**
- Latência **extremamente baixa**
- Conexões seguras com:
  - Certificados
  - Autenticação mútua
  - Criptografia em trânsito

---

#### ✔ Suporte a Múltiplos Protocolos

- **MQTT** (o mais comum em IoT)
- **HTTPS**
- **MQTT sobre WebSockets**

Permite que uma grande variedade de dispositivos se conecte com flexibilidade.

---

#### ✔ Rules Engine Integrado

Engine de regras que:

- **Filtra**, **transforma** e **roteia** mensagens para vários destinos, como:
  - **DynamoDB**
  - **Lambda**
  - **S3**
  - **SNS / SQS**
  - **Kinesis**
  - Serviços customizados

Permite:

- Processamento em tempo real dos dados recebidos
- Integração com outros serviços AWS

---

#### ✔ Comunicação Bidirecional

- Envia e recebe mensagens do dispositivo
- Permite que:
  - Dispositivos conversem com a nuvem
  - **Dispositivos conversem entre si** via IoT Core (publish/subscribe)

---

## 🧠 AWS IoT Greengrass — Computação na Borda

### 🎯 O que é o AWS IoT Greengrass

**AWS IoT Greengrass** é uma plataforma que permite **executar aplicações IoT na borda**, diretamente nos dispositivos ou gateways locais.

Ele traz parte das capacidades da nuvem **para perto do dispositivo**.

---

### 🧩 O que o Greengrass Fornece

- **Runtime open-source** para operações locais
- **Client software** para conectar dispositivos ao AWS IoT
- Capacidade de rodar:
  - **Componentes pré-prontos**
  - **Componentes customizados**
  - **Funções Lambda embarcadas**

Principais funcionalidades:

- **Processamento local de dados**
- Aplicações continuam funcionando **mesmo sem internet**
- Suporte nativo a **Pub/Sub offline** entre componentes
- **OTA (Over-The-Air Updates)**:
  - Atualizações seguras e automáticas de software
  - Gerenciamento de grandes frotas de dispositivos

---

## 🧮 Quando Usar Cada Um

### ✔ Use IoT Core quando

- Precisa **conectar dispositivos à nuvem**
- Deseja gerenciar:
  - Comunicação
  - Roteamento de mensagens
- Quer processar dados no backend usando o **Rules Engine**
- Os dispositivos possuem **conectividade razoável e frequente** com a internet

---

### ✔ Use IoT Greengrass quando

- Precisa de **computação na borda (edge)**
- Dispositivos **nem sempre estão conectados**
- Necessita de:
  - **Streams locais**
  - **Pub/Sub offline** entre componentes
  - Execução de **lógica IoT local**
  - **Gerenciamento de frotas** com atualizações OTA
- Quer:
  - Reduzir uso de banda
  - Reduzir latência para processamento crítico

---

## ✅ Resumo Final

- **IoT Core** → Conectividade, registro e roteamento de mensagens entre dispositivos e a AWS, com processamento via **Rules Engine** e múltiplos protocolos (MQTT, HTTPS, WebSockets).
- **IoT Greengrass** → Execução local de aplicações IoT, **Pub/Sub offline**, componentes embarcados e **atualizações OTA**.

Usados juntos, **IoT Core + Greengrass** formam um ecossistema IoT:

- Completo
- Escalável
- Seguro
- Adequado tanto para:
  - Dispositivos sempre conectados
  - Ambientes com conectividade intermitente

---

## ✅ Checklist de Aprendizado

- [ ] Sei o que é o **AWS IoT Core** e seus principais recursos (Registry, Rules Engine, protocolos)
- [ ] Sei o que é o **AWS IoT Greengrass** e seu foco em **edge computing**
- [ ] Entendi quando usar apenas IoT Core e quando combinar com Greengrass
- [ ] Sei que juntos eles formam uma solução IoT **cloud + edge** completa

---

## 🏷️ Tags

`#aws` `#iot` `#iot-core` `#greengrass` `#edge-computing` `#mqtt`

---

**Última atualização**: 📅 [DD/MM/YYYY]


