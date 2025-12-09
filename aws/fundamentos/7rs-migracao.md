# 🔄 Os 7 Rs da Migração para a Nuvem — Explicação Completa

> Framework estruturado para escolher a estratégia de migração adequada para cada workload

---

## 📌 Resumo

Os **7 Rs** foram criados para oferecer às organizações um framework testado e confiável, usado como guia de alto nível na hora de migrar ambientes de um data center on-premises para a nuvem pública — como a AWS.

Seja você responsável por migrar um data center inteiro ou apenas uma aplicação isolada, os 7 Rs ajudam a escolher a estratégia mais adequada de acordo com as necessidades do negócio.

---

## 🔍 Por que os 7 Rs são importantes?

Ao migrar qualquer workload, não basta mover o servidor e pronto.

**Você precisa considerar:**

- ✅ Dados dos clientes
- ✅ Como usuários vão acessar a aplicação após a migração
- ✅ Como garantir segurança e conformidade
- ✅ Como monitorar, atualizar e operar essa aplicação na nuvem
- ✅ Custos, performance, integrações e dependências

Ou seja, há muito mais envolvido do que simplesmente "subir a aplicação na AWS".

Os 7 Rs servem justamente para ajudar você a avaliar qual estratégia faz mais sentido para cada sistema, reduzindo riscos e guiando um plano claro.

---

## ⭐ Os 7 Rs da Migração

Aqui estão eles:

---

### 1️⃣ Rehosting ("Lift and Shift")

Quando você pensa em rehosting no contexto de migração para a nuvem, normalmente esse método é chamado de **"lift and shift"**.

Você literalmente pega a sua infraestrutura como ela funciona hoje no on-premises e move para a AWS com o mínimo possível de mudanças.

**É mover as aplicações "como estão", sem reformular arquitetura, sem reescrever código, apenas replicando o ambiente no cloud.**

**Esse método é:**

- ✅ Muito comum para quem está começando a jornada de migração
- ✅ Geralmente o mais rápido
- ✅ Exige pouco esforço de desenvolvimento
- ✅ Reduz riscos de interrupção

**Mas tem um ponto negativo importante:**

➡️ Você não aproveita todo o potencial da AWS (elasticidade, automação, serverless, custo otimizado, etc.).

Além disso, nem todos os workloads conseguem ser movidos tão facilmente ― por dependências, hardware, licenças, redes, e assim por diante.

**Por isso, é essencial planejar e verificar se a migração é realmente viável antes de mover aplicações e dados.**

**Exemplo:**

Migrar VMs on-premises para EC2 usando AWS Application Migration Service.

**Características:**

- ✅ Migração rápida
- ✅ Baixo custo inicial
- ✅ Reduz complexidade
- ✅ Ideal para iniciantes em cloud
- ⚠️ Não aproveita benefícios nativos da nuvem
- ⚠️ Pode ter custos maiores no longo prazo
- ⚠️ Requer validação de viabilidade antes da migração

---

### 2️⃣ Replatforming ("Lift, Tinker and Shift")

No replatforming você começa a aproveitar alguns ganhos rápidos ("low-hanging fruit") da nuvem, principalmente ao usar serviços gerenciados da AWS.

**A ideia aqui não é reescrever nada.**

➡️ A lógica central da aplicação continua igual.

➡️ Mas você substitui partes da infraestrutura por serviços AWS.

**Exemplo clássico:**

Você tem vários bancos relacionais em servidores on-premises.

Em vez de migrar para EC2 (como faria no rehosting), você usa:

- ✅ **AWS Database Migration Service (DMS)** para migrar
- ✅ **Amazon RDS** como destino

**Isso elimina:**

- ✅ Administração manual
- ✅ Patching
- ✅ Upgrades
- ✅ Replicação configurada na mão

**E abre portas para:**

- ✅ Automação
- ✅ Escalabilidade
- ✅ Procedimentos de alta disponibilidade
- ✅ Redução de custos
- ✅ Mais confiabilidade para o cliente

**Essa lógica se aplica não só a bancos de dados, mas também a:**

- ✅ Compute
- ✅ Storage
- ✅ Rede
- ✅ Segurança
- ✅ Machine Learning
- ✅ Qualquer categoria da AWS

**A chave aqui é:** ter conhecimento suficiente dos serviços AWS para tomar boas decisões de migração.

**Características:**

- ✅ Aproveita alguns serviços gerenciados
- ✅ Menos esforço que refactoring completo
- ✅ Melhor custo-benefício que rehosting puro
- ✅ Elimina tarefas operacionais manuais
- ⚠️ Ainda requer ajustes manuais
- ⚠️ Requer conhecimento dos serviços AWS

---

### 3️⃣ Repurchasing ("Drop and Shop")

O repurchasing é provavelmente o mais simples dos Rs — para workloads específicos.

**Em vez de mover sua aplicação atual para a AWS, você simplesmente compra uma nova versão SaaS equivalente, geralmente no AWS Marketplace.**

**Exemplo típico:**

Você tem um ERP ou CRM antigo, que não funciona bem na nuvem.

