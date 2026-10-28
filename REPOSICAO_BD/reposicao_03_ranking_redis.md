# Reposição 3 — Ranking de jogadores com Redis (Sorted Set)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Tipo:** atividade prática individual
**Tempo estimado:** 2 horas
**Ferramentas:** Google Colab (Python) e o seu banco do Redis Cloud
**Entrega:** os prints (ou o link do notebook) e a reflexão final, pelo Teams ou conforme combinado com o professor

---

## Objetivo
Praticar o tipo de dado **Sorted Set** do Redis, que guarda itens com uma **pontuação** e os mantém sempre em ordem. É a estrutura ideal para rankings.

## Como funciona (leia antes)
Um **Sorted Set** é um conjunto em que cada item tem:
- um **membro** (o nome do item, como um jogador) — não se repete;
- um **score** (a pontuação, como um número) — o Redis usa o score para manter tudo **ordenado automaticamente**.

Você não precisa ordenar nada: ao pedir o ranking, o Redis já devolve na ordem certa.

**O Python que você vai usar:** variáveis, `print`, dicionários `{"chave": valor}` e listas. Nada além disso.

## Antes de começar
- [ ] Seu banco do Redis Cloud está **ativo** (criado na aula 5).
- [ ] Você tem o **host**, a **porta** e a **senha** dele (no site do Redis Cloud: seu banco → *Configuration* → *Public endpoint*, que mostra o host e a porta separados por `:`).
- [ ] Você tem uma conta Google para abrir o [Google Colab](https://colab.research.google.com/).

---

## Passo a passo

### Passo 1 — Instalar a biblioteca
Em um novo notebook do Colab, na primeira célula:
```python
!pip install redis
```

### Passo 2 — Conectar ao Redis
Troque `COLE_AQUI_O_HOST` pelo host e `12345` pela porta do seu banco. A senha será pedida em uma caixa, e não fica escrita no notebook.
```python
from getpass import getpass
import redis

host = "COLE_AQUI_O_HOST"
porta = 12345
senha = getpass("Senha do Redis: ")

r = redis.Redis(host=host, port=porta, password=senha, ssl=True, decode_responses=True)
print(r.ping())
```
**Resultado esperado:** `True`. Se der erro de conexão, tente trocar `ssl=True` por `ssl=False`.

### Passo 3 — Criar o ranking
Os nomes das chaves desta atividade começam com `rep:`, para não misturar com outros dados seus.
```python
r.delete("rep:ranking")
r.zadd("rep:ranking", {"Ana": 120, "Bruno": 95, "Carla": 150, "Diego": 80})
print("Jogadores no ranking:", r.zcard("rep:ranking"))
```
- `zadd` adiciona membros com seus scores (um dicionário `membro: pontos`).
- `zcard` conta quantos membros existem.

**Resultado esperado:** `Jogadores no ranking: 4`.

### Passo 4 — Ver o pódio (os 3 primeiros)
```python
print(r.zrevrange("rep:ranking", 0, 2, withscores=True))
```
- `zrevrange` devolve do **maior** para o **menor** score. Os números `0` e `2` pedem da posição 0 (1º lugar) até a 2 (3º lugar).
- `withscores=True` mostra também as pontuações.

**Resultado esperado:** `[('Carla', 150.0), ('Ana', 120.0), ('Bruno', 95.0)]`.

### Passo 5 — Somar pontos
O Diego ganhou 50 pontos:
```python
r.zincrby("rep:ranking", 50, "Diego")
print(r.zrevrange("rep:ranking", 0, 3, withscores=True))
```
**Resultado esperado:** `[('Carla', 150.0), ('Diego', 130.0), ('Ana', 120.0), ('Bruno', 95.0)]`. O Diego subiu para o 2º lugar sem que você precisasse reordenar nada.

### Passo 6 — Consultar um jogador
```python
print("Pontos do Bruno:", r.zscore("rep:ranking", "Bruno"))
print("Posição da Ana:", r.zrevrank("rep:ranking", "Ana") + 1)
```
O `zrevrank` conta a partir do zero, por isso somamos 1 para obter a posição de verdade.

**Resultado esperado:** `Pontos do Bruno: 95.0` e `Posição da Ana: 3`.

### Passo 7 — Buscar por faixa de pontos
Quem tem entre 100 e 140 pontos?
```python
print(r.zrangebyscore("rep:ranking", 100, 140, withscores=True))
```
**Resultado esperado:** `[('Ana', 120.0), ('Diego', 130.0)]` (aqui a ordem vai do menor para o maior).

### Passo 8 — Remover um jogador
```python
r.zrem("rep:ranking", "Bruno")
print("Restaram:", r.zcard("rep:ranking"))
print(r.zrevrange("rep:ranking", 0, -1, withscores=True))
```
O `-1` significa "até o último". **Resultado esperado:** `Restaram: 3` e a lista sem o Bruno.

**Tire um print das saídas dos passos 3 a 8.**

### Passo 9 — Sua vez: crie o seu próprio ranking
Monte um ranking sobre **algo do seu interesse** (filmes com suas notas, jogos, times, músicas...). Use a chave `rep:meu_ranking` e faça, **sem copiar os dados do exemplo**:
1. Crie o ranking com **pelo menos 5 membros** e seus scores (`zadd`).
2. Mostre os **3 primeiros** (`zrevrange`).
3. **Some pontos** a um dos membros e mostre o ranking de novo (`zincrby`).
4. Mostre a **posição** de um membro (`zrevrank`).

**Tire um print do seu ranking.**

### Passo 10 — Limpar o que você criou
```python
r.delete("rep:ranking", "rep:meu_ranking")
print(r.exists("rep:ranking"), r.exists("rep:meu_ranking"))
```
**Resultado esperado:** `0 0`.

### Passo 11 — Reflexão (em texto)
Responda em 3 a 5 linhas:
1. Por que um Sorted Set é melhor para um ranking do que uma List ou um Hash?
2. Cite **uma situação real** em que um ranking assim seria útil. O que seria o "membro" e o que seria o "score"?

---

## Como será avaliado (10 pontos)
| Critério | Pontos |
|---|---|
| Passos 1 a 8 executados, com os prints | 4,0 |
| Seu próprio ranking (passo 9), com dados diferentes do exemplo | 3,0 |
| Limpeza das chaves (passo 10) | 1,0 |
| Reflexão (passo 11) | 2,0 |

## Problemas comuns
| Sintoma | O que fazer |
|---|---|
| `r.ping()` dá erro ou não responde | Confira host, porta e senha; teste `ssl=False`; veja se o banco está ativo no Redis Cloud |
| `NameError: name 'r' is not defined` | Execute de novo a célula do Passo 2 (a sessão do Colab pode ter reiniciado) |
| `ModuleNotFoundError: No module named 'redis'` | Execute a célula do Passo 1 |
| Resultado diferente do esperado | Você pode ter rodado a célula do Passo 3 ou do Passo 5 mais de uma vez. Rode o `r.delete("rep:ranking")` e refaça a partir do Passo 3 |

## Checklist antes de enviar
- [ ] Prints dos passos 3 a 8
- [ ] Meu ranking com 5 membros ou mais, com os 4 comandos do passo 9
- [ ] Chaves apagadas no passo 10
- [ ] Reflexão respondida
- [ ] Nenhuma senha escrita no notebook
