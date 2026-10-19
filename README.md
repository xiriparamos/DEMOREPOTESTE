# 2627leic32d-t1-group-2

**TDS — Trabalho Prático Fase 1 · Go em modo consola** · LEIC · Semestre de Inverno 2026/2027
 Entrega: **26 de outubro**  · 

----

<p align="center">
<strong><a href="#enunciado"> Enunciado</a></strong>
|
<strong><a href="#duvidas"> Dúvidas e Decisões</a></strong>
|
<strong><a href="#trabalho"> O Nosso Trabalho</a></strong>
|
<strong><a href="#evolucao"> Evolução</a></strong>
</p>

----

##  Grupo

| Membro | Nº | GitHub |
|---|---|---|
| Filipa | | |
| Guilherme | | |
| João | | |

##  Como funciona este README

1. **[Enunciado](#enunciado)** → resumo e interpretação do que nos pedem.
2. **[Dúvidas e Decisões](#duvidas)** → cada dúvida que surge tem: contexto do enunciado → dúvida → **a ideia de cada um** → **decisão tomada**. Fica tudo registado para vermos a evolução e para explicarmos na apresentação.
3. **[O Nosso Trabalho](#trabalho)** → o que fizemos, como está organizado e como correr.


> 💡 Regra: **ninguém implementa uma coisa com dúvida aberta (🟡)** sem ficar decidida (🟢) aqui primeiro.

Legenda: 🟡 Por decidir · 🟢 Decidido · 🔴 Revisto/alterado

----

<a id="enunciado"></a>
##  Enunciado — Interpretação

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
##  Dúvidas e Decisões

###  Índice

| # | Tema | Estado |
|---|---|---|
| [D01](#d01) | 1ºDUVIDA POR ESCREVER | 🟡 |


----

<a id="d01"></a>
### D01 — _(a preencher)_  🟡

** Contexto no enunciado:** *"_(a preencher)_"*. 

** Dúvida:** escrever a duvida

| Membro | Ideia |
|---|---|
| **Filipa:** | |
| **Guilherme:** | |
| **João:** | |

** Decisão tomada:**
> _(a preencher)_

** Porquê:** _(a preencher)_ · ** Data:** _ · 

----



### ➕ Template A copiar

Copiar, colar no fim e preencher (e adicionar ao índice):

```markdown
<a id="dXX"></a>
### DXX — Título da dúvida  🟡

** Contexto no enunciado:** _resumo da parte do enunciado_

** Dúvida:** _qual é a duvida concreta?_

| Membro | Ideia |
|---|---|
| **Filipa:** | |
| **Guilherme:** | |
| **João:** | |

** Decisão tomada:**
> _(a preencher)_

** Porquê:** _ · **Data:** _ · 
```

----

<a id="trabalho"></a>
##  O Nosso Trabalho

> Preencher à medida que for avançando. Serve de guia para a apresentação.

###  Visão geral
_por prencher_

###  Estrutura do projeto
```
src/
├── como/      # oq cada pasta tem 
├── esta/     # oq cada pasta tem 
└── estruturado/          # oq cada pasta tem 
```

###  Como correr
```bash
# comando para compilar / correr
```

###  Exemplo de sessão
```
output da consola
```

###  Checklist de funcionalidades

| Funcionalidade | Responsável | Estado |
|---|---|---|
| Parte do trabalho | | ⬜ |

Legenda: 🟡 Por decidir · 🟢 Decidido · 🔴 Revisto/alterado


<a id="evolucao"></a>
##  Evolução das decisões

| Data | Decisão | O que mudou | Porquê |
|---|---|---|---|
| 31/02/1900|ficar de pe | de bolas a bolinhas| mais pratico e comodo |

----

<p align="center"><sub>TDS · LEIC · ISEL · 2026/2027 · Grupo 2</sub></p>