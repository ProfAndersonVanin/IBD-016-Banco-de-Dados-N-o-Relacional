# IBD-016 — Banco de Dados Não Relacional

**Instituição:** FATEC Mauá
**Modalidade:** Presencial
**Carga horária:** 80 aulas (20 encontros de 4h/a)
**Período letivo:** 2º semestre de 2026
**Dia da semana:** Sábados
**Repositório da disciplina:** [github.com/ProfAndersonVanin/IBD-016-Banco-de-Dados-N-o-Relacional](https://github.com/ProfAndersonVanin/IBD-016-Banco-de-Dados-N-o-Relacional)

---

## Informações gerais

### Competências
- Desenvolver projetos de banco de dados com diferentes abordagens de modelagem e implementação.
- Utilizar técnicas de armazenamento e tratamento de dados não estruturados.
- Suportar a recuperação de dados usados em aplicações.

### Objetivos de aprendizagem
- Caracterizar banco de dados relacional x não relacional.
- Utilizar banco de dados não relacional.
- Utilizar sistemas de banco de dados paralelos e distribuídos.
- Compreender Data Warehouse e Mineração de Dados.
- Identificar métodos seguros de gerenciamento de banco de dados.

### Ementa
Dados estruturados e não estruturados; arquitetura de bancos de dados não convencionais; introdução a Data Warehouse; aplicações não convencionais; modelagem NoSQL (definições e motivação); categorias NoSQL (chave-valor, documentos, colunas, grafos); projeto lógico de banco de dados não relacional; implementações práticas das principais categorias NoSQL.

### Metodologia de ensino
- Aulas expositivas dialogadas.
- Aprendizagem Baseada em Projetos/Problemas (ABP).
- Gamificação.
- Estudo de caso real.
- Uso de ferramentas práticas online e gratuitas em todos os encontros (Redis Cloud, MongoDB Atlas, DataStax Astra DB, Neo4j AuraDB).

### Avaliação
- **Formativa:** exercícios práticos com rubrica de avaliação, ao longo do semestre.
- **Somativa:**
  - Avaliação 1 (P1): prova, aplicada em 17/10/2026.
  - Avaliação 2: projeto técnico, avaliado entre 09 e 14/11/2026.
  - Avaliação em pares e trabalhos interdisciplinares.
  - Validação do projeto final para inclusão no Portfólio Digital do aluno.

### Horários
- **Aula normal:** sábados, 9h50–11h30 e 11h40–13h20 (4h/a).
- **Reposição:** sábados, 8h00–9h40 e 13h20–15h00 (4h/a adicionais).

---

## Cronograma (atualizado em 09/09/2026)

*Cada Aula corresponde a um Encontro de 4h/a — 20 no total.*

### ✅ Aulas concluídas

| Aula | Data | Sessão | Conteúdo |
|---|---|---|---|
| 1 | 22/08 | Normal | Apresentação da disciplina; dados estruturados x não estruturados |
| 2 | 29/08 | Normal | Limitações do modelo relacional; Teorema CAP; motivação NoSQL |
| 3 | 29/08 | Reposição | Introdução ao NoSQL: categorias e casos de uso |
| 4 | 05/09 | Normal | Chave-valor: conceitos e arquitetura |
| 5 | 05/09 | Reposição | Prática chave-valor — Redis Cloud (free tier), CRUD |

### 🔜 Aulas previstas

| Aula | Data | Sessão | Conteúdo |
|---|---|---|---|
| 6 | 17/10 | Normal | Chave-valor aplicado: cache e sessões |
| 7 | 17/10 | Reposição | Revisão geral + **Avaliação 1 (P1)** |
| 8 | 24/10 | Normal | Orientado a documentos: conceitos |
| 9 | 24/10 | Reposição | Prática documentos — MongoDB Atlas (free tier), CRUD |
| 10 | 31/10 | Normal | MongoDB Atlas: Aggregation Pipeline |
| 11 | 31/10 | Reposição | Orientado a colunas: conceitos |
| 12 | 07/11 | Normal | Prática colunas — DataStax Astra DB |
| 13 | 07/11 | Reposição | Orientado a grafos: conceitos |
| 14 | 14/11 | Normal | Prática grafos — Neo4j AuraDB |
| 15 | 14/11 | Reposição | Orientação final + **Avaliação 2** |
| 16 | 21/11 | Normal | Projeto lógico de banco de dados não relacional |
| 17 | 28/11 | Normal | Data Warehouse e Mineração de Dados |
| 18 | 05/12 | Normal | BD paralelos/distribuídos; segurança e gerenciamento |
| 19 | 12/12 | Normal | Finalização e ensaio das apresentações |
| 20 | 12/12 | Reposição | Apresentação dos projetos + Portfólio Digital |

**Observação:** os sábados 12/09, 19/09, 26/09, 03/10 e 10/10/2026 não têm aula normal (ausência do docente); o conteúdo correspondente já está redistribuído nas sessões de reposição das aulas 7, 9, 11, 13 e 15.

---

## Bibliografia

### Básica
- BOAGLIO, F. *MongoDB: Construa novas aplicações com novas tecnologias*. Casa do Código, 2015.
- ELMASRI, R.; NAVATHE, S. B. *Sistemas de Banco de Dados: Fundamentos e Aplicações*. 7ed. Pearson, 2019.
- SADALAGE, P.; FOWLER, M. *NoSQL Essencial*. Novatec, 2013.
- SINGH, H. *Data Warehouse: conceitos, tecnologias, implementação e gerenciamento*. Makron Books, 2001.

### Complementar
- FAROULT, S. *Refatorando Aplicativos SQL*. Alta Books, 2009.
- PANIZ, D. *NoSQL: Como armazenar os dados de uma aplicação moderna*. Casa do Código, 2016.
- SOUZA, M. *Desvendando o MongoDB*. Ciência Moderna, 2015.

### Referências acadêmicas complementares (aprofundamento teórico)
- CODD, E. F. *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM, 1970.
- BREWER, E. *Towards Robust Distributed Systems*. Keynote, ACM PODC, 2000.
- GILBERT, S.; LYNCH, N. *Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services*. ACM SIGACT News, v. 33, 2002.
- BREWER, E. *CAP Twelve Years Later: How the "Rules" Have Changed*. IEEE Computer, 2012.
- ABADI, D. *Consistency Tradeoffs in Modern Distributed Database System Design: CAP is Only Part of the Story*. IEEE Computer, v. 45, n. 2, 2012.
- LANEY, D. *3D Data Management: Controlling Data Volume, Velocity and Variety*. META Group (Gartner), 2001.
