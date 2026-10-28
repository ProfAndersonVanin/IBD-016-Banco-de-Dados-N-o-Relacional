# Reposição 5 — Modelando documentos: embutir ou referenciar? (prática no papel)

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Tipo:** atividade prática individual, sem programar
**Tempo estimado:** 2 a 3 horas
**Ferramentas:** um editor de texto e um navegador
**Entrega:** um documento com os seus JSONs e as respostas (Word, Google Docs, PDF ou `.md`), pelo Teams ou conforme combinado com o professor

---

## Objetivo
Treinar a decisão mais importante ao modelar para o MongoDB: **embutir** um dado dentro de outro documento ou **referenciá-lo** por um identificador. Você vai modelar três situações e conferir se os seus JSONs estão corretos.

## Antes de começar

### Como escrever um documento em JSON
```json
{
  "titulo": "Bolo de cenoura",
  "tempo_minutos": 50,
  "vegetariana": true,
  "ingredientes": ["cenoura", "ovos", "farinha"],
  "autor": { "nome": "Dona Maria", "cidade": "Mauá" },
  "passos": [
    { "ordem": 1, "texto": "Bata a cenoura com os ovos." },
    { "ordem": 2, "texto": "Misture a farinha e asse." }
  ]
}
```
Regras: nomes dos campos e textos entre **aspas duplas**; números e `true`/`false` **sem aspas**; **vírgula** entre os itens, mas não depois do último; **chaves** `{}` para objetos e **colchetes** `[]` para listas.

### Embutir x referenciar
| | **Embutir** (colocar dentro do documento) | **Referenciar** (guardar só o identificador de outro documento) |
|---|---|---|
| Use quando | os dados são lidos **sempre juntos**, em **pouca quantidade** e não existem sozinhos | a quantidade pode **crescer sem limite**, os dados são **usados em vários lugares** ou **mudam com frequência** |
| Vantagem | uma só consulta traz tudo | evita repetir dados e mantém o documento pequeno |
| Custo | o documento pode ficar grande | é preciso fazer uma segunda consulta |

Para referenciar, o documento guarda o identificador do outro. Por exemplo, `"autor_id": "A01"`.

---

## Passo a passo

Para **cada um dos três cenários** abaixo, faça os itens **a**, **b**, **c** e **d**.

### Cenário 1 — Blog
Um blog tem **posts**. Cada post tem título, texto, data, tags (palavras-chave), um **autor** (nome e e-mail) e **comentários**. Um post popular pode receber **milhares de comentários**; cada comentário tem o nome de quem comentou, o texto e a data.

### Cenário 2 — Escola
Uma escola guarda **alunos**. Cada aluno tem nome, data de nascimento, **endereço** (rua, número, cidade), vários **telefones** e o **histórico das disciplinas cursadas**, com o nome da disciplina, o ano e a nota final.

### Cenário 3 — Playlists de música
Um aplicativo tem **músicas** (título, duração, artista com nome e país) e **playlists** (nome, dono e as músicas que a compõem, em uma ordem). Uma playlist tem muitas músicas, e **a mesma música pode estar em muitas playlists**.

### Os itens de cada cenário
- **a)** Escreva em **JSON** um documento de exemplo de cada coleção que você criar (inclua pelo menos **2 itens** em cada lista que usar).
- **b)** Liste o que você **embutiu** e o que você **referenciou**.
- **c)** **Justifique** cada escolha em uma frase, usando os critérios da tabela.
- **d)** Escreva **uma pergunta fácil** que o seu modelo responde bem e **uma pergunta difícil** (que exigiria mais de uma consulta ou muito trabalho).

> Não existe um único modelo certo. O que conta é que a escolha seja **coerente** e **bem justificada**.

### Passo final — Conferir o JSON
Um JSON com erro de vírgula ou de aspas não funciona no banco. Escolha **um** dos seus documentos de cada cenário e **valide**:
1. Abra um validador de JSON online, como o `jsonlint.com`, cole o documento e confira se aparece a mensagem de que é válido.
2. Se ele apontar um erro, corrija e valide de novo.

Escreva em uma frase se o seu JSON estava válido de primeira ou o que você precisou corrigir.

**Opcional (para quem quiser ir além):** no Atlas, em **Browse Collections**, abra uma coleção, clique em **Insert Document**, mude para a visualização em JSON, cole o seu documento e insira. O rótulo dos botões pode variar.

### Reflexão final
Responda em 3 a 5 linhas: em qual cenário a decisão entre embutir e referenciar foi **mais difícil**, e por quê?

---

## Como será avaliado (10 pontos)
| Critério | Pontos |
|---|---|
| Documentos JSON corretos e coerentes com o cenário (3 cenários) | 3,0 |
| Separação clara entre o que foi embutido e referenciado, com justificativa | 3,0 |
| Perguntas fáceis e difíceis bem escolhidas | 1,5 |
| Validação do JSON | 1,0 |
| Reflexão final | 1,5 |

## Checklist antes de enviar
- [ ] Os três cenários têm os itens a, b, c e d
- [ ] Pelo menos 2 itens em cada lista usada nos exemplos
- [ ] Validei um JSON de cada cenário
- [ ] Respondi a reflexão final
