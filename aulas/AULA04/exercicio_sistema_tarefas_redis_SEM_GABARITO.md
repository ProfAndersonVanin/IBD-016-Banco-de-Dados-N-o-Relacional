# Exercício Guiado — Sistema de Tarefas com Redis (Google Colab)
### Versão sem gabarito — este arquivo indica o que fazer, não a solução pronta

**Disciplina:** IBD-016 — Banco de Dados Não Relacional

Este guia foi escrito para quem ainda está começando a programar. Ele **não mostra o código pronto** — em cada passo, você vai saber qual comando do Redis usar e o que ele precisa receber, mas a construção da linha de código é com você. Use os comandos vistos em aula e no `passo_a_passo_redis_cloud.md` como referência de sintaxe.

Vá com calma, teste um passo de cada vez, e confira sempre a saída antes de seguir para o próximo.

---

## Antes de começar: alguns termos que vamos usar

- **Célula (cell):** cada "bloco" de código dentro do Google Colab, executado separadamente.
- **Executar uma célula:** clicar no botão de "play" (▶) ao lado da célula, ou `Shift + Enter`.
- **Biblioteca:** um "pacote de ferramentas prontas" que outras pessoas programaram. Vamos usar a biblioteca `redis`.
- **Lista (list):** uma coleção de itens em ordem, como uma lista de compras.
- **Dicionário (dict):** uma coleção de pares "nome do campo: valor", como uma ficha de cadastro. Em Python, aparece entre chaves `{ }`.
- **Conjunto (set):** parecido com uma lista, mas **não permite itens repetidos** e não tem ordem garantida.
- **TTL (Time To Live):** o "tempo de vida" de uma informação — depois desse tempo, ela desaparece sozinha.

---

## Pré-requisitos

- [ ] Conta gratuita no Redis Cloud, com um banco criado e com status **Active**.
- [ ] Host, porta e senha desse banco anotados (tela "Configuration" do seu banco).
- [ ] Conta Google, para acessar o Google Colab.

---

## Passo 0 — Preparar o ambiente no Google Colab

**O que fazer:**
1. Acesse o Google Colab e crie um novo notebook.
2. Na primeira célula, instale a biblioteca do Redis para Python (o nome do pacote é `redis`).
3. Em uma nova célula, importe a biblioteca e crie o objeto de conexão, usando seu host, porta e senha. Não esqueça dos parâmetros que indicam que a conexão usa criptografia (TLS) e que as respostas devem vir como texto legível, não como bytes.
4. Teste se a conexão funcionou usando o comando que "pergunta" ao servidor se ele está disponível.

**Comandos/conceitos envolvidos:** instalação via `pip`, criação do objeto de conexão (`redis.Redis(...)`), comando de teste `PING`.

**Como saber se deu certo:** o teste de conexão deve responder com `True`.

---

## Passo 1 — Cadastrar três tarefas em uma lista

**O que fazer:** crie uma **lista** chamada `aula:tarefas` e adicione três tarefas de sua escolha dentro dela. Depois, leia a lista inteira para conferir.

**Comando para adicionar itens ao final de uma lista:** `RPUSH`
**Comando para ler um intervalo de itens de uma lista:** `LRANGE`

**Dica:** o comando de leitura de listas pede uma posição inicial e uma posição final. Para ler a lista inteira, existe uma forma de indicar "do começo até o fim" sem saber quantos itens ela tem — pesquise como indicar "o último item" em uma lista no Redis.

**Como saber se deu certo:** a leitura deve mostrar as três tarefas que você cadastrou, na ordem em que foram adicionadas.

---

## Passo 2 — Criar um hash para uma tarefa

**O que fazer:** crie um **hash** (uma "ficha" de uma única tarefa) com os campos `titulo`, `prioridade` e `responsavel` preenchidos. Sugestão de nome de chave: `aula:tarefa:1`.

**Comando para criar/atualizar um hash com vários campos de uma vez:** `HSET`
**Comando para ler todos os campos de um hash:** `HGETALL`

**Dica:** o `HSET` em Python aceita um parâmetro especial para receber vários campos de uma vez, no formato de um dicionário Python (`{"campo": "valor", ...}`). Pesquise o nome desse parâmetro na documentação da biblioteca `redis` para Python.

**Como saber se deu certo:** a leitura deve mostrar um dicionário com os três campos e os valores que você definiu.

---

## Passo 3 — Criar um conjunto de tags (com uma repetida de propósito)

**O que fazer:** crie um **conjunto** chamado `aula:tags`, com pelo menos quatro tags — repita uma delas de propósito.

**Comando para adicionar itens a um conjunto:** `SADD`
**Comando para ler todos os itens de um conjunto:** `SMEMBERS`

**Pergunta para você responder antes de rodar:** quantos itens você acha que vão aparecer na leitura, já que uma das tags foi repetida? Rode o código e confirme sua previsão.

**Como saber se deu certo:** a tag repetida deve aparecer **apenas uma vez** no resultado.

---

## Passo 4 — Registrar a quantidade de tarefas concluídas

**O que fazer:** simule que duas tarefas foram concluídas, aumentando um contador chamado `aula:tarefas_concluidas` a cada conclusão.

