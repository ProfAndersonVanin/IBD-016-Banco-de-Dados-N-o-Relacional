# Guia de Estudos — Avaliação 1 (P1)
**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Conteúdo cobrado:** Aulas 1 a 4
**Formato da prova:** 8 questões objetivas (múltipla escolha) + 2 questões dissertativas

Este material reúne, organizado por eixo temático, tudo o que foi visto até a Aula 4 e que pode ser cobrado na prova. Ele não substitui suas anotações de aula nem os slides — é um roteiro para guiar sua revisão e um checklist para você verificar se está pronto.

**Como estudar com este guia:**
1. Leia cada eixo temático e confira se você consegue explicar os pontos-chave **com suas próprias palavras**, sem olhar o material.
2. Responda as perguntas de autoavaliação de cada seção antes de seguir para a próxima.
3. Nos dois dias antes da prova, refaça o checklist final sem consultar nada — o que você não conseguir marcar é onde focar sua revisão.

---

## Eixo 1 — Dados estruturados, semiestruturados e não estruturados (Aula 1)

**Pontos-chave:**
- **Estruturados:** esquema fixo, definido antes da inserção dos dados (ex.: tabela relacional).
- **Semiestruturados:** têm alguma organização (tags, hierarquia), mas sem esquema rígido (ex.: JSON, XML).
- **Não estruturados:** sem modelo predefinido (ex.: imagens, vídeos, texto livre).
- Estimativas de mercado indicam que **80–90% dos dados gerados hoje são não estruturados** — um dos motivadores da adoção do NoSQL.

**Autoavaliação:**
- Você consegue dar um exemplo do dia a dia para cada uma das três categorias, diferente dos exemplos vistos em aula?
- Você sabe explicar por que um arquivo JSON com campos variáveis é semiestruturado, e não estruturado?

---

## Eixo 2 — Big Data: os 5 V's (Aula 1)

**Pontos-chave (memorize o que cada "V" significa, não decore frases prontas):**

| V | Significa |
|---|---|
| Volume | Crescimento exponencial da quantidade de dados gerados |
| Velocidade | Dados gerados/processados quase em tempo real |
| Variedade | Múltiplos formatos e fontes coexistindo |
| Veracidade | Qualidade e confiabilidade do dado nem sempre garantidas na origem |
| Valor | O dado só é útil se transformado em informação/decisão |

**Autoavaliação:**
- Se alguém confundir "Volume" com "Variedade", você consegue explicar a diferença com um exemplo?
- Você sabe qual "V" foi acrescentado por último na formulação original do conceito (dica: os 3 primeiros foram Volume, Velocidade e Variedade)?

---

## Eixo 3 — Limitações do modelo relacional e ACID x BASE (Aula 2)

**Pontos-chave — limitações do relacional:**
- **Rigidez de esquema** (schema-on-write): alterar tabelas em produção é custoso.
- **Escalabilidade vertical x horizontal:** relacional tradicionalmente escala "para cima" (mais hardware em uma máquina); NoSQL nasce pensado para escalar "para os lados" (mais máquinas).
- **Custo de JOINs** em grande escala e distribuídos.
- **Impedância objeto-relacional:** objetos aninhados da aplicação exigem várias tabelas + ORM.

**Pontos-chave — ACID x BASE:**

| ACID | BASE |
|---|---|
| Atomicidade, Consistência, Isolamento, Durabilidade | Basically Available, Soft state, Eventually consistent |
| Consistência forte, imediata | Consistência eventual |
| Padrão do modelo relacional | Comum em muitos bancos NoSQL |

**Autoavaliação:**
- Você consegue explicar, em uma frase, cada uma das quatro letras de ACID?
- Dado um cenário (ex.: sistema bancário x contador de curtidas), você consegue justificar qual dos dois modelos (ACID ou BASE) faz mais sentido, e por quê?

---

## Eixo 4 — Teorema CAP (Aula 2)

**Este é um tema que pode aparecer tanto na parte objetiva quanto na dissertativa — revise com atenção.**

