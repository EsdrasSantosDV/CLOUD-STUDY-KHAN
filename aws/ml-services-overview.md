# 🤖 AWS Machine Learning Services — Visão Geral das Categorias

> Visão completa e organizada de todos os serviços de Machine Learning da AWS

---

## 📌 Resumo

A AWS oferece uma ampla gama de serviços de Machine Learning organizados em diferentes camadas, desde frameworks puros até serviços de aplicação prontos para uso. Esta visão geral organiza esses serviços por categoria e nível de complexidade.

---

## 1️⃣ Supported Frameworks and Interfaces

### O que são

Frameworks de ML "puros", voltados para quem já é praticante experiente e quer controle total do pipeline e da infra.

**Frameworks suportados:**

- ✅ **TensorFlow**
- ✅ **Apache MXNet**
- ✅ **PyTorch**

**Usados em:**

- ✅ Amazon SageMaker
- ✅ AWS Deep Learning AMIs
- ✅ AWS Deep Learning Containers

**Palavras-chave:**

- "Bring your own framework"
- "Full control over infra"
- "Deep Learning AMIs / Containers"

---

## 2️⃣ Platform Service Layer – Amazon SageMaker

### O que é

Serviço fully managed para:

- ✅ **Build** → **Train** → **Deploy** modelos de ML em escala

**Suporta:**

- ✅ Built-in algorithms
- ✅ Seus próprios modelos (BYO model)
- ✅ Frameworks como TensorFlow, MXNet, PyTorch

### Extensões importantes

- ✅ **SageMaker Canvas** – interface visual no-code para business analysts fazerem previsões
- ✅ **SageMaker Studio Lab** – ambiente gratuito para aprender/experimentar ML
- ✅ **Data Labeling** – rotulagem de imagens, textos, vídeos
- ✅ **Data Wrangler** – preparação de dados para ML (selecionar, limpar, transformar, visualizar)
- ✅ **Feature Store** – catálogo central de features compartilhadas entre times/modelos
- ✅ **Autopilot** – AutoML: você fornece dataset, ele gera, treina e escolhe o melhor modelo
- ✅ **JumpStart** – modelos pré-prontos + templates
- ✅ **SageMaker Edge** – inferência em dispositivos de borda
- ✅ **Pipelines** – CI/CD para workflows de ML
- ✅ **Clarify** – detecção de viés e explicabilidade

**Palavras-chave:**

- "Fully managed ML platform"
- "Build, train, and deploy in one service"
- "Supports TensorFlow, MXNet, PyTorch"
- "End-to-end ML"

**Casos de uso:**

- ✅ Qualquer solução onde você precisa treinar seu próprio modelo em produção
- ✅ Pipelines de ML corporativos com governança e MLOps

---

## 3️⃣ ML Services with Hardware Component

### 🔹 AWS DeepLens

**O que é:**

Câmera programável com deep learning embarcado para visão computacional.

**Palavras-chave:**

- "Deep learning-enabled video camera"
- "On-device inference"

**Casos de uso:**

- ✅ Classificar espécies/animais
- ✅ Detectar objetos (plantas, chapéus, produtos) em vídeo

---

### 🔹 AWS DeepRacer

**O que é:**

Mini carro autônomo físico + simulador, focado em reinforcement learning.

**Palavras-chave:**

- "Fully autonomous race car"
- "Reinforcement learning"
- "DeepRacer League"

**Casos de uso:**

- ✅ Aprender RL na prática
- ✅ Competir em corridas com modelos RL

---

### 🔹 AWS DeepComposer

**O que é:**

Teclado USB + serviço de ML para generative AI focado em música.

**Palavras-chave:**

- "Generative AI"
- "Musical keyboard"

**Casos de uso:**

- ✅ Compor músicas usando ML
- ✅ Aprender conceitos de geração de conteúdo

---

### 🔹 Amazon Monitron

**O que é:**

Sistema end-to-end de predictive maintenance com:

- ✅ Sensores físicos
- ✅ Gateway on-premises
- ✅ Serviço na AWS

**Palavras-chave:**

- "Predictive maintenance"
- "Physical sensors on equipment"

**Casos de uso:**

