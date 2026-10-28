# Reposição 1 — Mapa dos bancos NoSQL (pesquisa)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Tipo:** pesquisa individual
**Tempo estimado:** 2 a 3 horas
**Ferramentas:** apenas um navegador
**Entrega:** um documento de 1 a 2 páginas (Word, Google Docs, PDF ou `.md`), pelo Teams ou conforme combinado com o professor

---

## Objetivo
Conhecer, por conta própria, bancos de dados NoSQL das quatro categorias estudadas e comparar as características de cada um, treinando também a escolha do banco certo para cada necessidade de um sistema.

## Antes de começar: vocabulário
- **Categorias:** chave-valor, documentos, colunas e grafos (vistas nas aulas 3 e 4).
- **Licença:** *código aberto* (pode ser usado e modificado livremente), *proprietário* (pago ou com uso restrito) ou *serviço em nuvem* (oferecido pronto por uma empresa, cobrado pelo uso).
- **DB-Engines:** site que mantém um ranking de popularidade de bancos de dados e uma página de resumo para cada um (`db-engines.com`).

---

## Passo a passo

### Passo 1 — Escolha 4 bancos, um de cada categoria
Regras:
- Um banco de **cada** categoria.
- **Pelo menos dois** bancos diferentes de Redis, MongoDB, Cassandra e Neo4j (os quatro que fazem parte do curso).

| Categoria | Sugestões |
|---|---|
| Chave-valor | Redis, Amazon DynamoDB, Memcached |
| Documentos | MongoDB, Apache CouchDB, Google Cloud Firestore, Amazon DocumentDB |
| Colunas | Apache Cassandra, ScyllaDB, Apache HBase, Google Bigtable |
| Grafos | Neo4j, Amazon Neptune, JanusGraph, Memgraph |

> Alguns bancos são **multimodelo** (aceitam mais de um tipo de dado). Nesse caso, use como categoria a que o site oficial ou o DB-Engines destaca em primeiro lugar.

### Passo 2 — Pesquise e preencha a tabela
Copie a tabela abaixo para o seu documento e preencha **uma coluna por banco**, com suas próprias palavras. Procure no **site oficial** do banco (documentação, páginas de "casos de uso" ou "customers") e na página do banco no **DB-Engines**.

| Campo | Banco 1 | Banco 2 | Banco 3 | Banco 4 |
|---|---|---|---|---|
| Nome | | | | |
| Categoria | | | | |
| Como o dado é organizado | | | | |
| Como se consulta (linguagem ou forma de acesso) | | | | |
| Licença | | | | |
| Um caso de uso real (empresa ou tipo de sistema) | | | | |
| Um ponto forte | | | | |
| Uma limitação | | | | |
| Posição no ranking do DB-Engines **e a data da consulta** | | | | |

A posição no ranking muda com o tempo, por isso a data da consulta é obrigatória.

### Passo 3 — Qual banco para qual necessidade?
Imagine um **aplicativo de delivery de comida**. Ele tem quatro necessidades:

| Necessidade | Qual dos seus 4 bancos você usaria? Por quê? (1 ou 2 linhas) |
|---|---|
| **a)** Guardar temporariamente o cardápio mais acessado, para carregar mais rápido | |
| **b)** Cadastrar restaurantes, cada um com campos diferentes (horários, cardápio, fotos) | |
| **c)** Registrar milhões de posições de GPS dos entregadores, com muita escrita | |
| **d)** Recomendar "quem pediu isto também pediu aquilo" a partir das conexões entre clientes e pratos | |

Se um dos seus quatro bancos não for adequado para uma necessidade, escolha outro banco da tabela de sugestões e explique.

### Passo 4 — Fontes
No fim do documento, liste **todos os endereços (links)** que você consultou, com a **data de acesso**. Escreva tudo com as suas palavras: textos copiados não serão aceitos.

---

## Como será avaliado (10 pontos)
| Critério | Pontos |
|---|---|
| Tabela completa e correta (4 bancos, uma coluna cada) | 4,0 |
| Passo 3: escolhas coerentes e justificadas | 3,0 |
| Texto com palavras próprias e organizado | 2,0 |
| Fontes listadas com data de acesso | 1,0 |

## Checklist antes de enviar
- [ ] Escolhi um banco de cada categoria e dois deles estão fora dos quatro do curso
- [ ] Preenchi todos os campos da tabela, inclusive a posição no DB-Engines com a data
- [ ] Respondi as quatro necessidades do delivery
- [ ] Listei as fontes com a data de acesso
- [ ] Escrevi com minhas palavras
