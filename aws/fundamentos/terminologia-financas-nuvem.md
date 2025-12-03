# 💰 Terminologia de Finanças Aplicada à Nuvem — Explicado de Forma Técnica e Clara

> Conceitos financeiros essenciais para entender custos e investimentos na nuvem

---

## 📌 Resumo

Este documento cobre a terminologia financeira aplicada à computação em nuvem, essencial para profissionais que precisam entender custos, investimentos e análises financeiras relacionadas a serviços cloud.

---

## 📊 Amortização

É o processo de distribuir o custo de um pagamento ao longo do tempo, seguindo um cronograma que reflete o benefício daquele investimento a cada período.

**Em cloud, por exemplo:**

Um pagamento upfront de Reserved Instance pode ser amortizado ao longo dos 1 ou 3 anos de duração do contrato.

---

## 📈 Variable Costs (Custos Variáveis)

São custos que crescem ou diminuem conforme o volume de negócio.

**Exemplo:**

Se você hospeda sites e precisa de mais servidores à medida que novos clientes chegam, esse custo extra é variável.

---

## 💳 Upfront Charges (Cobranças Antecipadas)

Em RIs e Savings Plans, você pode pagar:

- ✅ **All Upfront (AURI)** → 100% antecipado
- ✅ **Partial Upfront (PURI)** → parte antecipada + parcelas
- ✅ **No Upfront (NURI)** → zero antecipado, só parcelas

**Contabilmente:**

O valor pago antecipado pode ser tratado como despesa antecipada (prepaid expense) e depois amortizado ao longo da vida da RI.

---

## 💼 Opex (Operating Expenditure)

São gastos operacionais consumidos dentro do próprio período — sem gerar ativo duradouro.

**Exemplo:**

Serviços sob demanda na nuvem (ex: EC2 On-Demand) são Opex.

Não envolvem depreciação nem amortização: o custo é reconhecido direto no resultado do período.

---

## 📊 ROI (Return on Investment)

Retorno percentual obtido sobre um investimento.

**Exemplo:**

Num caso de rightsizing, o ROI pode ser:

**Economia obtida – custo de engenharia para realizar o ajuste.**

---

## 📋 Income Statement (Demonstração de Resultados / P&L)

Mostra lucro ou prejuízo em um período (mês, trimestre, ano).

**Exemplo:**

Amortizações de RIs aparecem como despesas dentro desse período.

No ano 2 de uma RI de 3 anos, a amortização correspondente aparece no P&L daquele ano.

---

## 💵 NPV (Net Present Value)

Avalia se um projeto é financeiramente vantajoso, trazendo todos os fluxos de caixa futuros (custos e receitas) ao valor presente usando uma taxa de desconto.

**Exemplo clássico:**

Comparar o valor presente de uma RI All Upfront vs o custo projetado de um RI No-Upfront parcelado.

---

## 📉 Depreciação

Parecido com amortização, mas aplicado a ativos físicos (servidores, equipamentos).

Reduz o valor contábil ao longo do tempo, refletindo desgaste ou perda de utilidade.

---

## 💰 EBITDA

**Earnings Before Interest, Taxes, Depreciation and Amortization.**

É o lucro antes dessas subtrações.

**Exemplo de impacto:**

- ✅ Pagar RI All Upfront e amortizar → afeta EBITDA de forma diferente
- ✅ Usar On-Demand (puro Opex) → reduz EBITDA diretamente no período

---

## 🏦 Capitalization (Capitalização)

É quando um gasto pode ser tratado como ativo, e não despesa imediata.

Esse ativo será depreciado ou amortizado.

**Exemplo:**

Algumas empresas capitalizam certos investimentos de longo prazo (como RIs), dependendo da política contábil.

---

## 📊 Cost Allocation

É o processo fundamental do FinOps: distribuir custos corretamente entre centros de custo, aplicações, equipes ou produtos.

**Inclui:**

- ✅ Custos diretos
- ✅ Amortizações
- ✅ Custos compartilhados

Uma boa alocação permite saber exatamente quanto custa rodar cada workload.

---

## 📐 Unit Economics

Mede custo por unidade de valor gerado.

**Exemplo:**

Se seu ambiente custa 5 milhões/mês e suporta 10 milhões de páginas pagas, seu Unit Metric é:

**Webpages per $ = 2**

Próximas otimizações passam a ser medidas por esse indicador, permitindo decisões mais precisas sobre custo x benefício.

---

## 🔒 Fixed Cost (Custo Fixo)

Não muda com o volume de negócio.

**Exemplo:**

O financiamento de um prédio do datacenter custa igual independentemente de quantos servidores ele abriga.

---

## 📑 Balance Sheet (Balanço Patrimonial)

Foto da situação financeira da empresa em um dia específico.

Mostra ativos, passivos e valores residuais de itens ainda não depreciados ou amortizados.

**Exemplo:**

Comprar uma RI de 3 anos → no final do ano 1, parte dela ainda aparece como ativo no balanço.

---

## 🏗️ Capex (Capital Expenditure)

Gastos que viram ativos — ex: prédio, servidores, switches, storage físico.

São depreciados ou amortizados ao longo do tempo.

**Importante:**

Rodar VM na nuvem não é Capex: é Opex.

---

## 📊 Comparação: Capex vs Opex

| Aspecto | Capex | Opex |
|--------|-------|------|
| **Natureza** | Investimento em ativos | Gastos operacionais |
| **Exemplo** | Comprar servidor físico | Usar EC2 On-Demand |
| **Contabilização** | Depreciação/Amortização | Despesa direta no período |
| **Impacto no EBITDA** | Não afeta diretamente | Reduz diretamente |
| **Flexibilidade** | Baixa (ativo fixo) | Alta (paga conforme usa) |

---

## 🔗 Recursos Adicionais

- [AWS Cost Management](https://aws.amazon.com/aws-cost-management/)
- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
- [FinOps Foundation](https://www.finops.org/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender conceito de amortização e como se aplica a RIs
- [ ] Diferenciar custos variáveis de custos fixos
- [ ] Compreender formas de pagamento upfront (AURI, PURI, NURI)
- [ ] Entender diferença entre Opex e Capex
- [ ] Compreender ROI e como calcular em contextos de cloud
- [ ] Entender Income Statement (P&L) e impacto de amortizações
- [ ] Compreender NPV e análises de viabilidade financeira
- [ ] Diferenciar depreciação de amortização
- [ ] Entender impacto no EBITDA de diferentes modelos de pagamento
- [ ] Compreender capitalização de investimentos
- [ ] Entender Cost Allocation e FinOps
- [ ] Compreender Unit Economics e métricas de eficiência
- [ ] Entender Balance Sheet e tratamento contábil de RIs

---

## 🏷️ Tags

`#aws` `#fundamentos` `#financas` `#finops` `#custo` `#capex` `#opex` `#roi` `#ebitda` `#amortizacao` `#depreciacao` `#npv` `#cost-allocation` `#unit-economics`

---

## ✅ Resumo Final

Você acabou de passar pela parte mais densa da terminologia financeira aplicada à nuvem.

**Agora já domina:**

- ✅ Amortização, depreciação, ROI, EBITDA
- ✅ Capex versus Opex
- ✅ NPV e análises de viabilidade
- ✅ Unit economics
- ✅ Custo fixo, custo variável
- ✅ Alocação de custos
- ✅ Relação entre contabilidade e modelos de consumo cloud

---

**Última atualização**: 📅 [DD/MM/YYYY]