- ✅ Monitorar máquinas em fábricas
- ✅ Detectar falhas iminentes em equipamentos industriais

---

### 🔹 AWS Panorama

**O que é:**

Appliance de hardware que conecta a câmeras IP existentes e roda computer vision on-prem.

**Palavras-chave:**

- "Computer vision appliance"
- "Analyzes video streams from IP cameras"

**Casos de uso:**

- ✅ Monitorar segurança de trabalhadores
- ✅ Identificar gargalos em linhas de produção

---

## 4️⃣ Application Layer Services (alto nível, via API)

Esses são os favoritos de prova: serviços de ML prontos, acessados por API, pouca ou nenhuma experiência em ML exigida.

### 🔹 Amazon Rekognition

**O que é:**

Serviço de computer vision para imagens e vídeos.

**Palavras-chave:**

- "Image and video analysis"
- "Facial recognition"
- "Text in images (OCR)"
- "Custom Labels"

**Casos de uso:**

- ✅ Detectar objetos, pessoas, ações, texto em imagens/vídeos
- ✅ Verificar similaridade de faces, autenticação, moderação de conteúdo
- ✅ Usar Custom Labels para detectar seu próprio produto/marca em prateleiras

---

### 🔹 Amazon Transcribe

**O que é:**

Speech-to-text – converte fala em texto (live ou arquivos).

**Palavras-chave:**

- "Automatic Speech Recognition (ASR)"
- "Speech-to-text"
- "Call Analytics"
- "Transcribe Medical"

**Casos de uso:**

- ✅ Legendas de reuniões em tempo real
- ✅ Transcrição de áudios para busca
- ✅ Call centers: gerar transcrições e métricas de qualidade

---

### 🔹 Amazon Polly

**O que é:**

O inverso do Transcribe: text-to-speech com vozes naturais.

**Palavras-chave:**

- "Text-to-speech"
- "SSML (Speech Synthesis Markup Language)"

**Casos de uso:**

- ✅ Narração de e-learning, podcasts, notícias
- ✅ Leitura de artigos, posts, RSS

---

### 🔹 Amazon Translate

**O que é:**

Serviço de tradução automática multi-idioma.

**Palavras-chave:**

- "Neural machine translation"
- "Localization"

**Casos de uso:**

- ✅ Localizar sites e apps para múltiplos idiomas
- ✅ Traduzir grandes volumes de texto
- ✅ Customizar vocabulário com termos de marca/produto

---

### 🔹 Amazon Lex

**O que é:**

Serviço para criar chatbots conversacionais (voz e texto), com suporte a diálogos multi-turn.

**Palavras-chave:**

- "Conversational interfaces"
- "Chatbots"
- "Natural language understanding (NLU)"

**Casos de uso:**

- ✅ Assistentes virtuais
- ✅ Bots de FAQ
- ✅ Atendimento automatizado (muito usado com Amazon Connect)

---

### 🔹 Amazon Comprehend

**O que é:**

Serviço de NLP (Natural Language Processing) para extrair insights de texto.

**Palavras-chave:**

- "Sentiment analysis"
- "Entity recognition"
- "Key phrase extraction"
- "Language detection"
- "Comprehend Medical"

**Casos de uso:**

- ✅ Analisar sentimento de reviews
- ✅ Extrair entidades (pessoas, datas, orgs) de documentos
- ✅ Com Comprehend Medical: extrair dosagens, medicamentos, termos clínicos

---

### 🔹 Amazon Forecast

**O que é:**

Serviço de time series forecasting: previsão de demanda, vendas, recursos.

**Palavras-chave:**

- "Forecasting"
- "Time-series prediction"

**Casos de uso:**

- ✅ Prever estoque, demanda de produtos
- ✅ Previsão de visitas em parque temático, tráfego em loja, capacidade

---

### 🔹 Amazon Textract

**O que é:**

Serviço para extrair texto e estrutura de documentos (PDFs, imagens, formulários, tabelas).

**Palavras-chave:**

- "OCR + structure"
- "Forms and tables extraction"

**Casos de uso:**

- ✅ Extrair dados de formulários para sistemas internos
- ✅ Processar documentos digitalizados sem conhecer o layout exato

---

### 🔹 Amazon Kendra