**Pontos-chave:**
- **C — Consistency (Consistência):** todos os nós enxergam o mesmo dado ao mesmo tempo.
- **A — Availability (Disponibilidade):** todo pedido recebe uma resposta, mesmo diante de falhas parciais.
- **P — Partition Tolerance (Tolerância a Partição):** o sistema continua funcionando mesmo com falha de comunicação entre nós.
- O teorema diz que, diante de uma partição de rede, o sistema precisa escolher entre **C** ou **A** — não é possível garantir os três ao mesmo tempo.
- **Sistemas CP:** priorizam recusar uma resposta a entregar dado desatualizado (ex.: sistemas bancários, reservas de assento).
- **Sistemas AP:** priorizam sempre responder, mesmo com dado eventualmente consistente (ex.: contador de curtidas, carrinho de compras, cache de sessão).

**Dica para a dissertativa:** ao explicar o CAP, não basta decorar as siglas — pratique explicar **por que** a escolha entre C e A só é necessária quando existe uma partição de rede, e prepare de cor um exemplo de sistema CP e um exemplo de sistema AP, com a justificativa de por que aquele sistema fez aquela escolha.

**Autoavaliação:**
- Você consegue explicar o CAP sem olhar suas anotações, em até 3 frases?
- Você tem um exemplo de sistema CP e um de sistema AP prontos, com justificativa?

---

## Eixo 5 — Categorias NoSQL e Polyglot Persistence (Aula 2)

**Pontos-chave — as quatro categorias:**

| Categoria | Estrutura | Caso de uso típico |
|---|---|---|
| Chave-valor | par chave → valor | cache, sessão, contadores |
| Documentos | JSON/BSON aninhado | catálogos, perfis, CMS |
| Colunas | famílias de colunas distribuídas | séries temporais, IoT |
| Grafos | nós e arestas | redes sociais, recomendação |

- **Polyglot persistence:** combinar mais de uma categoria de banco (NoSQL e/ou relacional) na mesma aplicação, conforme a necessidade de cada tipo de dado.

**Autoavaliação:**
- Dado um sistema com necessidades variadas (ex.: rede social), você consegue identificar qual categoria atenderia melhor cada parte do sistema?
- Você sabe explicar, com suas palavras, o que é polyglot persistence e por que uma aplicação real usaria mais de uma categoria de banco?

---

## Eixo 6 — Banco de dados chave-valor: conceito e arquitetura (Aula 3)

**Pontos-chave:**
- Modelo mais simples do NoSQL: par único (chave → valor), sem esquema, sem linguagem de consulta complexa.
- Consulta só é eficiente **pela chave** — buscar por conteúdo do valor exige varrer tudo ou manter índices externos.
- **Arquitetura interna:** tabela hash (acesso O(1)), armazenamento in-memory (por isso é tão rápido), particionamento (sharding), replicação primário-réplica, persistência em disco (RDB = snapshot, AOF = log de escrita), TTL (tempo de vida das chaves).
- **Anatomia de uma chave:** convenção `recurso:id` (ex.: `produto:101`) — mantém o banco organizado e escalável.

**Autoavaliação:**
- Você sabe explicar por que um banco chave-valor consegue responder em O(1)?
- Você sabe a diferença entre RDB e AOF como mecanismos de persistência?
- Você consegue montar uma chave seguindo o padrão `recurso:id` para um cenário novo (ex.: um pedido de compra)?

---

## Eixo 7 — Redis na prática: estruturas de dados e comandos (Aula 3)

**Pontos-chave — estruturas de dados do Redis:**

| Estrutura | O que é | Caso de uso |
|---|---|---|
| String | texto, número ou binário | cache simples, contadores |
| List | coleção ordenada, permite repetição | filas |
| Hash | pares campo → valor dentro de uma chave | representar um objeto |
| Set | coleção não ordenada, **sem repetição** | tags, relações |
| Sorted Set | como o Set, mas com pontuação (score) | rankings, filas de prioridade |

**Pontos-chave — comandos fundamentais (saiba o que cada um faz):**

| Comando | Função |
|---|---|
| `SET` / `GET` | grava / lê um valor simples |
| `DEL` | remove uma chave |
| `EXISTS` | verifica se uma chave existe |
| `EXPIRE` / `TTL` | define / consulta o tempo de vida de uma chave |
| `INCR` | incrementa um valor numérico atomicamente |
| `HSET` / `HGET` / `HGETALL` | grava / lê um campo / lê todos os campos de um hash |

