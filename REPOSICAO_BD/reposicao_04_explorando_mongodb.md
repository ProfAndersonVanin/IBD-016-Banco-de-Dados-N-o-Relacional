# Reposição 4 — Explorando dados reais no MongoDB Atlas (filmes)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Tipo:** atividade prática individual
**Tempo estimado:** 2 a 3 horas
**Ferramentas:** MongoDB Atlas (plano gratuito) e Google Colab (Python)
**Entrega:** os prints (ou o link do notebook), suas 3 consultas e a reflexão final, pelo Teams ou conforme combinado com o professor

---

## Objetivo
Praticar consultas no MongoDB sobre **dados reais**: o Atlas oferece conjuntos de dados de exemplo, e você vai explorar o de **filmes** (`sample_mflix`). Aqui você só **lê** os dados: não altere nem apague nada.

**O Python que você vai usar:** variáveis, `print`, dicionários, listas e `for`, como nas aulas 9 e 10.

## Antes de começar
- [ ] Seu cluster gratuito do MongoDB Atlas está **ativo** (criado na aula 9), com usuário do banco criado e acesso de rede liberado (`0.0.0.0/0`).
- [ ] Você tem uma conta Google para abrir o [Google Colab](https://colab.research.google.com/).

---

## Passo a passo

### Passo 1 — Carregar os dados de exemplo no Atlas
1. No Atlas, clique em **Database** no menu lateral.
2. No seu cluster, clique nos **três pontinhos (...)** e escolha **Load Sample Dataset**.
3. Se aparecer uma lista de conjuntos de dados, marque apenas o **`sample_mflix`** (os outros não são necessários) e confirme.
4. Aguarde cerca de 5 minutos. Ao terminar, clique em **Browse Collections**: você deve ver o banco `sample_mflix`.

> O conjunto completo ocupa boa parte dos 512 MB do plano gratuito. Carregue só o `sample_mflix` sempre que a tela permitir. Os nomes dos botões do Atlas podem variar um pouco; procure o item equivalente.

### Passo 2 — Conectar pelo Colab
Em um novo notebook, instale o driver e conecte-se, como na aula 9. A string de conexão é pedida em uma caixa e não fica escrita no notebook.
```python
!pip install pymongo
```
```python
from getpass import getpass
from pymongo import MongoClient

uri = getpass("Cole a connection string (já com a senha): ")
client = MongoClient(uri)

print(client.admin.command("ping"))
```
Escolha o banco e a coleção de filmes:
```python
db = client["sample_mflix"]
filmes = db["movies"]
```
**Resultado esperado do `ping`:** um dicionário com `'ok': 1.0`.

### Passo 3 — Quantos filmes existem? E como é um documento?
```python
print("Total de filmes:", filmes.count_documents({}))
print(filmes.find_one())
```
Você deve ver mais de 20 mil filmes (o número exato pode variar). No documento impresso, procure estes campos:
- `title` — o título;
- `year` — o ano;
- `genres` — **lista** de gêneros;
- `cast` — **lista** de atores;
- `runtime` — duração em minutos;
- `imdb` — um **objeto** com `rating` (nota) e `votes` (número de votos).

### Passo 4 — Filtrar por um campo simples
Filmes de 1994:
```python
print("Filmes de 1994:", filmes.count_documents({"year": 1994}))

for f in filmes.find({"year": 1994}).limit(5):
    print(f["title"])
```
O `limit(5)` mostra só os 5 primeiros, para a saída não ficar enorme.

### Passo 5 — Filtrar por um valor dentro de uma lista
Filmes do gênero Comedy (o MongoDB procura o valor dentro da lista `genres`):
```python
print("Comédias:", filmes.count_documents({"genres": "Comedy"}))

for f in filmes.find({"genres": "Comedy"}).limit(5):
    print(f["title"])
```

### Passo 6 — Comparar números, dentro de um objeto
Filmes com nota 9 ou mais. A nota está dentro do objeto `imdb`, por isso usamos a **notação de ponto**: `"imdb.rating"`.
```python
filtro = {"imdb.rating": {"$gte": 9}}
campos = {"title": 1, "imdb.rating": 1, "_id": 0}

print("Filmes com nota 9 ou mais:", filmes.count_documents(filtro))

for f in filmes.find(filtro, campos).limit(5):
    print(f)
```
- `$gte` significa "maior ou igual".
- `campos` escolhe o que aparece: `1` mostra, `0` esconde.

### Passo 7 — Ordenar
Os 5 filmes mais longos. O filtro `{"runtime": {"$gt": 0}}` garante que só entrem filmes com duração numérica.
```python
filtro = {"runtime": {"$gt": 0}}
campos = {"title": 1, "runtime": 1, "_id": 0}

for f in filmes.find(filtro, campos).sort("runtime", -1).limit(5):
    print(f)
```
`sort("runtime", -1)` ordena do maior para o menor. **Resultado esperado:** 5 linhas, com a duração em ordem decrescente.

### Passo 8 — Combinar condições
Comédias lançadas de 2000 em diante com nota 8 ou mais. Vários campos no mesmo filtro funcionam como "**e**": todas as condições precisam ser verdadeiras.
```python
filtro = {"genres": "Comedy", "year": {"$gte": 2000}, "imdb.rating": {"$gte": 8}}
campos = {"title": 1, "year": 1, "imdb.rating": 1, "_id": 0}

print("Quantidade:", filmes.count_documents(filtro))

for f in filmes.find(filtro, campos).limit(5):
    print(f)
```

**Tire um print das saídas dos passos 3 a 8.**

### Passo 9 — Sua vez: três perguntas suas
Invente **3 perguntas próprias** sobre os filmes e responda cada uma com uma consulta. Escreva a pergunta em um comentário (`# ...`) acima do código. Ideias:
- filmes de um **país** (`"countries": "Brazil"`);
- filmes com um **ator** que você gosta (`"cast": "Nome do Ator"`);
- filmes de um **gênero** com mais de 150 minutos de duração;
- quantos filmes têm **dois gêneros específicos**.

Use pelo menos **um** operador de comparação (`$gt`, `$gte`, `$lt` ou `$lte`) e **uma** consulta com `sort`. **Tire um print.**

### Passo 10 — Reflexão (em texto)
Responda em 3 a 5 linhas:
1. Os campos `genres` e `cast` são **listas dentro do documento**. Como essas informações seriam guardadas em um banco relacional?
2. Explore alguns documentos: todos têm exatamente os mesmos campos? O que isso mostra sobre a flexibilidade do modelo de documentos?
3. Que pergunta você gostaria de fazer a esse banco que ainda parece difícil com o que você aprendeu até agora?

---

## Como será avaliado (10 pontos)
| Critério | Pontos |
|---|---|
| Passos 1 a 8 executados, com os prints | 4,0 |
| Três perguntas próprias, com operador de comparação e `sort` (passo 9) | 3,0 |
| Reflexão (passo 10) | 2,0 |
| Notebook organizado e sem senha escrita | 1,0 |

## Problemas comuns
| Sintoma | O que fazer |
|---|---|
| `ServerSelectionTimeoutError` | Confira o acesso de rede (`0.0.0.0/0`) e se o cluster está ativo |
| `Authentication failed` | Revise a connection string: o marcador `<db_password>` precisa ser trocado pela senha, sem os sinais `< >` |
| `Total de filmes: 0` | Os dados ainda não terminaram de carregar, ou o nome do banco ou da coleção está errado. Confira `sample_mflix` e `movies` |
| `KeyError` em `f["title"]` | Algum documento não tem esse campo. Use `print(f)` em vez de `f["title"]` |
| O `find` mostra algo estranho como `<pymongo.cursor.Cursor ...>` | Percorra o resultado com `for f in filmes.find(...):` e use `print` na linha recuada |
| `IndentationError` | A linha depois do `for` precisa estar recuada com 4 espaços |

## Checklist antes de enviar
- [ ] Dados de exemplo carregados e `ping` funcionando
- [ ] Prints dos passos 3 a 8
- [ ] Três perguntas próprias, com comparação e `sort`, e o print
- [ ] Reflexão respondida
- [ ] Nenhuma senha ou connection string escrita no notebook
