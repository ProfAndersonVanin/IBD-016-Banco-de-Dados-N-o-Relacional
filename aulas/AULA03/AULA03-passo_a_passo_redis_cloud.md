# Passo a Passo — Parte Prática (Aula 3): Redis Cloud

**Disciplina:** IBD-016 — Banco de Dados Não Relacional
**Referência:** Encontro 5 (Reposição) — Prática chave-valor com Redis Cloud
**Pré-requisito:** Nenhuma instalação local é necessária — todo o processo é feito no navegador, exceto a etapa opcional de linha de comando.

---

## Antes de começar — checklist de pré-requisitos

- [ ] Navegador atualizado (Chrome, Firefox ou Edge)
- [ ] Um e-mail válido para cadastro (ou conta Google/GitHub, se preferir login social)
- [ ] Não é necessário cartão de crédito em nenhuma etapa
- [ ] Opcional: terminal do sistema operacional, caso queira usar `redis-cli` em vez da interface visual
- [ ] Opcional (sem instalar nada): conta Google, caso prefira usar o Google Colab (ver Opção C da Parte 4)

---

## Parte 1 — Criar conta gratuita no Redis Cloud

1. **Acesse** [https://redis.io/try-free/](https://redis.io/try-free/) (ou `redis.io/cloud`).
2. Clique no botão **"Try Redis Cloud free"** (ou equivalente, como **"Get started free"**).
3. Na tela de cadastro, escolha uma das opções:
   - Preencher **e-mail** e **senha** manualmente; ou
   - Clicar em **"Continue with Google"** / **"Continue with GitHub"** para login social (mais rápido).
4. Se cadastrou por e-mail, verifique sua caixa de entrada e clique no **link de confirmação** enviado pelo Redis.
5. Após confirmar, você será redirecionado ao **console do Redis Cloud** (`app.redislabs.com` ou `cloud.redis.io`).
6. Na primeira vez, o sistema pode pedir para preencher um breve formulário (nome, finalidade de uso) — selecione algo como **"Learning / Personal project"**.

> **Ponto de atenção:** em nenhum momento dessa etapa será solicitado cartão de crédito. Se a tela pedir dados de pagamento, você está no fluxo errado (plano pago) — volte e procure a opção **"Free"** ou **"Essentials Free"**.

---

## Parte 2 — Criar seu primeiro banco de dados gratuito

1. No painel principal (dashboard), clique em **"New database"** (Novo banco de dados) ou **"Create database"**.
2. Na tela **"Create database"**, em **Type of subscription**, selecione a opção gratuita:
   - **"Try 30 MB for free"** ou **"Essentials — Free"** (nomenclatura pode variar ligeiramente conforme atualizações da interface).
3. Preencha os campos que aparecerem:
   - **Database name:** o Redis gera um nome automático (ex.: `database-1`) — você pode renomear para algo como `ibd016-aula3`.
   - **Database version:** deixe a versão padrão sugerida (mais recente estável).
   - **Cloud vendor:** escolha entre **AWS**, **Google Cloud** ou **Microsoft Azure** — para fins didáticos, qualquer um serve; AWS costuma ter mais regiões no Brasil/América do Sul.
   - **Region:** escolha a região mais próxima da sua localização (ex.: `sa-east-1` — São Paulo, se disponível) para reduzir a latência.
4. Revise as informações e clique em **"Create database"**.
5. Aguarde alguns segundos — o status do banco muda de **"Pending"** para **"Active"** automaticamente.
6. Quando o status estiver **Active**, clique no **nome do banco** para abrir a tela de detalhes.

---

## Parte 3 — Obter as credenciais de conexão

Na tela de detalhes do banco, localize a seção **"Configuration"** ou **"Connect"**:

1. **Public endpoint**: algo no formato
   ```
   redis-12345.c1.sa-east-1-1.ec2.cloud.redislabs.com:12345
   ```
   Esse valor combina o **host** e a **porta** (após os dois-pontos).
2. **Default user password**: clique no ícone de "olho" ou **"Show"** para revelar a senha gerada automaticamente. Copie e guarde-a — ela não é reenviada por e-mail.
3. Verifique se a opção **TLS/SSL** está habilitada ou não (indicado na própria tela). Isso influencia como você vai se conectar na Parte 4.
4. Anote os três dados em um bloco de notas temporário:
   ```
   HOST: redis-12345.c1.sa-east-1-1.ec2.cloud.redislabs.com
   PORT: 12345
   PASSWORD: ************
   ```

---

## Parte 4 — Conectando ao banco (três opções)

### Opção A — RedisInsight (interface visual, recomendada para iniciantes)

> **Atenção:** o console do Redis Cloud pode oferecer um botão do tipo **"Connect with RedisInsight" / "Open with RedisInsight Web"** que abre uma versão web embutida direto no navegador. Essa versão está em **rollout gradual** e pode não estar disponível para o seu banco, aparecendo a mensagem *"RedisInsight Web is not yet available in your selected region, tier, or configuration"*. Isso não significa que algo deu errado — é só uma limitação temporária dessa funcionalidade específica. Nesse caso, use o **RedisInsight desktop** (instruções abaixo), que funciona para qualquer banco, independente de região ou tier.

1. Baixe o RedisInsight desktop em [https://redis.io/insight/](https://redis.io/insight/) (disponível para Windows, macOS ou Linux).
2. Instale e abra o aplicativo.
3. Clique em **"Add Redis database"**.
4. Preencha **Host**, **Port** e **Password** com os dados anotados na Parte 3.
5. Dê um apelido em **Database alias** (ex.: `IBD-016 Aula 3`).
6. Se o banco exigir TLS, marque a opção **"Use TLS"** e clique em **"Test Connection"** antes de salvar.
7. Clique em **"Add Redis Database"** para salvar e conectar.
8. Após conectado, você verá o painel do RedisInsight com abas como **Browser**, **CLI**, **Workbench** — a aba **CLI** (ou **Workbench**) é onde os comandos da Parte 5 serão digitados, e a aba **Browser** permite visualizar as chaves gravadas (inclusive as criadas via Google Colab, Opção C).

### Opção B — Terminal com `redis-cli`

1. Verifique se o `redis-cli` está instalado (`redis-cli --version` no terminal). Caso não esteja, instale o pacote `redis-tools` (Linux) ou `redis` via Homebrew (macOS).
2. Se o banco **não exigir TLS**, conecte com:
   ```
   redis-cli -h SEU_HOST -p SUA_PORTA -a SUA_SENHA
   ```
3. Se o banco **exigir TLS**, baixe o certificado indicado na tela **Security** do console e conecte com:
   ```
   redis-cli --tls --cacert redis_ca.pem -h SEU_HOST -p SUA_PORTA -a SUA_SENHA
   ```
4. Se a conexão for bem-sucedida, o prompt mudará para algo como:
   ```
   redis-12345.c1.sa-east-1-1.ec2.cloud.redislabs.com:12345>
   ```
   Isso confirma que você está pronto para executar comandos.

### Opção C — Google Colab (sem instalar nada na máquina)

Alternativa recomendada para quem não quer instalar nenhum programa: o Colab roda em uma máquina virtual na nuvem do Google, então nada é instalado no seu computador — basta uma conta Google e o navegador.

1. Acesse [https://colab.research.google.com/](https://colab.research.google.com/) e crie um **novo notebook** (`File → New notebook`).
2. Na primeira célula, instale o cliente Python do Redis (a instalação acontece na nuvem do Colab, não na sua máquina):
   ```python
   !pip install redis
   ```
   Execute a célula (▶ ou `Shift+Enter`) e aguarde a confirmação de instalação.
3. Em uma nova célula, conecte-se ao seu banco usando os dados anotados na Parte 3:
   ```python
   import redis

   r = redis.Redis(
       host="SEU_HOST_AQUI",
       port=SUA_PORTA_AQUI,
       password="SUA_SENHA_AQUI",
       ssl=True,               # coloque False se o banco não exigir TLS
       decode_responses=True
   )

   print(r.ping())  # deve retornar True se a conexão funcionou
   ```
4. Se a célula retornar `True`, a conexão está pronta. Os comandos da Parte 5 podem ser executados como métodos Python — veja a tabela de equivalência abaixo.

**Tabela de equivalência: comando Redis → método Python (biblioteca `redis`)**

| Comando Redis | Método em Python |
|---|---|
| `SET chave valor` | `r.set("chave", "valor")` |
| `GET chave` | `r.get("chave")` |
| `EXISTS chave` | `r.exists("chave")` |
| `DEL chave` | `r.delete("chave")` |
| `EXPIRE chave segundos` | `r.expire("chave", segundos)` |
| `TTL chave` | `r.ttl("chave")` |
| `INCR chave` | `r.incr("chave")` |
| `HSET chave campo valor` | `r.hset("chave", "campo", "valor")` |
| `HGET chave campo` | `r.hget("chave", "campo")` |
| `HGETALL chave` | `r.hgetall("chave")` |

> **Dica pedagógica:** ao executar cada célula no Colab, comente acima dela qual comando Redis nativo ela equivale (ex.: `# equivalente a: SET produto:101 "Teclado Mecânico"`) — isso ajuda a fixar a sintaxe original, que é a que aparece na documentação e em avaliações.

---

## Parte 5 — Executando os comandos CRUD (passo a passo com saída esperada)

Digite cada comando abaixo (na aba CLI/Workbench do RedisInsight ou no terminal) e observe o retorno. Se estiver usando o **Google Colab (Opção C)**, use a tabela de equivalência da Parte 4 para traduzir cada comando para o método Python correspondente — o resultado esperado é o mesmo.

### 5.1 — CREATE / UPDATE (`SET`)
```
SET produto:101 "Teclado Mecânico"
```
**Saída esperada:** `OK`

**Célula equivalente no Google Colab:**
```python
# equivalente a: SET produto:101 "Teclado Mecânico"
r.set("produto:101", "Teclado Mecânico")
```
Saída esperada: `True`

### 5.2 — READ (`GET`)
```
GET produto:101
```
**Saída esperada:** `"Teclado Mecânico"`

**Célula equivalente no Google Colab:**
```python
# equivalente a: GET produto:101
r.get("produto:101")
```
Saída esperada: `'Teclado Mecânico'`

### 5.3 — Verificar se a chave existe (`EXISTS`)
```
EXISTS produto:101
```
**Saída esperada:** `(integer) 1` (1 = existe, 0 = não existe)

**Célula equivalente no Google Colab:**
```python
# equivalente a: EXISTS produto:101
r.exists("produto:101")
```
Saída esperada: `1`

### 5.4 — DELETE (`DEL`)
```
DEL produto:101
```
**Saída esperada:** `(integer) 1` (quantidade de chaves removidas)

Confirme a remoção:
```
GET produto:101
```
**Saída esperada:** `(nil)` — a chave não existe mais.

**Célula equivalente no Google Colab:**
```python
# equivalente a: DEL produto:101
r.delete("produto:101")
```
Saída esperada: `1`

```python
# confirmando a remoção — equivalente a: GET produto:101
print(r.get("produto:101"))
```
Saída esperada: `None`

### 5.5 — Expiração (`EXPIRE` e `TTL`)
Recrie a chave e defina expiração de 60 segundos:
```
SET produto:101 "Teclado Mecânico"
EXPIRE produto:101 60
```
**Saída esperada do EXPIRE:** `(integer) 1`

Verifique o tempo restante:
```
TTL produto:101
```
**Saída esperada:** um número decrescente, ex. `(integer) 57` (segundos restantes). Após 60 segundos, `GET produto:101` retornará `(nil)`.

**Célula equivalente no Google Colab:**
```python
# recriando a chave — equivalente a: SET produto:101 "Teclado Mecânico"
r.set("produto:101", "Teclado Mecânico")

# equivalente a: EXPIRE produto:101 60
r.expire("produto:101", 60)
```
Saída esperada: `True`

```python
# equivalente a: TTL produto:101
r.ttl("produto:101")
```
Saída esperada: algo como `57` (segundos restantes)

### 5.6 — Contadores (`INCR`)
```
SET visitas:pagina 0
INCR visitas:pagina
INCR visitas:pagina
GET visitas:pagina
```
**Saída esperada do último GET:** `"2"`

**Célula equivalente no Google Colab:**
```python
# equivalente a: SET visitas:pagina 0
r.set("visitas:pagina", 0)

# equivalente a: INCR visitas:pagina (executado duas vezes)
r.incr("visitas:pagina")
r.incr("visitas:pagina")

# equivalente a: GET visitas:pagina
r.get("visitas:pagina")
```
Saída esperada: `'2'`

### 5.7 — Hashes (`HSET` / `HGET` / `HGETALL`)
```
HSET usuario:1 nome "Ana" email "ana@exemplo.com"
HGET usuario:1 nome
HGETALL usuario:1
```
**Saída esperada do HGET:** `"Ana"`
**Saída esperada do HGETALL:** lista com todos os campos e valores (`nome`, `Ana`, `email`, `ana@exemplo.com`).

**Célula equivalente no Google Colab:**
```python
# equivalente a: HSET usuario:1 nome "Ana" email "ana@exemplo.com"
r.hset("usuario:1", mapping={"nome": "Ana", "email": "ana@exemplo.com"})

# equivalente a: HGET usuario:1 nome
r.hget("usuario:1", "nome")
```
Saída esperada: `'Ana'`

```python
# equivalente a: HGETALL usuario:1
r.hgetall("usuario:1")
```
Saída esperada: `{'nome': 'Ana', 'email': 'ana@exemplo.com'}`

> **Nota técnica:** no `HSET` em Python, o parâmetro `mapping={...}` é a forma correta de definir múltiplos campos de uma vez — pequena diferença de sintaxe em relação ao comando nativo, vale destacar em aula.

---

## Parte 6 — Atividade prática guiada: contador de visitas com expiração

Esta é a atividade avaliativa formativa da aula (individual ou em duplas). Execute cada etapa **na ordem**, conferindo a saída de cada comando.

**Passo 1 — Criar o contador zerado:**
```
SET visitas:home 0
```
Confirme com `GET visitas:home` → deve retornar `"0"`.

**Passo 2 — Simular 5 acessos:**
Execute o comando abaixo **5 vezes seguidas** (uma por vez):
```
INCR visitas:home
```
A cada execução, o valor deve aumentar em 1. Ao final, confira:
```
GET visitas:home
```
**Saída esperada:** `"5"`

**Passo 3 — Definir expiração de 5 minutos (300 segundos):**
```
EXPIRE visitas:home 300
TTL visitas:home
```
O `TTL` deve retornar um valor próximo de `300` e ir diminuindo a cada consulta.

**Passo 4 — Criar o cadastro do usuário como hash:**
```
HSET usuario:1 nome "Seu Nome" email "seuemail@exemplo.com"
HGET usuario:1 nome
```
**Saída esperada:** o nome que você cadastrou, entre aspas.

**Passo 5 — Registrar evidência da atividade:**
Tire um print (screenshot) da tela mostrando os comandos executados e as saídas correspondentes (RedisInsight ou terminal), e envie conforme instrução do docente.

---

## Solução de problemas comuns

| Sintoma | Causa provável | Como resolver |
|---|---|---|
| `Connection refused` ou timeout | Host/porta incorretos, ou firewall da rede bloqueando a porta | Confira o endpoint copiado na Parte 3; tente outra rede (ex.: dados móveis) se a rede institucional bloquear portas não usuais |
| "RedisInsight Web is not yet available in your selected region, tier, or configuration" | O botão de RedisInsight embutido no console ainda não está disponível para o seu banco (rollout gradual da Redis) | Use o **RedisInsight desktop** (Opção A, instruções de download e conexão manual) — funciona independente de região ou tier |
| `NOAUTH Authentication required` | Senha não informada ou incorreta | Reconfira a senha copiada na tela **Configuration**; use `-a SUA_SENHA` no `redis-cli` ou preencha o campo Password no RedisInsight |
| `WRONGPASS invalid username-password pair` | Senha copiada com espaço extra ou caractere cortado | Copie novamente a senha inteira, sem espaços antes/depois |
| Erro relacionado a certificado/TLS | Banco exige TLS mas a flag `--tls`/certificado não foi usada | Baixe o certificado na aba **Security** do console e use `--tls --cacert` conforme a Parte 4 |
| No Colab, `redis.exceptions.ConnectionError` | Parâmetro `ssl` incorreto, ou host/porta com espaço/erro de digitação | Confira se `ssl=True`/`False` corresponde à configuração real do banco; reconfira host e porta copiados |
| Banco aparece como "Pending" por muito tempo | Provisionamento em andamento (raro demorar mais que 1–2 min) | Atualize a página; se persistir, tente criar novamente |

---

## Checklist final da prática

- [ ] Conta criada no Redis Cloud
- [ ] Banco de dados gratuito criado e com status **Active**
- [ ] Conexão estabelecida (RedisInsight ou `redis-cli`)
- [ ] Comandos SET, GET, DEL, EXPIRE, TTL, INCR, HSET, HGET, HGETALL testados
- [ ] Atividade do contador de visitas concluída (Parte 6)
- [ ] Print da evidência enviado conforme orientação do docente