Em vez de migrá-lo, você compra um ERP/CRM SaaS moderno e desativa o legado.

**Vantagens:**

- ✅ O produto fica sempre atualizado
- ✅ Manutenção e patches são feitos pelo fornecedor
- ✅ Você ganha recursos modernos
- ✅ Corta infraestrutura antiga
- ✅ Reduz esforço e tempo de migração

**Para certos sistemas, é literalmente a solução mais rápida e mais barata.**

**Exemplos:**

- ✅ Substituir um CRM próprio por Salesforce
- ✅ Substituir um ERP local por SAP S/4HANA Cloud
- ✅ Usar soluções do AWS Marketplace

**Características:**

- ✅ Elimina necessidade de gerenciar infraestrutura
- ✅ Foco no negócio, não em TI
- ✅ Atualizações automáticas
- ✅ Solução mais rápida para casos específicos
- ⚠️ Pode ter custos recorrentes maiores
- ⚠️ Menos controle sobre a solução

---

### 4️⃣ Refactoring / Re-architecting

Essa é a estratégia mais profunda, mais cara e mais demorada — mas também a que traz mais benefícios a longo prazo.

**Refatorar/re-arquitetar significa repensar completamente a solução usando:**

- ✅ Serviços gerenciados AWS
- ✅ Arquitetura serverless
- ✅ Microservices
- ✅ Eventos
- ✅ Filas
- ✅ Padrões distribuídos
- ✅ Automação completa

**O problema com aplicações on-premises:**

Aplicações on-premises costumam ser monolíticas, altamente acopladas. Qualquer mudança impacta tudo.

**A solução com cloud-native:**

Ao re-arquitetar com microserviços e serviços desacoplados:

- ✅ Cada componente opera de forma independente
- ✅ Falhas são isoladas
- ✅ Você escala só o que precisa
- ✅ Evolução é muito mais rápida

**E não é só a aplicação:**

Você pode re-arquitetar seus processos de:

- ✅ Segurança
- ✅ Monitoramento
- ✅ Observabilidade
- ✅ Redes
- ✅ CI/CD
- ✅ Governança
- ✅ Backup e DR
- ✅ Tudo mesmo

**Apesar do custo inicial maior, as vantagens são enormes:**

- ✅ Escalabilidade extrema
- ✅ Performance superior
- ✅ Maior agilidade de negócio
- ✅ Redução de custos no longo prazo

**E, sinceramente:**

➡️ Quase toda aplicação migrada para a nuvem acaba sendo re-arquitetada em algum momento da vida útil.

**Características:**

- ✅ Máximo aproveitamento dos serviços cloud
- ✅ Melhor performance e escalabilidade
- ✅ Otimização de custos no longo prazo
- ✅ Arquitetura moderna e resiliente
- ⚠️ Maior investimento inicial
- ⚠️ Requer mais tempo e recursos
- ⚠️ Estratégia de longo prazo

---

### 5️⃣ Relocating (VMware → VMware Cloud on AWS)

Relocating é parecido com rehosting, mas ocorre no nível do hipervisor.

**Esse método é específico para ambientes VMware on-premises que migram para VMware Cloud on AWS.**

VMware é extremamente comum em empresas no mundo inteiro, então este método:

- ✅ Permite mover as VMs para a AWS sem alterar nada na aplicação
- ✅ Mantém as mesmas ferramentas VMware
- ✅ Mantém as mesmas APIs
- ✅ Permite estender o data center para a nuvem
- ✅ Habilita o uso de serviços AWS juntos (ML, serverless, analytics, etc.)

**A grande vantagem:**

➡️ Você não precisa reescrever ou reconfigurar nada.

➡️ É literalmente estender seu ambiente atual para dentro da AWS.

**Soluções disponíveis:**

- ✅ VMware Cloud on AWS
- ✅ AWS Outposts

**Características:**

- ✅ Migração sem mudanças na aplicação
- ✅ Mantém ambiente VMware existente
- ✅ Útil para ambientes híbridos
- ✅ Permite usar serviços AWS nativos junto
- ⚠️ Pode não aproveitar todos os benefícios cloud
- ⚠️ Específico para ambientes VMware

---

### 6️⃣ Retiring

Durante o inventário da migração, você inevitavelmente encontra:

- ✅ Sistemas antigos
- ✅ Aplicações sem dono
- ✅ Coisas que "ninguém sabe por que ainda existem"

**Migrar isso não faz sentido.**

Consome tempo. Gasta dinheiro. Traz risco.

**O caminho correto é:**

➡️ Desligar e aposentar.

**Migração é também um processo de limpeza que ajuda a enxergar o que realmente importa.**

**Características:**

- ✅ Reduz custos imediatamente
- ✅ Simplifica portfólio
- ✅ Reduz superfície de ataque
- ✅ Libera recursos para outras iniciativas
- ✅ Processo de limpeza organizacional

---

### 7️⃣ Retaining ("Revisitar mais tarde")

Migrar para a nuvem não significa migrar tudo.

**Haverá casos onde você decide manter aplicações on-premises, por vários motivos:**

