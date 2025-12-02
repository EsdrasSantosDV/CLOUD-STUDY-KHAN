# 🔧 AWS Glue — ETL Serverless

> Serviço totalmente gerenciado para extração, transformação e carga (ETL) de dados na AWS

---

## 📌 Resumo

**AWS Glue** é um serviço totalmente gerenciado e serverless para ETL, que facilita:

- ✅ Categorizar dados
- ✅ Limpar e enriquecer datasets
- ✅ Criar pipelines ETL escaláveis
- ✅ Mover dados entre fontes e destinos com confiabilidade
- ✅ Construir um catálogo unificado de metadados (Data Catalog)

Ele fornece uma UI intuitiva semelhante a ferramentas ETL tradicionais, mas com todas as vantagens de uma arquitetura serverless: sem servidores, baixo custo, escalabilidade automática e profunda integração com os serviços AWS.

---

## 🎯 O que é ETL?

**ETL (Extract, Transform, Load)** é o processo clássico utilizado para:

- ✅ **Extrair** dados de várias fontes (bancos, sistemas legados, SaaS, Salesforce, etc.)
- ✅ **Transformar** esses dados (limpeza, padronização, enriquecimento, conversão, conformidade)
- ✅ **Carregar** os dados para um destino (banco, data warehouse, data lake)

Esse fluxo é essencial para consolidar dados para BI, analytics e tomada de decisão.

---

## 🌐 O Contexto Moderno: Big Data & Cloud

Com o surgimento da nuvem e SaaS:

- ✅ Mais fontes de dados
- ✅ Maior volume de dados
- ✅ Maior velocidade (streaming, near real-time)
- ✅ Mais variedade (semiestruturado, logs, IoT, JSON, Parquet, etc.)

Isso exigiu ferramentas de integração de dados mais avançadas, capazes de lidar com:

- ✅ Escalabilidade
- ✅ Flexibilidade
- ✅ Custos menores
- ✅ Conectividade nativa com serviços cloud

Surgem então as soluções **ETL serverless**, nas quais você escreve código sem precisar gerenciar a infraestrutura que o executa.

---

## 🔄 ETL Tradicional vs ETL Serverless (AWS Glue)

A palestra destaca que o trabalho de ETL não é o valor final — **o valor está na análise dos dados depois de preparados**.

O serverless melhora o processo ao:

- ✅ Remover a necessidade de provisionar servidores
- ✅ Reduzir custo e manutenção
- ✅ Acelerar pipelines
- ✅ Permitir maior uso de dados em tempo real
- ✅ Facilitar integrações com fontes/destinos na nuvem

### 🔽 Comparação Simplificada

| Aspecto | ETL Tradicional | ETL Serverless (AWS Glue) |
|---------|----------------|---------------------------|
| **Infraestrutura** | Servidores on-premises gerenciados manualmente | Totalmente gerenciado pela AWS |
| **Escalabilidade** | Limitada; exige compra de hardware | Escala automática |
| **Custo** | Alto (licenciamento + hardware + manutenção) | Paga apenas pelo que usa |
| **Conexão com fontes cloud** | Pode exigir conectores adicionais | Nativo |
| **Desenvolvimento** | Ferramentas complexas, scripts manuais | Auto-geração de código, UI e Glue Studio |
| **Tempo para iniciar** | Demorado | Minutos |
| **Modelo** | ETL clássico | ETL e ELT modernos, com Glue + Athena/Redshift |

---

## 🧩 AWS Glue — O ETL Serverless da AWS

AWS Glue simplifica o processo com:

- ✅ **Interface visual (Glue Studio)** para montar pipelines ETL
- ✅ **Auto-geração de código serverless** (Spark / Python)
- ✅ **Catálogo de dados (Glue Data Catalog)**
- ✅ **Conexão nativa** com S3, Redshift, RDS, DynamoDB, Kinesis, etc.
- ✅ **Execução totalmente gerenciada** — sem servidores
- ✅ **Transformações, crawlers e job scheduling**

**No modelo serverless:**

- Dados continuam sendo extraídos, transformados e carregados
- Mas todo o processamento acontece na nuvem, sem gerenciamento de infraestrutura
- O Glue lida com escalabilidade, paralelismo e falhas

---

## 🔄 O Processo ETL Continua Igual

Mesmo usando Glue, a essência permanece:

- ✅ Perfilamento dos dados
- ✅ Extração de dados relevantes
- ✅ Tratamento de dados não estruturados / semiestruturados
- ✅ Transformações
- ✅ Carregamento em destinos como Amazon Redshift ou S3 (data lake)
- ✅ Preparação para consumo por ferramentas de BI

**O objetivo final segue o mesmo:**

Preparar dados com qualidade para permitir análises melhores e decisões mais precisas.

---

## 🧩 Componentes Principais do AWS Glue

