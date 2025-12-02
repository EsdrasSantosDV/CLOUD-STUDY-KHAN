# 📊 Amazon QuickSight — Visão Técnica Profunda

> Serviço de Business Intelligence serverless da AWS para visualização e análise de dados

---

## 📌 Resumo

**Amazon QuickSight** é, essencialmente, o mecanismo de Business Intelligence serverless da AWS.

Se eu pudesse resumir o serviço em uma frase, diria:

**QuickSight é a camada final do pipeline de dados: onde informação bruta se transforma em percepção de valor.**

Ele fecha o ciclo natural dos sistemas analíticos: coletar → preparar → visualizar → decidir.

---

## 🎯 O papel do QuickSight no ecossistema AWS

Usando uma analogia típica de Engenharia:

- ✅ **Athena** é o "motor SQL" que lê S3 como se fosse um banco
- ✅ **Glue** é o "oficineiro" que organiza, cataloga e transforma dados
- ✅ **QuickSight** é o "painel do carro" — a interface onde realmente enxergamos o que importa

**QuickSight aceita dados de:**

- ✅ Serviços AWS (S3, Athena, Redshift, RDS, IoT, etc.)
- ✅ Fontes externas (SaaS, bancos on-premises, arquivos CSV/Excel)
- ✅ Streams preparados por Glue ou aplicações customizadas

**O objetivo final sempre é o mesmo:**

Permitir que qualquer pessoa — técnica ou não — consiga interpretar dados com qualidade.

Inclusive exportando dashboards para PDF, e-mail ou impressão.

---

## 🔄 O fluxo conceitual do QuickSight

A AWS documenta esse fluxo, mas a explicação prática costuma faltar. Aqui vai a versão "de engenheiro com 40 anos de estrada":

### 1️⃣ Coleta

Você unifica tudo — de serviços AWS a arquivos externos.

QuickSight não quer ser um repositório, apenas um consumidor inteligente.

### 2️⃣ Preparação

Aqui acontece a limpeza básica, cálculos derivados, joins e filtros globais.

É uma etapa menos poderosa que Glue, mas prática o suficiente para análises diárias.

### 3️⃣ Modelagem visual

QuickSight identifica automaticamente o melhor gráfico com base no tipo de dados.

> 💡 **Minha opinião:** Isso ajuda iniciantes, mas analistas mais experientes preferem customizar.

### 4️⃣ Interatividade e insights

Aqui QuickSight mostra seu diferencial:

- ✅ Tooltips dinâmicos
- ✅ Filtros por controle
- ✅ Parâmetros
- ✅ Cálculos condicionais
- ✅ Natural language insights (SPICE + ML)

### 5️⃣ Publicação e compartilhamento

Dashboards podem ser publicados para usuários do mesmo tenant AWS.

Ou enviados por PDF/e-mail.

---

## 🛠️ O fluxo prático do QuickSight — Os 5 passos

### Passo 1 — Criar uma nova análise

Classicão.

Um "canvas" limpo esperando datasets.

---

### Passo 2 — Adicionar datasets

Você pode usar:

- ✅ **Datasets SPICE** (importados para memória in-memory, extremamente rápidos)
- ✅ **Datasets direct query** (live, sem carregamento para SPICE)

> 💡 **Minha opinião:**
> 
> SPICE é quase sempre a melhor opção. Estabilidade, velocidade e previsibilidade.

---

### Passo 3 — Criando a primeira visualização — filtros e escolha automática

Ao adicionar campos, QuickSight tenta inferir:

- ✅ Tipo de gráfico adequado
- ✅ Eixo x/y
- ✅ Medidas vs. dimensões

Em engenharia de dados, isso é útil para análises rápidas, mas sempre revise manualmente.

**Os filtros aplicados no exemplo:**

- ✅ Unblended Cost
- ✅ Line Item Usage Account ID
- ✅ Line Item Product Code

Isso é essencialmente um drill-down clássico de FinOps:

**Conta → Serviço → Custo**

Essa é, inclusive, a espinha dorsal do relatório AWS Cost & Usage Report (CUR).

---

### Passo 4 — Ajustando e refinando a visualização

Renomear eixos, formatar labels e ajustar o tema não é apenas estética.

É parte do que chamamos de:

**Data Humanization** — facilitar a leitura reduz o erro interpretativo.

Um gráfico bem formatado vale mais que dez slides de explicação.

---

### Passo 5 — Criando uma segunda visualização — análise de uso

Você adicionou:

- ✅ Line Item Usage Amount
- ✅ Start Date

Isso cria um gráfico temporal clássico de uso vs. tempo.

Muito usado em:

- ✅ FinOps
- ✅ SRE
- ✅ Capacity planning

---

## 🧮 Criando campos calculados — Purchase Option

Esse é o ponto onde QuickSight vira uma ferramenta analítica de verdade.

**O cálculo:**

A lógica do campo Purchase Option usa IFs para categorizar instâncias como:

