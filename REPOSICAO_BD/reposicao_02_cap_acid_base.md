# Reposição 2 — CAP, ACID e BASE no dia a dia (pesquisa e raciocínio)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Tipo:** atividade individual de pesquisa e raciocínio
**Tempo estimado:** 2 a 3 horas
**Ferramentas:** apenas um navegador e um editor de texto
**Entrega:** um documento de 1 a 2 páginas (Word, Google Docs, PDF ou `.md`), pelo Teams ou conforme combinado com o professor

---

## Objetivo
Fixar o Teorema CAP e a diferença entre ACID e BASE, aplicando as ideias a sistemas que você usa no dia a dia. Esses foram os pontos em que a turma mais teve dificuldade na primeira avaliação, por isso o foco aqui é **explicar com suas palavras**.

## Resumo para consulta
Leia antes de começar. Depois, **escreva as suas próprias versões**, sem copiar.

- **C — Consistency (Consistência):** todos os servidores enxergam o mesmo dado ao mesmo tempo.
- **A — Availability (Disponibilidade):** toda requisição recebe uma resposta, mesmo que alguns servidores tenham falhado.
- **P — Partition tolerance (Tolerância a partição):** o sistema continua funcionando mesmo quando a comunicação entre os servidores falha (a "partição").
- **A regra do teorema:** quando acontece uma partição, o sistema precisa escolher entre **C** (recusar ou atrasar a resposta para não mostrar dado errado) e **A** (responder sempre, mesmo que com dado desatualizado). Um sistema que escolhe consistência é chamado de **CP**; um que escolhe disponibilidade, de **AP**.
- **ACID** (banco relacional tradicional): Atomicidade, Consistência, Isolamento e Durabilidade. Consistência forte e imediata.
- **BASE** (comum em NoSQL): *Basically Available, Soft state, Eventually consistent*. Aceita que os dados fiquem temporariamente diferentes entre os servidores, desde que fiquem iguais com o tempo.

---

## Passo a passo

### Passo 1 — Defina com suas palavras
Para cada letra, escreva **uma ou duas frases**: C, A e P. Depois escreva a regra do teorema em **uma frase**.

### Passo 2 — Uma analogia do cotidiano
Invente uma **analogia** que explique o CAP usando uma situação do dia a dia (2 a 4 linhas). **Não use banco nem rede social.** Ideias para inspirar: duas pessoas mantendo cópias do mesmo caderno sem conseguir se falar; um grupo de WhatsApp quando a internet de alguém cai; duas lojas da mesma rede que dividem um estoque.

### Passo 3 — Classifique 6 sistemas
Copie a tabela, classifique cada sistema como **CP** ou **AP** e responda o restante. Pense em uma situação em que **a rede entre os servidores falhou**.

| # | Sistema | CP ou AP? | Por quê? (1 ou 2 frases) | O que o usuário vê durante a falha? |
|---|---|---|---|---|
| 1 | Transferência de dinheiro entre contas | | | |
| 2 | Contador de curtidas de uma publicação | | | |
| 3 | Reserva de assento em um voo | | | |
| 4 | Carrinho de compras de uma loja virtual | | | |
| 5 | Placar de uma partida de futebol ao vivo em um aplicativo | | | |
| 6 | Controle das permissões de acesso dos funcionários de uma empresa | | | |

### Passo 4 — ACID x BASE
Preencha a tabela com suas palavras:

| | ACID | BASE |
|---|---|---|
| O que garante | | |
| O que sacrifica | | |
| Um sistema onde faz sentido usar | | |

### Passo 5 — Pesquisa rápida
Pesquise na internet **um exemplo real** de banco de dados ou serviço e descubra se ele é descrito como CP, AP ou ajustável. Escreva em 3 a 5 linhas qual banco você escolheu, o que encontrou e **o endereço (link) da fonte**.

### Passo 6 — Reflexão
Responda em 4 a 6 linhas: **um mesmo sistema pode misturar partes CP e partes AP?** Use como exemplo uma loja virtual (pagamento, vitrine de produtos, estoque, avaliações) e diga qual parte faria cada escolha.

---

## Como será avaliado (10 pontos)
| Critério | Pontos |
|---|---|
| Definições corretas e escritas com palavras próprias (passos 1 e 2) | 2,5 |
| Classificação e justificativa dos 6 sistemas (passo 3) | 3,0 |
| Tabela ACID x BASE (passo 4) | 1,5 |
| Pesquisa com fonte (passo 5) | 1,0 |
| Reflexão (passo 6) | 2,0 |

Em alguns cenários mais de uma resposta pode ser aceita, **desde que bem justificada**.

## Checklist antes de enviar
- [ ] Defini C, A e P e a regra do teorema com minhas palavras
- [ ] Criei uma analogia que não usa banco nem rede social
- [ ] Classifiquei os 6 sistemas e respondi as quatro colunas
- [ ] Preenchi a tabela ACID x BASE
- [ ] Citei a fonte da pesquisa
- [ ] Respondi a reflexão