### Glue Data Catalog

- ✅ Catálogo centralizado de metadados
- ✅ Armazena esquemas, tabelas e partições
- ✅ Usado por Athena, Redshift Spectrum e EMR
- ✅ Integração com Hive Metastore

#### Principais funções do Data Catalog

O Data Catalog é criado e populado quando você executa crawlers. Ele funciona como um:

- ✅ **Repositório persistente de metadados**
- ✅ Contém tabelas, schemas, versões de schema, jobs, conexões
- ✅ É usado por Athena, Redshift Spectrum, Glue ETL Jobs e outros serviços

**Principais funções:**

- ✅ Armazenar definições de tabelas e schemas inferidos
- ✅ Manter histórico de versões de schema
- ✅ Fornecer metadados para ETL jobs
- ✅ Servir como catálogo centralizado para toda a plataforma de dados AWS

O Glue atualiza automaticamente metadados conforme:

- ✅ Novas colunas aparecem
- ✅ Schemas mudam
- ✅ Partições são criadas

### Glue Crawlers

- ✅ Descobrem automaticamente esquemas de dados
- ✅ Escaneiam fontes de dados (S3, RDS, DynamoDB, etc.)
- ✅ Populam o Data Catalog automaticamente
- ✅ Suportam particionamento automático

#### O Papel do Crawler — Primeiro Passo no Glue

O **Glue Crawler** é responsável por:

- ✅ Conectar-se a data stores (S3, JDBC, etc.)
- ✅ Ler arquivos e descobrir automaticamente seu schema
- ✅ Criar ou atualizar tabelas no Glue Data Catalog
- ✅ Detectar mudanças de schema (ex.: novas colunas)
- ✅ Atualizar metadados para uso posterior em ETL jobs, Athena, Redshift Spectrum etc.

**Como o Crawler funciona:**

1. Analisa o conteúdo da origem usando **classifiers**
2. Determina o formato e o schema
3. Gera tabelas de metadados
4. Armazena tudo no Glue Data Catalog

> 💡 **Importante:** O crawler **NÃO descobre relações entre tabelas**, apenas estrutura e metadados.

**Formas de execução:**

- ✅ **Agendada** (cron, horários fixos)
- ✅ **On-demand** (manual)
- ✅ **Baseada em eventos** (ex.: novo arquivo S3 via EventBridge)

#### Criando um Crawler — Passo a Passo

Para criar um crawler no Glue:

1. **Nomeie o crawler**
2. **Escolha o data store** (S3, JDBC etc.)
3. **Informe o caminho** (com suporte a padrões como `s3://bucket/prefix/*.json`)
4. **Opcionalmente, adicione fontes adicionais**
5. **Defina o IAM Role** que permitirá acesso aos dados e ao Data Catalog
6. **Configure o schedule** (on-demand, eventos, cron)
7. **Defina a saída** — qual database armazenará as tabelas
8. **Ajuste opções avançadas**

### Glue ETL Jobs

- ✅ Scripts Python ou Scala para transformações
- ✅ Baseados em Apache Spark
- ✅ Execução serverless gerenciada
- ✅ Auto-scaling baseado na carga

#### Desenvolvimento

O Glue gera automaticamente código em:

- ✅ **Python (PySpark)**
- ✅ **Scala (Spark)**

Esse código corresponde às etapas de extract, transform e load definidas no wizard do Glue.

**Você pode:**

- ✅ Editar o script gerado
- ✅ Criar transformações customizadas
- ✅ Usar development endpoints para testar e depurar via IDE local
- ✅ Escrever leitores, writers e transforms personalizados

#### Agendamento e Execução

AWS Glue Jobs podem ser executados:

- ✅ **On-demand** (manual)
- ✅ **Agendados** (cron, intervalos, horários)
- ✅ **Disparados por eventos** (S3 upload, EventBridge, SNS, etc.)

Também é possível:

- ✅ Rodar múltiplos jobs em paralelo
- ✅ Criar dependências entre jobs
- ✅ Construir pipelines ETL mais complexos

**Logs:**

Todos os logs e alertas são enviados ao Amazon CloudWatch, permitindo:

- ✅ Monitoramento
- ✅ Alertas
- ✅ Troubleshooting

#### Como criar um ETL Job — Fluxo resumido

1. **Escolha a fonte de dados**
   - Deve existir como tabela no Data Catalog
   - Se necessário, inclui a connection JDBC/S3/etc.

2. **Defina o destino dos dados**
   - Catálogo existente
   - Ou criação dinâmica das tabelas-alvo

3. **Personalize parâmetros do job**
   - Configurações de execução
   - DPUs
   - Libraries
   - Arguments

4. **Ajuste o script gerado automaticamente**
   - Adicione transforms
   - Inclua múltiplas fontes ou destinos

