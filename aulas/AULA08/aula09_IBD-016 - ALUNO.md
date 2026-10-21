# Aula 9 – IBD-016 Banco de Dados Não Relacional
**Tema:** Prática com documentos — MongoDB Atlas (free tier), CRUD
**Data:** 24/10/2026 (sábado) — Reposição (compensa a aula de 19/09) — 4h/a

**Objetivo da aula:** criar um cluster gratuito no MongoDB Atlas, conectar-se a ele pelo Google Colab e executar operações CRUD sobre documentos (inserir, consultar, atualizar e remover), além de criar um índice e conferir seu efeito. A prática fecha com uma atividade avaliativa: o sistema de tarefas da Aula 5, agora modelado como documentos.

---

## Parte 1 — Criar o cluster gratuito no Atlas 

> Os nomes dos botões do Atlas mudam de tempos em tempos (por exemplo, "Build a Database" ou "Create Deployment"). Se a tela estiver diferente, procure o item equivalente.

1. Acesse [mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register) e entre com a conta criada.
2. **Crie um cluster** e escolha o plano **gratuito (M0 / Free)**. Ele oferece 512 MB de armazenamento, até 500 conexões e não tem prazo de expiração. Escolha um provedor (AWS, Google Cloud ou Azure) e uma região próxima. Dê um nome ao cluster, por exemplo `ibd016`, e confirme. A criação leva de 1 a 3 minutos. Não escolha planos pagos.
3. **Crie um usuário do banco** (*Database Access*). Esse usuário é diferente do seu login no Atlas.
   - Defina um nome, por exemplo `aluno_ibd016`.
   - Defina uma senha. Recomenda-se **gerar automaticamente** ou usar apenas letras e números, evitando caracteres como `@`, `:`, `/` e `?`, que quebram a connection string.
   - O papel deve permitir **leitura e escrita** (por exemplo, "read and write to any database").
   - Anote usuário e senha em um lugar seguro.
4. **Libere o acesso de rede** (*Network Access*) em *Add IP Address → Allow Access from Anywhere* (`0.0.0.0/0`). Isso é necessário porque o Google Colab usa endereços IP que mudam.
   > **Atenção:** liberar para qualquer IP só é aceitável porque o cluster é de estudo. Não guarde dados sensíveis nele e, ao fim da disciplina, remova essa regra ou apague o cluster.
5. Quando o cluster estiver ativo, clique em **Connect → Drivers → Python** e copie a **connection string**. Ela é parecida com:
   ```
   mongodb+srv://aluno_ibd016:<db_password>@ibd016.abcde.mongodb.net/?retryWrites=true&w=majority
   ```
   O Atlas preenche o nome do usuário e deixa um marcador no lugar da senha (como `<db_password>`; o nome do marcador pode variar).

**Checkpoint:** ✅ cluster ativo, usuário criado, acesso de rede liberado e connection string copiada.

---

## Parte 2 — Conectar pelo Google Colab 

> **O Python que você vai usar nesta aula (e só ele):**
> - **Variável:** `produtos = db["produtos"]` guarda algo em um nome para usar depois.
> - **`print(...)`:** mostra um valor na tela.
> - **Dicionário** `{"campo": valor}`: é assim que se escreve um **documento** e também um **filtro** de busca.
> - **Lista** `["a", "b"]`: vários valores em sequência.
> - **`for p in ...:`**: repete a linha recuada logo abaixo para cada resultado encontrado.
> - **Comentário** `# texto`: o Python ignora; serve para explicar o código.
>
> Os comandos do MongoDB (`insert_one`, `find`, `update_one`...) são apenas chamadas, como `print`. Não há funções, classes ou outros recursos avançados.

