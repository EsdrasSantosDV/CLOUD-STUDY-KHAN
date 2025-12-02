# 🔍 Amazon Athena — Visão Geral

> Serviço totalmente gerenciado e serverless para executar consultas SQL interativas sobre dados no S3

---

## 📌 Resumo

**Amazon Athena** é um serviço totalmente gerenciado e serverless que permite executar consultas SQL interativas diretamente sobre dados armazenados no Amazon S3 e em outras fontes de dados, sem necessidade de provisionar infraestrutura ou gerenciar clusters.

É agora um serviço oficialmente incluído no escopo da **AWS Certified Developer – Associate**, tornando-se essencial para quem trabalha com análise de dados em AWS.

---

## 🎯 Principais Características do Athena

### 1️⃣ Serverless

- ✅ Não há servidores para gerenciar
- ✅ Não é necessário configurar clusters Hadoop, Presto ou Spark
- ✅ Você paga apenas pelas consultas executadas

---

### 2️⃣ Consulta dados em S3 e outras fontes

Athena permite consultar:

- ✅ **Data Lakes no S3**
- ✅ **Data warehouses**
- ✅ **Repositórios de Big Data**
- ✅ **Fontes conectadas via federated queries** (ex.: RDS, DynamoDB)

---

### 3️⃣ SQL padrão

- ✅ Usa SQL tradicional
- ✅ Ideal para quem já conhece bancos relacionais
- ✅ Baixa curva de aprendizado

---

### 4️⃣ DDL para definição de tabelas

Athena não move seus dados — você cria tabelas que referenciam arquivos no S3.

**Suporta formatos como:**

- ✅ **CSV**
- ✅ **JSON**
- ✅ **Parquet** (altamente recomendado)
- ✅ **ORC**
- ✅ **Avro**

---

### 5️⃣ Particionamento

Você pode definir colunas como partition keys, permitindo:

- ✅ Consultas muito mais rápidas
- ✅ Redução de custos, pois o Athena escaneia apenas os arquivos das partições relevantes

**Exemplo típico:** Particionar por ano / mês / dia.

---

### 6️⃣ Integrações Importantes

Athena se integra facilmente com:

- ✅ **EMR & Glue**
  - Catalogação automática de dados
  - ETL e preparação de dados para data lakes
- ✅ **QuickSight**
  - Dashboards e visualizações interativas
- ✅ **Redshift Spectrum**
  - Consultar dados externos em S3 usando motores complementares

---

### 7️⃣ Suporte a dados comprimidos

Athena consegue ler arquivos em S3 nos formatos:

- ✅ **GZIP, Snappy, Zstd** etc.

**Benefícios:**

- ✅ Reduzem custo de armazenamento
- ✅ Aceleram consultas quando combinados com Parquet ou ORC

---

### 8️⃣ Acesso via

- ✅ **AWS Console**
- ✅ **SDKs**
- ✅ **Athena API**
- ✅ **ODBC / JDBC drivers** (útil para ferramentas BI)

---

## 💡 Quando Usar Amazon Athena

### ✅ É Indicado Quando:

- 📊 Você precisa analisar dados armazenados no S3
- 🔍 Quer executar consultas SQL sem gerenciar infraestrutura
- 💰 Precisa de modelo de custo pay-per-query
- 🗄️ Trabalha com data lakes e big data
- 📈 Precisa integrar com ferramentas BI existentes
- 🔄 Quer consultar dados de múltiplas fontes via federated queries

### ⚠️ Quando NÃO Usar:

- ❌ Precisa de transações ACID → Use **RDS** ou **DynamoDB**
- ❌ Consultas muito frequentes em pequenos datasets → Considere **RDS** ou **Redshift**
- ❌ Precisa de atualizações em tempo real → Use **DynamoDB** ou **RDS**

---

## 🧩 Componentes Principais

### Data Catalog

- ✅ Armazena metadados sobre tabelas e partições
- ✅ Integra com AWS Glue Data Catalog
- ✅ Permite compartilhamento de esquemas entre serviços

### Query Engine

- ✅ Baseado em Presto/Trino
- ✅ Executa consultas SQL padrão
- ✅ Otimiza automaticamente consultas

### Result Storage

- ✅ Resultados salvos automaticamente no S3
- ✅ Configurável para bucket específico
- ✅ Histórico de consultas disponível

---

## 📊 Exemplo de Uso

```sql
-- Criar tabela externa apontando para dados no S3
CREATE EXTERNAL TABLE sales_data (
    id int,
    product string,
    amount double,
    date string
)
PARTITIONED BY (year int, month int)
STORED AS PARQUET
LOCATION 's3://my-bucket/sales/';

-- Consultar dados particionados
SELECT product, SUM(amount) as total
FROM sales_data
WHERE year = 2024 AND month = 1
GROUP BY product;
```

---

## 💰 Custos

- **Cobrança:** Por TB de dados escaneados por consulta
- **Otimização:** Use particionamento e formatos como Parquet/ORC para reduzir custos
- **Sem custos:** Infraestrutura, setup ou manutenção

> 💡 **Dica**: Use particionamento e formatos colunares (Parquet/ORC) para reduzir drasticamente os custos de escaneamento

---

## 🔗 Recursos Adicionais

- [Documentação Oficial Amazon Athena](https://docs.aws.amazon.com/athena/)
- [Amazon Athena - Página do Produto](https://aws.amazon.com/athena/)
- [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html)
- [Best Practices do Athena](https://docs.aws.amazon.com/athena/latest/ug/best-practices.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o conceito de serviço serverless de consulta SQL
- [ ] Compreender como Athena consulta dados no S3 sem mover dados
- [ ] Saber criar tabelas externas com DDL
- [ ] Entender particionamento e seus benefícios
- [ ] Conhecer formatos de dados suportados (CSV, JSON, Parquet, ORC, Avro)
- [ ] Entender integração com Glue Data Catalog
- [ ] Saber otimizar consultas para reduzir custos
- [ ] Conhecer federated queries para outras fontes
- [ ] Entender integração com QuickSight e ferramentas BI

---

## 🏷️ Tags

`#aws` `#athena` `#sql` `#data-lake` `#big-data` `#analytics` `#s3` `#serverless` `#glue` `#quicksight`

---

**Última atualização**: 📅 [DD/MM/YYYY]