5. **Escolha como o job será disparado**
   - Manual
   - Agenda
   - Trigger de evento

No fim, o Glue gera o script PySpark/Scala pronto para execução e customização.

### Glue Studio

- ✅ Interface visual para criar jobs ETL
- ✅ Geração automática de código
- ✅ Visualização de transformações
- ✅ Teste e debug integrados

### Glue Workflows

- ✅ Orquestração de múltiplos jobs
- ✅ Dependências entre jobs
- ✅ Agendamento e triggers
- ✅ Monitoramento centralizado

### Classifiers — Como o Glue Identifica Formatos

Um **classifier** lê o conteúdo da origem e retorna:

- ✅ O formato do arquivo (por exemplo: JSON)
- ✅ O schema inferido

**Tipos de classifiers suportados:**

- ✅ **JSON**
- ✅ **CSV**
  - Delimitadores suportados: vírgula, pipe (|), tab, ponto e vírgula
- ✅ **Web logs**
- ✅ **Classificadores de bancos de dados**
- ✅ **Custom classifiers** (para formatos proprietários ou lógicas específicas)

**Por que usar classificadores customizados?**

- ✅ Quando o formato não segue padrões conhecidos
- ✅ Quando é necessário criar schemas mais precisos
- ✅ Quando a tabela padrão gerada pelo Glue não atende os requisitos analíticos

**Inclusão / Exclusão de padrões:**

Você pode:

- ✅ Excluir arquivos `.csv`
- ✅ Excluir pastas específicas do S3
- ✅ Usar expressões regulares para controle fino

Isso evita crawlers lendo arquivos indevidos e aceleram seu ETL.

### Connections — Acessando Data Stores

Uma **Connection** define as propriedades para conectar o Glue a bancos de dados via JDBC.

Ela é usada por:

- ✅ Crawlers
- ✅ ETL Jobs
- ✅ Visualizações do Glue Studio

**Bancos compatíveis via JDBC:**

- ✅ **Amazon Redshift**
- ✅ **Amazon RDS:**
  - PostgreSQL
  - MySQL
  - MariaDB
  - Oracle
  - SQL Server
  - Aurora (todas as variantes)

O tipo de conexão define como o Crawler irá inferir o schema e gerar metadados.

---

## 💡 Quando Usar AWS Glue

### ✅ É Indicado Quando:

- 🔄 Você precisa fazer ETL de dados na AWS
- 📊 Trabalha com data lakes no S3
- 🗄️ Precisa catalogar e descobrir esquemas automaticamente
- 💰 Quer modelo de custo pay-per-use
- 🚀 Precisa de escalabilidade automática
- 🔗 Quer integrar múltiplas fontes de dados AWS

### ⚠️ Quando NÃO Usar:

Apesar dos benefícios, há situações onde o Glue pode não ser a melhor escolha:

- ❌ **Pipelines ETL já existentes e maduros on-premises**
  - Ferramentas tradicionais podem estar profundamente integradas à operação.

- ❌ **Dependência forte em linguagens que o Glue não suporta**
  - Glue suporta apenas Python e Scala — empresas com forte investimento em Java podem sentir limitação.

- ❌ **Conectividade limitada**
  - Glue ainda não possui out-of-the-box connectors para certos sistemas (ex.: Salesforce, SAP), ao contrário de ferramentas ETL mais antigas.

- ❌ **Esquemas que mudam com muita frequência**
  - Mudanças podem exigir edição manual de jobs e scripts.

- ❌ **Glue ainda é um produto relativamente jovem**
  - Nem sempre atende todos os cenários empresariais que ferramentas ETL tradicionais suportam.

- ❌ **Transformações muito simples** → Considere scripts Lambda
- ❌ **Processamento em tempo real crítico** → Considere Kinesis Analytics
- ❌ **Precisa de controle total sobre infraestrutura** → Considere EMR

## 🔐 Permissões (IAM)

Para acessar fontes e destinos, o Job precisa de:

- ✅ **IAM Role** com permissões de leitura/escrita nos data stores
- ✅ Permissões de uso do Data Catalog
- ✅ Chaves de acesso, caso necessário

**Boas práticas:**

- ✅ Nunca usar o root user
- ✅ Garantir governança para dados sensíveis
- ✅ Conceder acesso mínimo necessário (least privilege)

---

## 🎁 Benefícios do Glue em relação ao ETL tradicional

### 🟦 Serverless

- ✅ Sem servidores para gerenciar
- ✅ Custos apenas enquanto jobs e crawlers estão rodando

### 🟦 Crawlers automáticos

- ✅ Descobrem schemas
- ✅ Detectam mudanças
- ✅ Atualizam tabelas automaticamente
- ✅ Podem disparar jobs ETL

### 🟦 Auto-Geração de Código

- ✅ Scripts PySpark/Scala criados automaticamente
- ✅ Fácil extensão com código customizado