**Comando para incrementar um valor numérico:** `INCR`
**Comando para ler o valor atual de uma chave simples:** `GET`

**Dica:** você não precisa criar essa chave com um valor inicial antes — o comando de incremento cria a chave sozinho, já começando do zero, se ela ainda não existir.

**Como saber se deu certo:** depois de "concluir" duas tarefas, o valor lido deve ser `2`.

---

## Passo 5 — Criar uma chave de cache com expiração de 5 minutos

**O que fazer:** crie uma chave chamada `aula:cache_resumo` com um valor de texto qualquer, e configure para que ela expire sozinha em 5 minutos (300 segundos).

**Comando para gravar um valor simples:** `SET`
**Comando para definir o tempo de vida de uma chave já existente:** `EXPIRE`
**Comando para consultar quanto tempo falta para uma chave expirar:** `TTL`

**Dica:** existe também um comando que já cria a chave **e** define o tempo de expiração em um único passo — se quiser, pesquise por ele como alternativa a usar `SET` e `EXPIRE` separadamente.

**Como saber se deu certo:** a consulta do tempo restante deve mostrar um número próximo de 300, que vai diminuindo a cada nova consulta.

---

## Passo 6 — Consultar e exibir todos os dados criados

**O que fazer:** em uma única célula, leia e exiba (com `print`) os dados de **todas** as estruturas criadas nos passos anteriores: a lista, o hash, o conjunto, o contador e a chave de cache (incluindo seu tempo restante).

**Comandos envolvidos:** um comando de leitura para cada tipo de estrutura — reveja os passos anteriores para lembrar qual comando lê cada tipo (lista, hash, conjunto, valor simples, tempo de expiração).

**Dica:** organize a saída com algum texto identificando cada bloco (por exemplo, um título antes de cada `print`), para facilitar a leitura de quem for conferir sua atividade.

**Como saber se deu certo:** todos os dados que você criou nos passos 1 a 5 devem aparecer juntos, de forma organizada, nesta única célula.

---

## Passo 7 — Remover todas as chaves criadas (limpeza)

**O que fazer:** encontre todas as chaves que começam com `aula:` de uma vez, e apague todas elas.

**Comando para buscar chaves que seguem um padrão de nome:** `KEYS` (usando um "curinga" para representar "qualquer coisa depois de `aula:`")
**Comando para apagar uma ou mais chaves:** `DEL`

**Dica:** o comando de busca por padrão retorna uma **lista** de nomes de chaves. O comando de apagar pode receber vários nomes de uma vez — mas, em Python, é preciso "espalhar" os itens da lista como argumentos separados. Pesquise como fazer isso usando um único caractere antes do nome da variável.

**Como saber se deu certo:** depois de apagar, repita a busca por chaves com o padrão `aula:*` — o resultado deve vir vazio.

---

## Conclusão

Se você completou os 7 passos, praticou String, List, Hash, Set, contador (`INCR`) e expiração (`EXPIRE`/`TTL`) — os principais blocos de construção de qualquer aplicação que usa Redis.

**Desafio opcional:** antes do Passo 7, crie uma segunda tarefa como hash (`aula:tarefa:2`) e adicione mais uma tag ao conjunto. Rode o Passo 6 de novo para ver os dados novos aparecerem.

---

## O que fazer se der erro

| Sintoma | Possível causa | O que revisar |
|---|---|---|
| Erro dizendo que uma variável de conexão não existe | Célula de conexão do Passo 0 não foi executada nesta sessão | Execute novamente as células do Passo 0, em ordem |
| Teste de conexão não retorna `True` | Host, porta ou senha incorretos | Revise os dados copiados da tela "Configuration" do Redis Cloud |
| Erro dizendo que o pacote `redis` não foi encontrado | Célula de instalação não foi executada, ou o ambiente reiniciou | Execute novamente a célula de instalação (`pip install`) |
| Leituras do Passo 6 vêm vazias | Algum passo anterior não foi executado, ou o nome da chave foi digitado de forma diferente em algum lugar | Confira se os nomes das chaves são exatamente iguais em todos os passos onde aparecem |
| Depois do Passo 7, ainda aparecem chaves | Os passos 1 a 5 foram executados de novo após a limpeza | Rode o Passo 7 mais uma vez |

---

## Checklist antes de encerrar

- [ ] Consegui conectar ao Redis Cloud (Passo 0)
- [ ] Criei a lista `aula:tarefas` com três tarefas (Passo 1)
- [ ] Criei o hash `aula:tarefa:1` com título, prioridade e responsável (Passo 2)
- [ ] Criei o conjunto `aula:tags` e confirmei minha previsão sobre a tag repetida (Passo 3)
- [ ] Registrei tarefas concluídas com o comando de incremento (Passo 4)
- [ ] Criei uma chave de cache com expiração de 5 minutos (Passo 5)
- [ ] Consultei e exibi todos os dados juntos (Passo 6) — e tirei um print desta tela
- [ ] Removi todas as chaves com o padrão `aula:*` e confirmei que a limpeza funcionou (Passo 7)
