# Aula 8 – IBD-016 Banco de Dados Não Relacional
**Tema:** Orientado a documentos: conceitos

**Objetivo da aula:** compreender o modelo orientado a documentos (JSON/BSON, coleções, flexibilidade de esquema), aprender a decidir entre **embutir** e **referenciar** dados ao modelar, relacionar a linguagem de consulta do MongoDB com o SQL e avaliar quando esse modelo é a escolha certa — preparando a prática da Aula 9.

---

## Bloco 1


### 1. Do chave-valor ao documento 

- No Redis, o valor é um **bloco opaco**: para achar produtos com preço acima de R$ 200 seria preciso varrer tudo ou manter índices por fora.
- No **banco orientado a documentos**, o valor é um **documento estruturado** (campos, objetos aninhados e listas) que o próprio banco entende. É possível **filtrar por qualquer campo, indexá-lo e atualizá-lo parcialmente**.
- Em termos de aplicação: o Hash do Redis guarda campos planos; o documento guarda a mesma ideia com **aninhamento** e **listas**, e com consultas ricas.

### 2. O modelo de documentos

**Correspondência com o relacional:**

| Relacional | MongoDB |
|---|---|
| Banco de dados | Database |
| Tabela | Collection (coleção) |
| Linha | Document (documento) |
| Coluna | Field (campo) |
| Chave primária | `_id` (criado automaticamente, tipo ObjectId) |
| JOIN | Documentos embutidos ou `$lookup` |
| Índice | Index |

**Exemplo de documento:**
```json
{
  "_id": "ObjectId(...)",
  "nome": "Teclado Mecânico",
  "preco": 350.0,
  "categoria": "periféricos",
  "estoque": 42,
  "tags": ["gamer", "usb"],
  "especificacoes": { "switch": "red", "layout": "ABNT2", "retroiluminado": true }
}
```
Observe os três elementos que o relacional não representa bem em uma única linha: um **valor simples** (`nome`), uma **lista** (`tags`) e um **objeto aninhado** (`especificacoes`).

**JSON x BSON.** O MongoDB usa **BSON**, uma serialização binária de JSON que acrescenta tipos (ObjectId, Date, Int32/Int64, Decimal128, dados binários) e permite percorrer o documento com eficiência. Cada documento pode ter até **16 MB**. Para valores monetários, prefira `Decimal128` ou guardar centavos como inteiro: números de ponto flutuante acumulam erros de arredondamento.

**Flexibilidade de esquema.** Documentos da mesma coleção podem ter campos diferentes. Isso responde diretamente à **rigidez de esquema** vista na Aula 2: evoluir o modelo não exige `ALTER TABLE`. O preço é o risco de inconsistência (um documento com `preco` como texto, outro como número). Duas defesas:
- **Validação de esquema** (`$jsonSchema`) definida na própria coleção.
- Modelos e validações na aplicação.

### Atividade 1 — Anatomia de um documento 

Considere o documento:
```json
{
  "titulo": "Interestelar",
  "ano": 2014,
  "generos": ["ficção científica", "drama"],
  "diretor": { "nome": "Christopher Nolan", "nascimento": 1970 },
  "elenco": [
    { "nome": "Matthew McConaughey", "papel": "Cooper" },
    { "nome": "Anne Hathaway", "papel": "Brand" }
  ],
  "avaliacao": 8.7
}
```
**a)** Identifique um campo simples, um objeto aninhado, uma lista de valores e uma lista de objetos.
**b)** Como você escreveria, em português, o filtro "filmes de ficção científica"? E como acessaria o nome do diretor?
**c)** Quantas tabelas seriam necessárias para representar esse filme em um modelo relacional normalizado? Cite quais.
**d)** Você precisa acrescentar o campo `orcamento` apenas a este filme. O que acontece com os outros documentos da coleção?

### 3. Trade-offs: CAP, transações, índices e quando usar

- **CAP.** Em um *replica set* (conjunto de réplicas), uma instância primária aceita as escritas e as demais a replicam. Em uma partição, o lado sem maioria não consegue eleger um primário. Por isso o MongoDB, com a configuração padrão, tende a **CP**, e o comportamento pode ser ajustado (`writeConcern`, `readPreference`). O **sharding** distribui os dados entre vários servidores (escala horizontal, Aula 2).
- **Transações.** Toda operação sobre **um único documento é atômica**. Há transações ACID com vários documentos, mas custam desempenho. Modelar bem (dados relacionados no mesmo documento) reduz a necessidade delas.
- **Índices.** O campo `_id` já é indexado. Campos usados em filtros e ordenações devem ter índice (simples, composto ou de texto). Contrapartida: escritas ficam um pouco mais lentas e o índice ocupa memória.
- **Quando usar:** catálogos de produtos, perfis de usuário, gestão de conteúdo, eventos e logs, APIs que já trabalham com JSON, protótipos que mudam rápido.
- **Quando ter cuidado:** muitas relações N:N com junções complexas, relatórios analíticos relacionais pesados, integridade referencial rígida e transações intensas entre várias entidades. Nesses casos, o relacional continua sendo a melhor escolha.
- **Polyglot persistence na prática:** o catálogo vive no MongoDB e o **Redis fica à frente como cache-aside** (Aula 6), exatamente o cenário da questão 10 da P1.

---

## Bloco 2

### 4. Modelagem: embutir x referenciar

**Princípio:** *dados que são acessados juntos devem ficar juntos.* Modele a partir das consultas que a aplicação faz, e não a partir das entidades.

