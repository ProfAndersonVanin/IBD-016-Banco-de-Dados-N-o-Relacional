# Atividades de reposição — visão geral e gabarito (uso do docente)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Quantidade:** 5 atividades individuais, cada uma em um arquivo pronto para entregar ao aluno
**Atenção:** este arquivo contém respostas e orientações de correção. **Não envie aos alunos.**

---

## 1. Resumo das atividades

| # | Arquivo | Atividade | Tipo | Ferramenta | Tempo estimado | Conteúdo que complementa |
|---|---|---|---|---|---|---|
| 1 | `reposicao_01_mapa_nosql.md` | Mapa dos bancos NoSQL | Pesquisa | Navegador | 2 a 3 h | Panorama das 4 categorias NoSQL e polyglot persistence |
| 2 | `reposicao_02_cap_acid_base.md` | CAP, ACID e BASE no dia a dia | Pesquisa e raciocínio | Navegador | 2 a 3 h | Teorema CAP, ACID x BASE (ponto fraco da P1) |
| 3 | `reposicao_03_ranking_redis.md` | Ranking de jogadores com Sorted Set | Prática | Colab + Redis Cloud | 2 h | Estruturas de dados do Redis |
| 4 | `reposicao_04_explorando_mongodb.md` | Explorando dados reais (filmes) | Prática | Colab + MongoDB Atlas | 2 a 3 h | Consultas no MongoDB (filtros, listas, objetos, ordenação) |
| 5 | `reposicao_05_modelagem_documentos.md` | Embutir ou referenciar? | Prática no papel | Editor de texto + validador de JSON | 2 a 3 h | Modelagem de documentos |

O tempo de 2 a 3 horas é uma estimativa. Se a coordenação exigir equivalência com a carga horária de uma aula, ajuste conforme a regra da instituição.

## 2. Como aplicar
- Cada arquivo é **autoexplicativo**: traz objetivo, passo a passo, o resultado esperado de cada passo, a rubrica de 10 pontos e um checklist.
- Entrega pelo Teams, ou na pasta do repositório da disciplina, conforme o seu combinado. O prazo é o que você definir.
- Todas as rubricas somam 10 pontos, para você converter na nota de reposição do jeito que preferir.
- As atividades 3 e 4 usam recursos que os alunos já têm (banco do Redis Cloud da aula 5 e cluster do Atlas da aula 9). A atividade 4 exige espaço livre no plano gratuito; o arquivo orienta a carregar só o `sample_mflix`.
- As atividades 1, 2 e 5 são textuais. Vale comparar as entregas entre si, para detectar respostas muito parecidas. A política de uso de ferramentas de IA fica a seu critério: nenhum dos arquivos fixa uma regra, e todos pedem que o aluno escreva com as próprias palavras.

---

## 3. Gabarito e orientações de correção

### Atividade 1 — Mapa dos bancos NoSQL
- **Tabela:** confira se os 4 bancos são de categorias diferentes e se pelo menos 2 estão fora de Redis, MongoDB, Cassandra e Neo4j. Bancos multimodelo (por exemplo, o DynamoDB) devem ser aceitos se o aluno justificar a categoria.
- **Posição no DB-Engines:** varia com o tempo, então basta constar uma posição e a data da consulta.
- **Passo 3 (delivery)** — resposta esperada, aceitando alternativas bem justificadas:

| Necessidade | Categoria esperada | Exemplos |
|---|---|---|
| a) Cache do cardápio | Chave-valor | Redis, Memcached |
| b) Cadastro de restaurantes com campos variáveis | Documentos | MongoDB, CouchDB, Firestore |
| c) Milhões de posições de GPS, muita escrita | Colunas | Cassandra, ScyllaDB |
| d) Recomendação "quem pediu isto também pediu" | Grafos | Neo4j, Neptune |

### Atividade 2 — CAP, ACID e BASE
- **Definições:** devem ser coerentes com C (todos veem o mesmo dado), A (toda requisição recebe resposta) e P (funciona mesmo com falha entre os nós), mais a regra de escolher entre C e A diante de uma partição.
- **Analogia:** deve explicar o trade-off sem usar banco nem rede social.
- **Classificação (passo 3):**

| # | Sistema | Resposta esperada | Observação |
|---|---|---|---|
| 1 | Transferência de dinheiro | CP | Prefere recusar a arriscar saldo errado |
| 2 | Curtidas | AP | Número aproximado é aceitável |
| 3 | Reserva de assento em voo | CP | Evitar vender o mesmo assento. Aceitar AP se o aluno justificar, lembrando que companhias aéreas vendem acima da capacidade (overbooking) e compensam depois |
| 4 | Carrinho de compras | AP | Exemplo clássico do artigo do Dynamo (Amazon, 2007): é melhor aceitar o item do que impedir a compra |
| 5 | Placar ao vivo | AP | Atraso pequeno é aceitável |
| 6 | Permissões de acesso | CP | Permissão desatualizada é risco de segurança |

