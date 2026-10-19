# 2627leic32d-t1-group-2

**TDS — Trabalho Prático Fase 1 · Go em modo consola** · LEIC · Semestre de Inverno 2026/2027
📅 Entrega: **26 de outubro** (fim do dia) · 🏷️ Tag: `Trab1`

----

<p align="center">
<strong><a href="#enunciado">📄 Enunciado</a></strong>
|
<strong><a href="#duvidas">🤔 Dúvidas e Decisões</a></strong>
|
<strong><a href="#trabalho">🛠️ O Nosso Trabalho</a></strong>
|
<strong><a href="#evolucao">📈 Evolução</a></strong>
</p>

----

## 👥 Grupo

| Membro | Nº | GitHub |
|---|---|---|
| Membro 1 | | |
| Membro 2 | | |
| Membro 3 | | |

## 🧭 Como funciona este README

1. **[Enunciado](#enunciado)** → resumo e interpretação do que nos pedem.
2. **[Dúvidas e Decisões](#duvidas)** → cada dúvida que surge tem: contexto do enunciado → dúvida → **a ideia de cada um** → **decisão tomada**. Fica tudo registado para vermos a evolução e para explicarmos na apresentação.
3. **[O Nosso Trabalho](#trabalho)** → o que fizemos, como está organizado e como correr.
4. **[Evolução](#evolucao)** → histórico de alterações às decisões.

> 💡 Regra: **ninguém implementa uma coisa com dúvida aberta (🟡)** sem ficar decidida (🟢) aqui primeiro.

Legenda: 🟡 Por decidir · 🟢 Decidido · 🔴 Revisto/alterado

----

<a id="enunciado"></a>
## 📄 Enunciado — Interpretação

### Contexto geral
- A avaliação prática é feita com uma aplicação **Go**, desenvolvida ao longo do semestre, em **várias fases**.
- A app permite que **2 jogadores, cada um no seu PC**, joguem Go.
- **Fase 1 (este trabalho):** app em **modo consola**, tabuleiro e jogadas em **texto**. Os 2 jogadores usam **a mesma instância**, à vez.
- **Fase 2:** vai usar **base de dados remota** → por isso o **modelo do domínio** (tabuleiro + regras) tem de ser **reutilizável** e **separado da interface**.
- Nesta fase, o estado dos jogos é guardado em **ficheiros de texto** no diretório de trabalho.

### Requisitos principais
- Pedras **pretas = `#`**, brancas = **`O`**.
- Tamanho do tabuleiro: 19x19, 13x13 ou 9x9 → usar **`BOARD_SIZE`** (constante) ou, idealmente, **definido no início de cada jogo**.
- Ao iniciar o programa já existe um **tabuleiro vazio 9x9**.
- Após **cada comando bem sucedido** mostrar: tabuleiro, **de quem é a vez**, **capturas** de cada jogador e, se terminou, a **pontuação final**.
- Máximo de **reutilização de código**: tudo o que não é UI deve estar no domínio.
- Submissão no GitHub do grupo com tag `Trab1`, com **várias submissões (commits)** ao longo do tempo.

### Exemplo de output do enunciado
```
> play 3f
  A B C D E F G H I
9 . . . . . . . . .
8 . . # . . . . . .
7 . . . . . . . . .
6 . . # . O . . . .
5 . . . . O . . . .
4 . . . . . . . . .
3 . . . . . # . . .
2 . . . . . . . . .
1 . . . . . . . . .
Turn: O (White)  Captures: #=0 - O=0
>
```

### Comandos

| Comando | O que faz (enunciado) |
|---|---|
| <kbd>new</kbd> `[<size>]` | Novo tabuleiro (tamanho opcional); **pretas jogam primeiro**; o tabuleiro corrente é **ignorado** |
| <kbd>play</kbd> `<position>` | Coloca pedra **se a jogada for válida**; pode **capturar** pedras cercadas do adversário |
| <kbd>pass</kbd> | Passa a vez; se o adversário **também passou na vez anterior → jogo termina** |
| <kbd>save</kbd> `<name>` | Guarda o estado num **ficheiro local**; pode ser feito **a qualquer momento**, mesmo com jogo terminado |
| <kbd>load</kbd> `<name>` | Carrega tabuleiro guardado; o tabuleiro corrente é **ignorado** |
| <kbd>exit</kbd> | Termina a aplicação |

### Regras resumidas
1. **Liberdade** = ponto vazio ortogonalmente adjacente (cima, baixo, esq., dir.). Cada pedra precisa de ≥1 liberdade **própria ou do grupo** (grupo = pedras ligadas ortogonalmente).
2. Pedra/grupo sem liberdades → **capturado**.
3. **Não se pode jogar** onde a pedra ficaria sem liberdades (**suicídio**), **exceto** se a jogada capturar pedras do adversário.
4. **Não se pode jogar** se **recriar o estado anterior** do tabuleiro (**loop / ko**).
5. **Pontuação final** = peças capturadas + posições vazias cercadas por pedras próprias.
   - **Pretas** levam **− komi**: **5.5** (19x19), **4.5** (13x13), **3.5** (9x9).
6. **Nota:** **não** se consideram pedras "mortas" dentro de áreas cercadas (simplificação).

----

<a id="duvidas"></a>
## 🤔 Dúvidas e Decisões

### 📑 Índice

| # | Tema | Estado |
|---|---|---|
| [D01](#d01) | Arquitetura e separação domínio / UI | 🟡 |
| [D02](#d02) | Tamanho do tabuleiro (`BOARD_SIZE` vs por jogo) | 🟡 |
| [D03](#d03) | Representação do tabuleiro e das pedras | 🟡 |
| [D04](#d04) | Formato das posições (`3f` vs `f3`, maiúsculas, limites) | 🟡 |
| [D05](#d05) | Output: desenho do tabuleiro e linha de estado | 🟡 |
| [D06](#d06) | Grupos e liberdades (como calcular) | 🟡 |
| [D07](#d07) | Capturas e ordem de verificação da jogada | 🟡 |
| [D08](#d08) | Suicídio | 🟡 |
| [D09](#d09) | Regra do loop (ko): o que comparar e o que guardar | 🟡 |
| [D10](#d10) | `pass` e fim de jogo | 🟡 |
| [D11](#d11) | Pontuação (território, áreas neutras, komi) | 🟡 |
| [D12](#d12) | Comando `new` (tamanho opcional, tamanhos válidos) | 🟡 |
| [D13](#d13) | Ficheiros: formato, nome, local, o que guardar | 🟡 |
| [D14](#d14) | `load`: validação e ficheiros inválidos | 🟡 |
| [D15](#d15) | Erros, comandos inválidos e mensagens | 🟡 |
| [D16](#d16) | Comandos depois do jogo terminar | 🟡 |
| [D17](#d17) | Testes | 🟡 |
| [D18](#d18) | Organização do Git (branches, commits, tag) | 🟡 |

----

<a id="d01"></a>
### D01 — Arquitetura e separação domínio / UI  🟡

**📖 Contexto no enunciado:** *"maximizar a reutilização do código… modelo do domínio… tudo aquilo que não seja relativo à interface com o utilizador"*. Na fase 2 haverá base de dados remota.

**❓ Dúvida:** Como dividimos o projeto? Que classes/módulos ficam no domínio, no armazenamento (ficheiros) e na UI (consola)? Como garantir que o domínio não depende de `println`/ficheiros?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _(a preencher)_ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d02"></a>
### D02 — Tamanho do tabuleiro (`BOARD_SIZE` vs por jogo)  🟡

**📖 Contexto no enunciado:** *"usar a dimensão configurada numa constante `BOARD_SIZE` ou, idealmente, definido no início de cada jogo"*. Ao arrancar existe um 9x9.

**❓ Dúvida:** Fazemos a constante ou já guardamos o tamanho dentro do objeto do jogo? O "ideal" compensa já agora?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d03"></a>
### D03 — Representação do tabuleiro e das pedras  🟡

**📖 Contexto no enunciado:** *Pedras pretas `#`, brancas `O`; tabuleiro com estado, capturas, vez.*

**❓ Dúvida:** Matriz 2D, `Map<Position, Player>`, lista? Estado **imutável** (cada jogada devolve novo estado) ou mutável? Como representar "vazio"? Enum/classe para o jogador?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d04"></a>
### D04 — Formato das posições  🟡

**📖 Contexto no enunciado:** Exemplo `play 3f` (**número primeiro, depois letra**); colunas `A B C … I`; linha 1 em baixo, linha 9 em cima.

**❓ Dúvida:**
- Aceitamos só `3f` ou também `f3`?
- Maiúsculas/minúsculas?
- Colunas `A–I` incluem o `I` (no exemplo sim). E para 13 e 19?
- Linhas com 2 dígitos (`13f`, `19s`) — como fazer o parse?
- Posição fora do tabuleiro → erro?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d05"></a>
### D05 — Output: tabuleiro e linha de estado  🟡

**📖 Contexto no enunciado:** *"Por cada execução bem sucedida de um comando é apresentado o tabuleiro… vez de jogar, capturas… pontuação final se tiver terminado"*. Formato do exemplo: `Turn: O (White)  Captures: #=0 - O=0`.

**❓ Dúvida:** Copiamos **exatamente** o formato do exemplo? Como alinhar quando há linhas com 2 dígitos (13x13/19x19)? Como mostramos a pontuação final? Mostra-se o tabuleiro após `save`/`load`/`new`?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d06"></a>
### D06 — Grupos e liberdades  🟡

**📖 Contexto no enunciado:** *Liberdade ortogonal; grupo ligado nas ortogonais com ≥1 liberdade.*

**❓ Dúvida:** Algoritmo para encontrar o grupo (BFS/DFS/recursivo)? Calculamos sempre de raiz ou guardamos grupos? Onde vive esta lógica (função pura no domínio)?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d07"></a>
### D07 — Capturas e ordem de verificação da jogada  🟡

**📖 Contexto no enunciado:** *"A jogada pode capturar as pedras do adversário que ficaram cercadas."* / *"A jogada só é realizada caso seja possível segundo as regras."*

**❓ Dúvida:** Qual a **ordem** exata ao jogar? Exemplo de proposta a discutir:
1. posição válida e vazia → 2. colocar pedra → 3. capturar grupos adversários sem liberdades → 4. verificar suicídio → 5. verificar loop → 6. aplicar (ou rejeitar e manter estado).

Como contamos as capturas (por jogador = pedras do adversário que ele capturou)?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d08"></a>
### D08 — Suicídio  🟡

**📖 Contexto no enunciado:** *"Uma pedra não pode ser colocada onde não teria liberdades diretas ou de grupo. Mas poderá fazê-lo se com este movimento forem capturadas pedras do oponente."*

**❓ Dúvida:** A verificação é feita **antes ou depois** de aplicar as capturas? O que acontece se juntar a um grupo próprio que fica sem liberdades? Mensagem de erro?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d09"></a>
### D09 — Regra do loop (ko)  🟡

**📖 Contexto no enunciado:** *"Uma pedra não pode ser jogada se recriar o estado anterior do tabuleiro (loop)."*

**❓ Dúvida:**
- "Estado anterior" = **só o tabuleiro imediatamente anterior** (ko simples) ou **qualquer estado anterior** (superko)?
- Comparamos só as pedras ou também de quem é a vez?
- Temos de **guardar o tabuleiro anterior** no ficheiro do `save` para o `load` continuar a respeitar a regra?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d10"></a>
### D10 — `pass` e fim de jogo  🟡

**📖 Contexto no enunciado:** *"Passa a vez para o adversário. O jogo termina caso o adversário também tenha passado na vez anterior."*

**❓ Dúvida:** Contamos **passes consecutivos** (um `play` no meio faz reset)? Como guardamos isto (contador / booleano)? Quando o jogo termina, mostramos logo a pontuação?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d11"></a>
### D11 — Pontuação  🟡

**📖 Contexto no enunciado:** *Pontuação = capturas + posições vazias cercadas pelas suas pedras. Pretas: − 5.5 / 4.5 / 3.5 (19 / 13 / 9). Não considerar pedras "mortas".*

**❓ Dúvida:**
- Como identificar **regiões vazias** (flood fill de vazios)?
- Região vazia **encostada às duas cores** → ninguém pontua?
- Região vazia encostada **só a uma cor** → pontua para essa cor?
- Tabuleiro **todo vazio** → ninguém pontua?
- Komi para tamanhos fora de 9/13/19 (se permitirmos)?
- Tipo da pontuação: `Double` (por causa do .5).
- Como declaramos o vencedor / empate (impossível com .5?)

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d12"></a>
### D12 — Comando `new`  🟡

**📖 Contexto no enunciado:** `new [<size>]` — tamanho opcional; pretas começam; tabuleiro corrente ignorado.

**❓ Dúvida:** Sem argumento → 9? Tamanhos válidos só 9/13/19 ou qualquer? Pedimos confirmação se houver jogo em curso (o enunciado diz que é *ignorado*)? Reset de capturas/passes/histórico?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d13"></a>
### D13 — Ficheiros: formato, nome, local, conteúdo  🟡

**📖 Contexto no enunciado:** *"ficheiros de texto localizados no diretório de trabalho"*; `save <name>` / `load <name>`; save pode ser feito a qualquer momento, mesmo com jogo terminado.

**❓ Dúvida:**
- Formato do ficheiro (linhas simples? chave=valor? JSON? uma linha por linha do tabuleiro?)
- Extensão (`.txt`, `.go`, nenhuma)?
- O que guardar: **tamanho, tabuleiro, vez, capturas, passes, jogo terminado, tabuleiro anterior (ko)**.
- Save com nome já existente → sobrescreve?
- Nomes com caracteres inválidos / paths (`../`)?
- Como abstrair o armazenamento (interface) para trocar por BD na fase 2?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d14"></a>
### D14 — `load`: validação e ficheiros inválidos  🟡

**📖 Contexto no enunciado:** `load <name>` carrega tabuleiro guardado; o corrente é ignorado.

**❓ Dúvida:** Ficheiro não existe → erro e mantém o jogo atual? Ficheiro corrompido/inconsistente (tamanho errado, símbolos estranhos)? Validamos o estado carregado? Mostramos o tabuleiro depois do load?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d15"></a>
### D15 — Erros, comandos inválidos e mensagens  🟡

**📖 Contexto no enunciado:** *"Por cada execução **bem sucedida** de um comando é apresentado o tabuleiro…"* (não diz nada sobre falhas).

**❓ Dúvida:** Em caso de erro mostramos o tabuleiro? Que mensagens (idioma PT/EN)? Comando desconhecido, argumentos a mais/menos, `play` sem posição… Como o domínio comunica o erro (exceção, `Result`, enum de erro)?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d16"></a>
### D16 — Comandos depois do jogo terminar  🟡

**📖 Contexto no enunciado:** `save` pode ser feito *mesmo que já tenha terminado*.

**❓ Dúvida:** Depois de terminar: `play` e `pass` dão erro? `new`, `load`, `save`, `exit` continuam permitidos? Quem pode ser mostrado (pontuação final em todos os outputs)?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d17"></a>
### D17 — Testes  🟡

**📖 Contexto no enunciado:** *(não referido — decisão nossa.)*

**❓ Dúvida:** Que framework? O que testar primeiro (liberdades, capturas, suicídio, ko, pontuação, save/load)? Há casos de teste de tabuleiros pequenos para validar as regras?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

<a id="d18"></a>
### D18 — Organização do Git  🟡

**📖 Contexto no enunciado:** *"repositório usado pelo grupo para trabalho colaborativo, devendo existir várias submissões até à submissão final"*; tag **`Trab1`** até **26 de outubro**.

**❓ Dúvida:** Branches por feature ou tudo na `main`? Convenção de commits? Quem faz a tag e quando? Quem revê o quê?

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _

----

### ➕ Template para novas dúvidas

Copiar, colar no fim e preencher (e adicionar ao índice):

```markdown
<a id="dXX"></a>
### DXX — Título da dúvida  🟡

**📖 Contexto no enunciado:** _citação/resumo da parte do enunciado_

**❓ Dúvida:** _qual é a questão concreta?_

| Membro | Ideia |
|---|---|
| **Membro 1:** | |
| **Membro 2:** | |
| **Membro 3:** | |

**✅ Decisão tomada:**
> _(a preencher)_

**🧠 Porquê:** _ · **📅 Data:** _ · **👤 Quem decidiu:** _
```

----

<a id="trabalho"></a>
## 🛠️ O Nosso Trabalho

> Preencher à medida que for avançando. Serve de guia para a apresentação.

### 🧩 Visão geral
_Uma frase/parágrafo: o que a app faz e como está organizada (ver [D01](#d01))._

### 🗂️ Estrutura do projeto
```
src/
├── domain/      # regras do Go, tabuleiro, jogadas, pontuação (sem I/O)
├── storage/     # save/load em ficheiros de texto
└── ui/          # consola: parse de comandos e output
```
_(ajustar ao que realmente fizermos)_

### ▶️ Como correr
```bash
# comando para compilar / correr
```

### 💬 Exemplo de sessão
```
> new 9
> play 5e
> pass
> save jogo1
> exit
```

### ✅ Checklist de funcionalidades

| Funcionalidade | Responsável | Estado |
|---|---|---|
| Modelo: tabuleiro e jogadores | | ⬜ |
| Parse de posições | | ⬜ |
| `new [size]` | | ⬜ |
| `play` + validações | | ⬜ |
| Grupos, liberdades e capturas | | ⬜ |
| Suicídio e loop (ko) | | ⬜ |
| `pass` + fim de jogo | | ⬜ |
| Pontuação + komi | | ⬜ |
| `save` / `load` | | ⬜ |
| `exit` | | ⬜ |
| Output do tabuleiro + estado | | ⬜ |
| Testes | | ⬜ |
| Tag `Trab1` | | ⬜ |

### 🎤 Para a apresentação
- [ ] Mostrar o enunciado e as [decisões](#duvidas) mais importantes
- [ ] Demo da app a correr
- [ ] Explicar a separação domínio / UI e porquê (fase 2)
- [ ] O que mudámos de ideias ao longo do trabalho (ver [Evolução](#evolucao))

----

<a id="evolucao"></a>
## 📈 Evolução das decisões

| Data | Decisão | O que mudou | Porquê |
|---|---|---|---|
| | | | |

----

<p align="center"><sub>TDS · LEIC · ISEL · 2026/2027 · Grupo 2</sub></p>