**Atenção especial:** entenda bem o que acontece com uma chave **depois** que o TTL expira (ela é removida automaticamente pelo Redis, sem qualquer ação da aplicação) — isso já foi tema de exercício em aula.

**Autoavaliação:**
- Para cada estrutura de dado da tabela acima, você consegue dar um caso de uso diferente dos exemplos vistos em aula?
- Você sabe prever o resultado de uma sequência de comandos (ex.: `SET` seguido de `EXPIRE` e, depois do tempo passar, um `GET`)?

---

## Eixo 8 — Redis aplicado: cache, sessões e rate limiting (Aula 4)

**Este eixo é a base da segunda questão dissertativa — revise com atenção.**

**Padrões de cache:**

| Padrão | Como funciona | Trade-off |
|---|---|---|
| Cache-aside (lazy loading) | Aplicação consulta o cache primeiro; se não encontrar, busca na fonte e grava no cache | Primeira consulta de cada chave é mais lenta |
| Write-through | Toda escrita vai para o cache **e** para o banco principal ao mesmo tempo | Cache nunca desatualiza, mas escrita é mais lenta |
| Write-behind | Escreve no cache primeiro; sincroniza com o banco principal depois, de forma assíncrona | Escrita rápida, mas risco de perda de dados |

- **Cache stampede:** quando uma chave muito acessada expira, várias requisições tentam recalculá-la ao mesmo tempo — mitigado com locks ou *jitter* (variação aleatória no TTL).

**Sessões distribuídas:**
- Problema: sessão guardada na memória de um único servidor "some" se o balanceador de carga rotear para outro servidor.
- Solução: centralizar as sessões em um Redis compartilhado, acessível por todos os servidores.
- Boas práticas: representar a sessão como **hash**, sempre definir **TTL**, e renovar o TTL a cada requisição do usuário ativo (*sliding expiration*).

**Rate limiting:**
- Padrão fixed window: usar `INCR` sobre uma chave que representa a janela de tempo atual, e `EXPIRE` para que a chave expire ao final da janela.

**Autoavaliação:**
- Você consegue explicar a diferença entre cache-aside, write-through e write-behind, e dizer quando cada um faz mais sentido?
- Você sabe explicar, com suas palavras, por que centralizar sessões em um Redis compartilhado resolve o problema de múltiplos servidores?
- Você consegue montar (mentalmente ou no papel) a lógica de um rate limiter simples com `INCR` e `EXPIRE`?

---

## Sobre o formato da prova

- **8 questões objetivas**, uma alternativa correta cada, cobrindo os 8 eixos temáticos acima.
- **2 questões dissertativas**, exigindo explicação com suas próprias palavras — uma delas envolve diretamente o Teorema CAP (Eixo 4), e a outra pede para você aplicar conceitos de Redis (cache e sessão) a um cenário prático (Eixos 6, 7 e 8).
- Não é uma prova de decoreba: as questões objetivas apresentam cenários e pedem para você aplicar o conceito, não apenas repetir uma definição.

---

## Checklist final (faça sem consultar nada)

- [ ] Sei diferenciar dados estruturados, semiestruturados e não estruturados, com exemplos próprios
- [ ] Sei explicar os 5 V's do Big Data
- [ ] Sei explicar pelo menos duas limitações do modelo relacional
- [ ] Sei diferenciar ACID de BASE
- [ ] Sei explicar o Teorema CAP e tenho um exemplo de sistema CP e um de sistema AP prontos
- [ ] Sei listar as quatro categorias NoSQL e um caso de uso para cada
- [ ] Sei explicar o que é polyglot persistence
- [ ] Sei explicar por que um banco chave-valor é tão rápido (arquitetura interna)
- [ ] Sei explicar a convenção de nomenclatura de chaves (`recurso:id`)
- [ ] Sei listar as cinco estruturas de dados do Redis e um caso de uso para cada
- [ ] Sei o que cada comando fundamental do Redis faz (SET, GET, DEL, EXISTS, EXPIRE, TTL, INCR, HSET, HGET, HGETALL)
- [ ] Sei explicar os três padrões de cache (cache-aside, write-through, write-behind)
- [ ] Sei explicar como o Redis resolve o problema de sessões em múltiplos servidores
- [ ] Sei explicar a lógica de um rate limiter simples com Redis

Se todos os itens estiverem marcados, você está pronto para a prova. Bons estudos!