### 🟦 Integração nativa

- ✅ S3, Athena, Redshift, RDS, DynamoDB, Lake Formation
- ✅ Glue Studio para pipelines visuais
- ✅ Development endpoints para integração com IDEs

---

## 🔗 Integração com Ecossistema AWS

O Glue integra-se profundamente com:

- ✅ **S3** - Data lakes e armazenamento
- ✅ **Athena** - Consultas SQL sobre dados catalogados
- ✅ **Redshift** - Data warehouse
- ✅ **RDS** - Bancos relacionais
- ✅ **DynamoDB** - NoSQL
- ✅ **Kinesis** - Streaming de dados
- ✅ **EMR** - Big Data processing
- ✅ **QuickSight** - Visualização e BI

---

## 📊 Exemplo de Fluxo ETL com Glue

```
1. Crawler descobre dados no S3
   ↓
2. Popula Glue Data Catalog
   ↓
3. Glue ETL Job transforma dados
   ↓
4. Carrega dados transformados no destino
   ↓
5. Athena/Redshift consulta dados preparados
   ↓
6. QuickSight visualiza resultados
```

---

## 💰 Preços do AWS Glue

### ✔ Cobranças

- ✅ **Crawlers** → por segundo
- ✅ **Jobs ETL** → por segundo
- ✅ **Data Catalog** → mensal por objetos/metadados
- ✅ **Development endpoints** → por hora (DPU)

### ✔ Custos aproximados

- ✅ **1 DPU = 4 vCPUs + 16 GB RAM**
- ✅ Preço nos EUA (N. Virginia): ~ **$0.44 por DPU-hora**

### ✔ Grátis

- ✅ Primeiros **1 milhão de objetos** no Data Catalog
- ✅ Primeiros **1 milhão de acessos mensais** ao catálogo

> 💡 **Dica**: Use particionamento e otimize jobs para reduzir tempo de execução e custos

---

## 🔗 Recursos Adicionais

- [Documentação Oficial AWS Glue](https://docs.aws.amazon.com/glue/)
- [AWS Glue - Página do Produto](https://aws.amazon.com/glue/)
- [Glue Studio - Interface Visual](https://docs.aws.amazon.com/glue/latest/ug/glue-studio.html)
- [Best Practices do Glue](https://docs.aws.amazon.com/glue/latest/dg/best-practices.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito de ETL (Extract, Transform, Load)
- [ ] Compreender diferenças entre ETL tradicional e serverless
- [ ] Conhecer os componentes principais do Glue
- [ ] Entender Glue Data Catalog e sua importância
- [ ] Saber usar Crawlers para descobrir esquemas
- [ ] Entender como Crawlers funcionam e suas formas de execução
- [ ] Saber criar um Crawler passo a passo
- [ ] Compreender Classifiers e quando usar custom classifiers
- [ ] Entender Connections para acesso a bancos via JDBC
- [ ] Criar jobs ETL usando Glue Studio
- [ ] Entender integração com outros serviços AWS
- [ ] Saber otimizar jobs para reduzir custos
- [ ] Entender workflows e orquestração

---

## 🏷️ Tags

`#aws` `#glue` `#etl` `#data-integration` `#data-lake` `#big-data` `#serverless` `#spark` `#data-catalog` `#crawlers` `#classifiers` `#connections`

---

## ✅ Resumo Final

O AWS Glue fornece um ambiente ETL totalmente gerenciado e serverless que inclui:

- ✅ **Crawler** para descobrir schemas
- ✅ **Data Catalog** persistente
- ✅ **Jobs ETL** em PySpark/Scala
- ✅ **Agendamentos, triggers e pipelines complexos**
- ✅ **Monitoramento via CloudWatch**
- ✅ **Conexões JDBC** e integração com todo o ecossistema AWS

Além disso, o Glue reduz custos, simplifica pipelines e acelera a preparação de dados.

**Mas, como toda tecnologia, não é perfeito:**

- ⚠️ Suporta apenas duas linguagens (Python e Scala)
- ⚠️ Possui menos conectores que ferramentas ETL maduras
- ⚠️ Pode não substituir pipelines tradicionais já consolidados

**O ecossistema completo inclui:**

- ✅ **Crawlers** → Descobrem e catalogam metadados
- ✅ **Classifiers** → Interpretam formatos e schemas
- ✅ **Connections** → Conectam o Glue a bancos via JDBC
- ✅ **Data Catalog** → Centraliza metadados
- ✅ **Jobs ETL serverless** → Movem e transformam dados em escala

Ele reduz drasticamente o esforço manual e permite focar no que realmente importa:

**Preparar dados de forma rápida, padronizada e integrada para análise e tomada de decisão.**

---

**Última atualização**: 📅 [DD/MM/YYYY]