- ✅ Dependências
- ✅ Custo
- ✅ Compliance
- ✅ Baixa prioridade
- ✅ Falta de benefício real ao migrar

**Isso é especialmente comum no início da jornada cloud**, quando você começa migrando:

- ✅ Backups
- ✅ DR
- ✅ Workloads pequenos
- ✅ Aplicações simples

**Com o tempo, conforme o time aprende AWS e ganha confiança, os workloads que foram "retidos" podem ser migrados no futuro.**

**"Retain" significa:** vamos voltar a avaliar isso no futuro.

**Características:**

- ✅ Evita migrações prematuras
- ✅ Permite planejamento adequado
- ✅ Reduz riscos de migração forçada
- ✅ Comum no início da jornada cloud
- ⚠️ Requer revisão periódica

---

## 📊 Comparação dos 7 Rs

| R | Estratégia | Esforço | Custo Inicial | Benefício Longo Prazo | Quando Usar |
|---|-----------|---------|---------------|----------------------|-------------|
| **Rehosting** | Lift and Shift | Baixo | Baixo | Baixo | Migração rápida, baixo risco |
| **Replatforming** | Lift, Tinker and Shift | Médio | Médio | Médio | Aproveitar serviços gerenciados |
| **Repurchasing** | Drop and Shop | Baixo | Variável | Alto | Substituir por SaaS |
| **Refactoring** | Re-architect | Alto | Alto | Muito Alto | Aplicações críticas, longo prazo |
| **Relocating** | Realocar infra | Baixo | Médio | Médio | Ambientes VMware, híbrido |
| **Retiring** | Desligar | Muito Baixo | Negativo | Alto | Aplicações obsoletas |
| **Retaining** | Manter | Nenhum | Nenhum | N/A | Dependências, compliance |

---

## 🎯 Como Escolher o R Correto?

**Considere:**

1. **Urgência da migração**
   - Rehosting ou Relocating para migrações rápidas
   - Refactoring para transformação estratégica

2. **Orçamento disponível**
   - Rehosting para orçamento limitado
   - Refactoring para investimento estratégico

3. **Complexidade da aplicação**
   - Aplicações simples → Rehosting
   - Aplicações complexas → Replatforming ou Refactoring

4. **Requisitos de negócio**
   - Compliance → Retaining ou Relocating
   - Inovação → Refactoring
   - Redução de custos → Retiring

5. **Dependências**
   - Muitas dependências → Retaining temporariamente
   - Poucas dependências → Qualquer estratégia

---

## 🔄 Estratégia de Migração Híbrida

**Importante:**

Cada workload pode seguir um R diferente — e frequentemente um portfólio inteiro envolve uma combinação deles.

**Exemplo de portfólio:**

- ✅ **30% Rehosting** — Aplicações simples, migração rápida
- ✅ **20% Replatforming** — Bancos de dados para RDS
- ✅ **10% Repurchasing** — CRM para Salesforce
- ✅ **15% Refactoring** — Aplicações críticas e estratégicas
- ✅ **10% Retiring** — Aplicações obsoletas
- ✅ **10% Retaining** — Aplicações com dependências complexas
- ✅ **5% Relocating** — Ambientes VMware

---

## 🔗 Recursos Adicionais

- [AWS Migration Hub](https://aws.amazon.com/migration-hub/)
- [AWS Application Migration Service](https://aws.amazon.com/application-migration-service/)
- [AWS Migration Strategies](https://aws.amazon.com/migration/)
- [VMware Cloud on AWS](https://aws.amazon.com/vmware/)
- [AWS Outposts](https://aws.amazon.com/outposts/)
- [AWS Migration Acceleration Program (MAP)](https://aws.amazon.com/migration-acceleration-program/)

---

## ✅ Checklist de Aprendizado

- [ ] Entender o propósito dos 7 Rs da migração
- [ ] Conhecer cada uma das 7 estratégias (Rehosting, Replatforming, Repurchasing, Refactoring, Relocating, Retiring, Retaining)
- [ ] Saber quando usar cada estratégia
- [ ] Compreender trade-offs entre esforço, custo e benefício
- [ ] Entender que múltiplos Rs podem ser usados em um mesmo portfólio
- [ ] Saber avaliar qual R é mais adequado para cada workload
- [ ] Compreender importância de Retiring e Retaining
- [ ] Entender diferença entre Rehosting e Replatforming

---

## 🏷️ Tags

`#aws` `#fundamentos` `#migration` `#7rs` `#lift-and-shift` `#rehosting` `#replatforming` `#refactoring` `#cloud-migration` `#strategy`

---

## 🎯 Conclusão

Os 7 Rs servem para:

- ✅ **Estruturar** a tomada de decisão
- ✅ **Reduzir** riscos
- ✅ **Definir** prioridades
- ✅ **Alinhar** estratégia do negócio com estratégia de migração

Cada workload pode seguir um R diferente — e frequentemente um portfólio inteiro envolve uma combinação deles.

---

**Última atualização**: 📅 [DD/MM/YYYY]