1. Acesse [colab.research.google.com](https://colab.research.google.com/) e abra o notebook-base (ou crie um novo).
2. **Célula 1 — instalar o driver:**
   ```python
   !pip install pymongo
   ```
3. **Célula 2 — conectar.** Antes de colar a string, **troque o marcador da senha (`<db_password>`) pela senha do usuário do banco, sem os sinais `<` e `>`**:
   ```python
   from getpass import getpass
   from pymongo import MongoClient

   uri = getpass("Cole a connection string (já com a senha): ")
   client = MongoClient(uri)

   print(client.admin.command("ping"))
   ```
   **Saída esperada:** um dicionário contendo `'ok': 1.0` (podem aparecer campos adicionais, como `$clusterTime`). Se algo estiver errado (rede, usuário ou senha), o erro pode levar até 30 segundos para aparecer.
4. **Célula 3 — escolher o banco e a coleção:**
   ```python
   db = client["ibd016_aula9"]
   produtos = db["produtos"]
   produtos.delete_many({})   # limpa a coleção para que o notebook possa ser reexecutado
   ```
   Repare que o banco e a coleção **só passam a existir quando o primeiro documento é inserido**.

**Checkpoint:** ✅ o `ping` retornou `ok: 1.0`.

---

## Parte 3 — Inserir documentos: Create

**3.1 — `insert_one`:**
```python
doc = {
    "nome": "Teclado Mecânico",
    "preco": 350.0,
    "categoria": "periféricos",
    "estoque": 42,
    "tags": ["gamer", "usb"],
    "especificacoes": {"switch": "red", "layout": "ABNT2", "retroiluminado": True}
}
resultado = produtos.insert_one(doc)
print("Inserido com _id:", resultado.inserted_id)
```
**Saída esperada:** `Inserido com _id: ` seguido de um ObjectId (por exemplo, `6720...`). O `_id` foi criado automaticamente.

**3.2 — `insert_many`** (repare que os documentos têm campos diferentes entre si):
```python
lote = [
    {"nome": "Mouse Sem Fio", "preco": 120.0, "categoria": "periféricos", "estoque": 80, "tags": ["sem fio", "usb"]},
    {"nome": "Monitor 24 polegadas", "preco": 899.9, "categoria": "monitores", "estoque": 15,
     "especificacoes": {"resolucao": "1920x1080", "painel": "IPS", "hz": 75}},
    {"nome": "Headset Gamer", "preco": 249.0, "categoria": "áudio", "estoque": 30, "tags": ["gamer", "usb", "microfone"]},
    {"nome": "Webcam Full HD", "preco": 199.0, "categoria": "periféricos", "estoque": 0, "tags": ["usb", "reunião"]},
    {"nome": "Cadeira Ergonômica", "preco": 1250.0, "categoria": "mobiliário", "estoque": 6, "garantia_meses": 24},
]
res = produtos.insert_many(lote)
print(len(res.inserted_ids), "documentos inseridos")
print("Total na coleção:", produtos.count_documents({}))
```
**Saída esperada:** `5 documentos inseridos` e `Total na coleção: 6`.

**3.3 — Conferir no Atlas:** no menu do cluster, abra **Browse Collections** (Data Explorer), escolha o banco `ibd016_aula9` e a coleção `produtos`. Os seis documentos devem aparecer. **Tire um print desta tela**, ele será evidência da entrega.

**Checkpoint:** ✅ total de 6 documentos e visualização no Atlas.

---

## Parte 4 — Consultar documentos: Read

O comando `find` devolve vários resultados. Para ver cada um deles, use um `for`, como nos exemplos: ele repete a linha recuada (com 4 espaços) uma vez para cada documento encontrado.

**4.1 — Filtro simples e `find_one`:**
```python
for p in produtos.find({"categoria": "periféricos"}):
    print(p["nome"], p["preco"])

webcam = produtos.find_one({"nome": "Webcam Full HD"})
print(webcam)
```

**Saída esperada:** Teclado Mecânico (350.0), Mouse Sem Fio (120.0) e Webcam Full HD (199.0), seguidos do documento completo da webcam.

De forma mais simples:

```python
webcam = produtos.find_one({"nome": "Webcam Full HD"})
print(webcam)

for p in produtos.find({"categoria": "periféricos"}):
    print(p)
```


**4.2 — Operadores de comparação, projeção e ordenação:**
```python
for p in produtos.find({"preco": {"$gt": 200}}, {"nome": 1, "preco": 1, "_id": 0}).sort("preco", -1):
    print(p)
```
**Saída esperada (4 itens, do mais caro ao mais barato):** Cadeira Ergonômica (1250.0), Monitor 24 polegadas (899.9), Teclado Mecânico (350.0) e Headset Gamer (249.0).


De forma mais simples:
Passo 1 — comparação ($gt, maior que)

```
filtro = {"preco": {"$gt": 200}}

for p in produtos.find(filtro):
    print(p)

```

Passo 2 — ordenação

```
filtro = {"preco": {"$gt": 200}}

for p in produtos.find(filtro).sort("preco", -1):
    print(p)

```


Passo 3 — projeção (escolher os campos)

```
filtro = {"preco": {"$gt": 200}}
campos = {"nome": 1, "preco": 1, "_id": 0}

for p in produtos.find(filtro, campos).sort("preco", -1):
    print(p)

```


**4.3 — Listas e campos aninhados:**
```python
print("Produtos com a tag gamer:")
for p in produtos.find({"tags": "gamer"}):
    print(p["nome"])

print("Produtos com painel IPS:")
for p in produtos.find({"especificacoes.painel": "IPS"}):
    print(p["nome"])
```
**Saída esperada:** Teclado Mecânico e Headset Gamer na primeira lista, e Monitor 24 polegadas na segunda.

**4.4 — Operadores lógicos e `$in`:**
```python
print("Sem estoque ou com preço abaixo de 130:")
for p in produtos.find({"$or": [{"estoque": 0}, {"preco": {"$lt": 130}}]}):
    print(p["nome"])

print("Categoria áudio ou monitores:")
for p in produtos.find({"categoria": {"$in": ["áudio", "monitores"]}}):
    print(p["nome"])
```
**Saída esperada:** Mouse Sem Fio e Webcam Full HD na primeira lista, e Monitor 24 polegadas e Headset Gamer na segunda. A ordem pode variar.

**4.5 — Contagem:**
```python
print("Com estoque:", produtos.count_documents({"estoque": {"$gt": 0}}))
```
**Saída esperada:** `Com estoque: 5`.

**Erros comuns desta parte:** esquecer as aspas nos operadores (`"$gt"`, e não `$gt`), esquecer os dois-pontos no fim da linha do `for` e esquecer de recuar a linha do `print` abaixo dele.

**Checkpoint:** ✅ as consultas retornaram os resultados esperados.

---

## Parte 5 — Atualizar documentos: Update 

Toda atualização usa um **filtro** (quem alterar) e um **operador** (como alterar). Sem o operador `$set`, o documento inteiro seria substituído.

```python
# 5.1 — $set: reajustar o preço de um produto
r = produtos.update_one({"nome": "Mouse Sem Fio"}, {"$set": {"preco": 129.9}})
print("encontrados:", r.matched_count, "| alterados:", r.modified_count)

# 5.2 — $inc: baixa de estoque após uma venda
produtos.update_one({"nome": "Teclado Mecânico"}, {"$inc": {"estoque": -1}})

# 5.3 — $push: acrescentar um item a uma lista
produtos.update_one({"nome": "Teclado Mecânico"}, {"$push": {"tags": "mecânico"}})

# 5.4 — update_many: alterar vários documentos de uma vez
produtos.update_many({"estoque": 0}, {"$set": {"disponivel": False}})
produtos.update_many({"estoque": {"$gt": 0}}, {"$set": {"disponivel": True}})

teclado = produtos.find_one({"nome": "Teclado Mecânico"})
print(teclado)
```
**Saída esperada:** `encontrados: 1 | alterados: 1` e, no último `print`, o teclado com `estoque` 41, `tags` `['gamer', 'usb', 'mecânico']` e `disponivel: True`.

**Checkpoint:** ✅ estoque do teclado em 41 e `disponivel` presente em todos os documentos.

---

## Parte 6 — Remover documentos: Delete 

```python
r = produtos.delete_one({"nome": "Cadeira Ergonômica"})
print("removidos:", r.deleted_count)

r = produtos.delete_many({"disponivel": False})
print("removidos:", r.deleted_count)

print("Restam:", produtos.count_documents({}))
```
**Saída esperada:** `removidos: 1`, `removidos: 1` (a webcam, sem estoque) e `Restam: 4`.

> `delete_many({})` apaga **todos** os documentos da coleção. É o que a Célula 3 usa para reiniciar o exercício, e é também o erro clássico de quem esquece o filtro.

---

## Parte 7 — Índices 

```python
produtos.create_index("categoria")
print(produtos.index_information())
```
**Saída esperada:** a lista de índices mostra o índice padrão `_id_` e o novo `categoria_1`.

**Para que serve:** sem índice, o banco precisa olhar todos os documentos para responder a um filtro por `categoria`. Com o índice, ele vai direto aos documentos certos, como o índice de um livro. Em uma coleção de 4 documentos a diferença não aparece, mas em milhões ela é enorme.

---

## Atividade avaliativa — Sistema de tarefas com MongoDB 

Na Aula 5, o sistema de tarefas do Redis exigiu uma lista, um hash, um set e um contador separados. Agora você vai montar o mesmo sistema com **documentos**.

**Enunciado:**

1. **Criar.** Em uma coleção `tarefas`, insira ao menos **4 tarefas**, cada uma com `titulo`, `prioridade` (alta, media ou baixa), `responsavel`, `tags` (lista), `concluida` (verdadeiro ou falso) e `criada_em` (a data, escrita como texto). Pelo menos uma tarefa deve ter um campo extra que as outras não têm. *Comandos:* `insert_many` (ou `insert_one`). *Dica:* para a data, escreva um texto, como `"2026-10-24"`.
2. **Consultar.**
   - **a)** as tarefas de prioridade alta que ainda não foram concluídas;
   - **b)** as tarefas que tenham a tag `estudo`;
   - **c)** somente `titulo` e `responsavel`, em ordem alfabética de título;
   - **d)** a quantidade de tarefas concluídas.
   *Comandos:* `find` (com filtro, projeção e `sort`) e `count_documents`.
3. **Atualizar.** Marque uma tarefa como concluída; acrescente uma tag nova a outra, sem duplicar tags; e altere a prioridade de todas as tarefas de um mesmo responsável. *Comandos:* `update_one` e `update_many`, com `$set` e `$addToSet`.
4. **Remover.** Apague todas as tarefas concluídas e confirme a quantidade que restou. *Comandos:* `delete_many` e `count_documents`.
5. **Índice.** Crie um índice no campo `prioridade` e mostre a lista de índices da coleção. *Comandos:* `create_index` e `index_information`.


Tire os prints de cada etapa **antes** de apagar os documentos no item 4.

---

## Bibliografia
**Básica:** BOAGLIO, F. *MongoDB: Construa novas aplicações com novas tecnologias*. Casa do Código, 2015.
**Complementar:** SOUZA, M. *Desvendando o MongoDB*. Ciência Moderna, 2015.
**Documentação oficial:** [mongodb.com/docs/atlas](https://www.mongodb.com/docs/atlas/) e [pymongo.readthedocs.io](https://pymongo.readthedocs.io/)