- **ACID x BASE (passo 4):** ACID garante consistência forte e sacrifica disponibilidade e desempenho em escala (ex.: sistema bancário). BASE garante disponibilidade e desempenho e sacrifica a consistência imediata (ex.: curtidas, feed).
- **Passo 5:** qualquer banco com fonte citada serve. Referências comuns: Cassandra (AP, ajustável), MongoDB (CP por padrão), DynamoDB (ajustável).
- **Reflexão (passo 6):** esperada a ideia de que um mesmo sistema mistura os dois, por exemplo: pagamento e baixa de estoque como CP; vitrine, avaliações e carrinho como AP.

### Atividade 3 — Ranking com Sorted Set
Saídas esperadas, na ordem dos passos:

| Passo | Saída |
|---|---|
| 2 | `True` |
| 3 | `Jogadores no ranking: 4` |
| 4 | `[('Carla', 150.0), ('Ana', 120.0), ('Bruno', 95.0)]` |
| 5 | `[('Carla', 150.0), ('Diego', 130.0), ('Ana', 120.0), ('Bruno', 95.0)]` |
| 6 | `Pontos do Bruno: 95.0` e `Posição da Ana: 3` |
| 7 | `[('Ana', 120.0), ('Diego', 130.0)]` |
| 8 | `Restaram: 3` |
| 10 | `0 0` |

- **Erros frequentes:** esquecer o `+ 1` na posição, rodar o passo 3 ou 5 mais de uma vez (a pontuação do Diego passa de 130) e deixar a senha escrita no notebook.
- **Passo 9:** o ranking próprio deve ter 5 membros ou mais e usar `zadd`, `zrevrange`, `zincrby` e `zrevrank`, com dados diferentes do exemplo.
- **Reflexão:** a List não ordena por pontuação e o Hash não mantém ordem; o Sorted Set mantém os itens ordenados pelo score, sem trabalho extra.

### Atividade 4 — Explorando dados reais
- Os resultados dependem do conjunto de dados do Atlas, então **os números exatos podem variar**. Verifique se o aluno executou os passos 3 a 8 e se há prints.
- **Passo 9:** as três perguntas devem ser próprias, com pelo menos um operador de comparação e uma consulta com `sort`.
- **Reflexão 1 (relacional):** `genres` e `cast` virariam tabelas associativas (por exemplo, `filme_genero` e `filme_ator`), com JOINs para remontar o filme.
- **Reflexão 2:** nem todos os filmes têm os mesmos campos; o modelo de documentos aceita isso sem migração.
- O aluno **não deve alterar nem apagar** os dados de exemplo.

### Atividade 5 — Modelagem de documentos
Resolução de referência. Aceite variações coerentes e bem justificadas.

**Cenário 1 — Blog**
- Coleção `posts`: título, texto, data, `tags` (lista embutida) e `autor` embutido (nome e e-mail). Se o autor tiver perfil próprio e puder mudar, é válido referenciar com `autor_id`.
- Coleção `comentarios`: `post_id`, nome, texto e data. **Referenciar**, porque podem ser milhares por post (array ilimitado). Variação válida: guardar também no post um contador e os últimos 3 comentários.
- Pergunta fácil: abrir um post com seu autor. Difícil: listar todos os comentários de um autor em todos os posts.

**Cenário 2 — Escola**
- Coleção `alunos`: `endereco` (objeto embutido), `telefones` (lista embutida) e `historico` (lista de objetos `{disciplina, ano, nota}` embutida, pois é limitada pelo tamanho do curso).
- Variação válida: coleção `matriculas`, se o aluno justificar a necessidade de consultar por disciplina.
- Pergunta fácil: mostrar o histórico de um aluno. Difícil: média de notas de uma disciplina em todos os alunos.

**Cenário 3 — Playlists (N:N)**
- Coleção `musicas`: título, duração e `artista` embutido (nome e país).
- Coleção `playlists`: nome, dono e `musicas`, uma lista de identificadores (`musica_id`), **referenciados**, porque a mesma música aparece em várias playlists. A ordem pode ser a própria posição na lista.
- Pergunta fácil: abrir uma playlist e buscar suas músicas. Difícil: saber em quantas playlists uma música aparece.

**Validação de JSON (passo final):** exija a frase do aluno dizendo se o JSON foi válido de primeira ou o que ele corrigiu. Erros comuns: vírgula sobrando, aspas simples, `true` ou `false` entre aspas.

---

## 4. O que foi verificado
- Os códigos das atividades 3 e 4 foram executados em simuladores de Redis e MongoDB, sem erros, e as saídas esperadas da atividade 3 conferem. No teste da atividade 4, os filtros `$gt` e `$gte` ignoraram corretamente documentos com campos ausentes ou do tipo errado.
- Os nomes dos campos do conjunto `sample_mflix` (`title`, `year`, `genres`, `cast`, `runtime`, `imdb.rating`, `countries`) e o caminho para carregar os dados de exemplo foram conferidos na documentação do MongoDB.
- Não foi possível testar contra um Atlas real: os nomes dos botões do Atlas podem mudar, e os resultados da atividade 4 dependem do conjunto de dados carregado.