**O que é:**

Serviço de busca inteligente com ML, estruturado para conteúdo corporativo.

**Palavras-chave:**

- "Intelligent enterprise search"
- "Natural language search"

**Casos de uso:**

- ✅ Busca unificada em documentos, wikis, SharePoint, S3 etc.
- ✅ FAQ inteligente em cima de base documental

---

### 🔹 Amazon Personalize

**O que é:**

Serviço de recomendação personalizada ao estilo Amazon.com.

**Palavras-chave:**

- "Personalized recommendations"
- "User segmentation"
- "Personalized ranking"

**Casos de uso:**

- ✅ Recomendar produtos, conteúdos, filmes, notícias
- ✅ Personalizar campanhas de marketing por usuário

---

## 5️⃣ Outros Serviços de ML (resumo rápido)

### Amazon Augmented AI (A2I)

Human-in-the-loop workflows para revisão humana de previsões de modelos (Rekognition, Textract, etc.).

---

### Amazon CodeGuru

Recomendações automáticas para review de código e otimização de performance (profiling).

---

### Amazon DevOps Guru

Detecta anomalies operacionais (potenciais outages, erros, degradação de serviço).

---

### Amazon Fraud Detector

Detecta fraude a partir de dados históricos (transações, login, etc.).

---

### Amazon Lookout for Equipment

Analisa dados de sensores para prever falhas em equipamentos.

---

### Amazon Lookout for Metrics

Detecta anomalias em métricas de negócio/operacionais (vendas, churn, KPIs).

---

### Amazon Lookout for Vision

Usa visão computacional para detectar defeitos em peças/produtos.

---

## 📊 Resumo por Nível de Complexidade

### 🟢 Nível 1: Serviços Prontos (Application Layer)

**Ideal para:** Iniciantes, casos de uso comuns

- Rekognition, Transcribe, Polly, Translate, Lex, Comprehend, Forecast, Textract, Kendra, Personalize

### 🟡 Nível 2: Plataforma Gerenciada (SageMaker)

**Ideal para:** Equipes que precisam treinar modelos customizados

- SageMaker e todas suas extensões

### 🔴 Nível 3: Frameworks e Hardware

**Ideal para:** Especialistas em ML que precisam controle total

- TensorFlow, PyTorch, MXNet, Deep Learning AMIs/Containers
- DeepLens, DeepRacer, DeepComposer, Monitron, Panorama

---

## 💡 Quando Usar Cada Categoria

### Use Application Layer Services quando:

- ✅ Precisa de resultados rápidos sem treinar modelos
- ✅ Casos de uso comuns (visão, voz, texto, tradução)
- ✅ Equipe sem experiência profunda em ML
- ✅ Quer focar no produto, não em ML

### Use SageMaker quando:

- ✅ Precisa treinar modelos customizados
- ✅ Tem dados específicos do seu domínio
- ✅ Precisa de controle sobre o pipeline completo
- ✅ Quer governança e MLOps

### Use Frameworks quando:

- ✅ Precisa de controle total sobre infraestrutura
- ✅ Tem requisitos muito específicos
- ✅ Equipe altamente especializada em ML
- ✅ Quer usar frameworks específicos sem abstrações

---

## 🔗 Recursos Adicionais

- [Documentação AWS Machine Learning](https://docs.aws.amazon.com/machine-learning/)
- [AWS Machine Learning - Página do Produto](https://aws.amazon.com/machine-learning/)
- [AWS Machine Learning Blog](https://aws.amazon.com/blogs/machine-learning/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender as diferentes categorias de serviços ML na AWS
- [ ] Diferenciar Application Layer vs Platform vs Frameworks
- [ ] Conhecer os principais serviços de Application Layer
- [ ] Entender quando usar SageMaker vs serviços prontos
- [ ] Conhecer serviços com componentes de hardware
- [ ] Entender Lookout e outros serviços especializados

---

## 🏷️ Tags

`#aws` `#machine-learning` `#ml` `#ai` `#sagemaker` `#rekognition` `#lex` `#comprehend` `#forecast` `#textract` `#kendra` `#personalize`

---

**Última atualização**: 📅 [DD/MM/YYYY]