- ✅ Spot
- ✅ Reserved
- ✅ On-demand
- ✅ Other

E aqui vai a nuance técnica:

O CUR da AWS é altamente granulado, e esse tipo de cálculo é essencial para relatórios de FinOps maduros.

> 💡 **Minha opinião:**
> 
> Esse é o tipo de cálculo que deveria sempre ser abstraído em datasets SPICE prontos.
> 
> Não deixe que cada analista replique esse cálculo manualmente — padronize na camada de dataset.

---

## 🧹 Limpando o gráfico — removendo valores irrelevantes

Você aplicou filtros customizados para excluir:

- ✅ "Other"
- ✅ Linhas vazias

Excelente prática.

**Dashboard limpo é dashboard útil.**

---

## 🧩 Componentes Principais

### SPICE (Super-fast, Parallel, In-memory Calculation Engine)

- ✅ Motor de cálculo in-memory do QuickSight
- ✅ Extremamente rápido para análises interativas
- ✅ Suporta até 25 GB de dados por dataset (Enterprise)
- ✅ Atualização automática ou sob demanda

### Datasets

- ✅ **SPICE datasets** - Dados importados na memória
- ✅ **Direct query datasets** - Consulta direta à fonte
- ✅ Preparação e transformação de dados integrada

### Visualizações

- ✅ Gráficos automáticos baseados em tipo de dados
- ✅ Customização completa de visualizações
- ✅ Múltiplos tipos de gráficos disponíveis

### Dashboards

- ✅ Compilação de múltiplas visualizações
- ✅ Interatividade e drill-down
- ✅ Compartilhamento e publicação
- ✅ Exportação para PDF/e-mail

### Insights com ML

- ✅ Natural language queries
- ✅ Auto Insights (detecção automática de anomalias)
- ✅ Previsões e forecasting

---

## 💡 Quando Usar Amazon QuickSight

### ✅ É Indicado Quando:

- 📊 Você precisa de visualização e BI na AWS
- 🔗 Quer integrar com serviços AWS (S3, Athena, Redshift, RDS)
- 💰 Precisa de solução serverless com custos previsíveis
- 👥 Quer permitir que usuários não-técnicos analisem dados
- 🚀 Precisa de escalabilidade automática
- 📈 Quer insights com ML integrados

### ⚠️ Quando NÃO Usar:

- ❌ Precisa de recursos avançados de ETL → Use Glue primeiro
- ❌ Análises muito complexas que exigem ferramentas especializadas
- ❌ Precisa de conectores específicos não suportados pelo QuickSight

---

## 🔗 Integração com Ecossistema AWS

O QuickSight integra-se profundamente com:

- ✅ **S3** - Data lakes e arquivos
- ✅ **Athena** - Consultas SQL sobre dados no S3
- ✅ **Redshift** - Data warehouse
- ✅ **RDS** - Bancos relacionais
- ✅ **Glue** - Dados preparados e catalogados
- ✅ **IoT** - Dados de dispositivos
- ✅ **Kinesis** - Streaming de dados

---

## 💰 Custos

- **Autor**: $5/mês por usuário
- **Reader**: $5/mês por usuário (visualização apenas)
- **Enterprise**: $10/mês por usuário (recursos avançados)
- **SPICE**: Incluído (até limite por dataset)
- **Sem custos**: Setup, infraestrutura ou manutenção

> 💡 **Dica**: Use SPICE para datasets frequentemente acessados para melhor performance e economia

---

## 🔗 Recursos Adicionais

- [Documentação Oficial Amazon QuickSight](https://docs.aws.amazon.com/quicksight/)
- [Amazon QuickSight - Página do Produto](https://aws.amazon.com/quicksight/)
- [QuickSight Best Practices](https://docs.aws.amazon.com/quicksight/latest/user/best-practices.html)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o papel do QuickSight no ecossistema AWS
- [ ] Compreender o fluxo conceitual (coleta → preparação → modelagem → insights → publicação)
- [ ] Saber criar análises e dashboards
- [ ] Entender diferença entre datasets SPICE e direct query
- [ ] Criar visualizações e ajustar formatação
- [ ] Criar campos calculados e transformações
- [ ] Aplicar filtros e limpeza de dados
- [ ] Entender integração com serviços AWS
- [ ] Publicar e compartilhar dashboards
- [ ] Usar insights com ML e natural language queries

---

## 🏷️ Tags

`#aws` `#quicksight` `#business-intelligence` `#bi` `#data-visualization` `#analytics` `#dashboards` `#serverless` `#spice` `#finops`

---

## 🔚 Conclusão

Amazon QuickSight é, na prática:

- ✅ **Serverless**
- ✅ **Autoescalável**
- ✅ **Integrado a toda a malha de dados AWS**
- ✅ **Muito mais acessível que ferramentas tradicionais** (Tableau, PowerBI)

E cumpre seu papel magistralmente: **transformar dados em decisões.**

---

**Última atualização**: 📅 [DD/MM/YYYY]