| | **Embutir** (documento dentro de documento) | **Referenciar** (guardar o `_id` de outro documento) |
|---|---|---|
| Quando | 1:1; 1:poucos, com limite conhecido; dados lidos sempre juntos | 1:muitos sem limite; N:N; dados usados isoladamente ou atualizados com frequência |
| Vantagem | Uma consulta traz tudo; atomicidade por documento | Evita duplicação; documentos menores |
| Custo | Duplicação; crescimento do documento (limite de 16 MB) | Duas consultas ou `$lookup` |

**Exemplo — pedido de e-commerce.** Os itens do pedido são lidos junto com ele e têm um número limitado, então são **embutidos**. Já o cliente é referenciado por `cliente_id`, porque existe e muda independentemente do pedido:
```json
{
  "numero": 1001,
  "cliente_id": "ObjectId(...)",
  "data": "2026-10-24",
  "status": "pago",
  "itens": [
    { "produto_id": "ObjectId(...)", "nome": "Teclado Mecânico", "quantidade": 1, "preco_unitario": 350.0 },
    { "produto_id": "ObjectId(...)", "nome": "Mouse Sem Fio", "quantidade": 2, "preco_unitario": 120.0 }
  ]
}
```
Repare que `nome` e `preco_unitario` são **copiados** para dentro do item. Essa duplicação é proposital: guarda o preço do momento da compra, mesmo que o produto mude depois (*snapshot*). Em um modelo relacional, o preço ficaria só na tabela de produtos.

**Antipadrões a evitar:**
- **Arrays que crescem sem limite** (ex.: todas as avaliações de um produto dentro do próprio produto): o documento incha e se aproxima dos 16 MB.
- **Documentos gigantes** que trazem dados que quase nunca são lidos.
- **Normalizar demais**, reproduzindo o relacional com dezenas de `$lookup`.
- **Tipos inconsistentes** para o mesmo campo.

### Atividade 2 — Modelando um pedido 

Um sistema relacional tem as tabelas:
```
clientes(id, nome, email, cidade)
produtos(id, nome, preco, categoria)
pedidos(id, cliente_id, data, status)
itens_pedido(pedido_id, produto_id, quantidade, preco_unitario)
```
**a)** Escreva o documento de um pedido, indicando o que você embutiria e o que referenciaria.
**b)** Por que `preco_unitario` é copiado para dentro do item do pedido?
**c)** As avaliações de produto podem chegar à casa dos milhares por produto. Elas devem ser embutidas no documento do produto ou ficar em uma coleção própria? Justifique.
**d)** Cite duas consultas que o seu modelo responde bem e uma que fica mais difícil.

### 5. A linguagem de consulta e o mapeamento SQL → MongoDB

O MongoDB tem uma linguagem de consulta própria (MQL), baseada em documentos JSON. Na Aula 9 ela será usada pelo Python (`pymongo`), com os mesmos nomes em *snake_case* (`insert_one`, `find`, `update_one`).

| SQL | MongoDB |
|---|---|
| `INSERT INTO ...` | `insertOne` / `insertMany` |
| `SELECT * FROM produtos` | `find()` |
| `WHERE preco > 100` | `find({ preco: { $gt: 100 } })` |
| `SELECT nome, preco` | `find({}, { nome: 1, preco: 1, _id: 0 })` |
| `ORDER BY preco DESC LIMIT 3` | `find().sort({ preco: -1 }).limit(3)` |
| `AND` / `OR` / `IN` | `$and` (implícito entre campos) / `$or` / `$in` |
| `UPDATE ... SET` | `updateOne({filtro}, { $set: {...} })` |
| `DELETE FROM ...` | `deleteOne` / `deleteMany` |
| `COUNT(*)` | `countDocuments` |

**Operadores que serão usados na prática:**
- **Comparação:** `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`, `$in`, `$nin`.
- **Lógicos:** `$and`, `$or`, `$not`.
- **Atualização:** `$set` (define campo), `$unset` (remove campo), `$inc` (soma), `$push` (acrescenta a uma lista), `$addToSet` (acrescenta sem repetir), `$pull` (remove da lista).
- **Listas:** `{ tags: "gamer" }` encontra documentos cuja lista contém "gamer".
- **Campos aninhados:** notação de ponto, como `"especificacoes.switch"`.

### Atividade 3 — De SQL para MongoDB

Escreva a consulta equivalente em MongoDB (no quadro compartilhado, sem executar). Considere as coleções `produtos`, `pedidos` e `clientes`.
1. `SELECT * FROM produtos WHERE categoria = 'periféricos';`
2. `SELECT nome, preco FROM produtos WHERE preco BETWEEN 100 AND 500 ORDER BY preco DESC;`
3. `SELECT COUNT(*) FROM pedidos WHERE status IN ('pago', 'enviado');`
4. `UPDATE produtos SET estoque = estoque - 1 WHERE id = 7;`
5. `DELETE FROM clientes WHERE cidade = 'Mauá' AND ativo = false;`

### 7. Encerramento e tarefa para a Aula 9

**Enquete rápida no chat (cada pergunta com uma resposta curta):**
1. Arrays que crescem sem limite: embutir ou referenciar? 
2. Qual operador soma um valor a um campo numérico? 
3. Em que tipo de dado um documento é melhor do que uma linha de tabela? 
4. O MongoDB, por padrão, tende a priorizar C ou A no CAP? 

---

## Bibliografia
**Básica:** BOAGLIO, F. *MongoDB: Construa novas aplicações com novas tecnologias*. Casa do Código, 2015. · SADALAGE, P.; FOWLER, M. *NoSQL Essencial*. Novatec, 2013.
**Complementar:** SOUZA, M. *Desvendando o MongoDB*. Ciência Moderna, 2015. · PANIZ, D. *NoSQL: Como armazenar os dados de uma aplicação moderna*. Casa do Código, 2016.
**Documentação oficial:** [mongodb.com/docs](https://www.mongodb.com/docs/)
