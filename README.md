# Renan Toons Wacky Wars

Jogo de luta 2D estilo plataforma (inspirado em *Super Smash Bros.*), feito em um único arquivo `index.html` (HTML + Canvas + JavaScript puro). Cada jogador tem 3 vidas; o dano acumulado aumenta o knockback, e quem cai para fora do palco perde uma vida.

## Como jogar

Abra o `index.html` no navegador (as imagens/sprites ficam nas pastas ao lado, ex.: `Renanzinho Sprites/`, `Aggie Sprites/`, e o `icon.png`).

### Controles

| Ação | Teclado | Celular |
|---|---|---|
| Andar | ← / → | Joystick na tela |
| Pular | ↑ | Joystick na tela |
| Ataque principal | **Z** | Botão Z |
| Ataque secundário | **X** | Botão X |
| Variações | segurar **↓** + Z ou X | Joystick para baixo + botão |
| Jogar de novo (fim da partida) | **R** | Tocar na tela |

Também há vibração em controles (Gamepad API) quando o navegador suporta.

## Personagens e ataques

| Personagem | Z | ↓ + Z | X | ↓ + X |
|---|---|---|---|---|
| **Renanzinho** | Soco (combo de 2 + chute) | Arremessa o balde na corda | Chute | — |
| **Aggie** | Arremessa frasco de esmalte (explode após 2 s) | Arremessa o alicate (boomerang) | Ataque de unha | — |
| **Renan Antigo** | Tiro (segurar = tiro carregado) | Escudo | Dash com soco | — |
| **Blui e Reddie** | Soco | — | Cospe mini bolinhas de gelatina | — |
| **Doper** | Arremessa copo (estilhaça ao ser tocado) | — | Empurrão de gerente | Toca sax e solta nota voadora |
| **Hofy** | Canta (nota que atravessa oponentes) | — | Taca o microfone (desliza até acertar) | — |
| **FooshLooket** | Cospe água que forma poça (no ar, é o pulo duplo) | — | Vira bola e quica | — |
| **Dan** | Desenha uma forma que ganha vida | Pinta poça de tinta (lentidão) | Giro com o pincel | — |
| **Suzan** | Batida de notebook | — | Solta o `print("hello world!")` | Bola jelly de bugs que persegue e atordoa |

> Hofy, Dan e Suzan não têm pulo duplo.

### Suzan

A nerd do grupo The White Ones, de Dan. Luta de longe, com programação:

- **Z, batida de notebook:** golpe único e curto (4 de dano), sem combo. É a defesa dela quando alguém cola.
- **X, `print("hello world!")`:** uma fileira de letras verdes (cor de terminal) sai da mão dela, letra por letra, fica esticada um instante e volta. Na ida empurra quem estiver no caminho (5 de dano); na volta puxa em direção a ela (3 de dano). Cada alvo apanha uma vez na ida e uma vez na volta, e a fileira só pode ser usada de novo depois que volta (mais um pequeno cooldown).
- **↓ + X, bola jelly de bugs:** uma bola de gelatina glitchada, de tamanho médio, que sai da mão dela e **persegue os oponentes** por **3 s**. Quem ela toca fica **atordoado por 1 s** (sem andar nem atacar; sem dano nem knockback). O cooldown é de **4 s** a partir do spawn.
  - A bola vai atrás do oponente vivo mais próximo, mas passa para outro alvo logo depois de atordoar alguém (só volta ao mesmo se não houver outro). O mesmo alvo só pode ser atordoado de novo depois de 1,6 s. Escudo ativo bloqueia o atordoamento e aliados são ignorados no 2 vs 2.
  - Visual: gelatina que ondula e estica na direção do movimento, com aberração cromática (ciano, magenta e amarelo multiplicados, que juntos dão preto), faixas deslocadas aleatórias de glitch e caracteres de terminal (`0`, `1`, `NaN`, `null`, `404`, `ERR`) piscando por dentro. Some aos poucos nos últimos 300 ms.
  - Implementação: bloco `BOLA JELLY DE BUGS DA SUZAN` do `index.html` (`glitchBalls`, `startGlitchBall`, `updateGlitchBalls`, `drawGlitchBalls`, `applyGlitchStun`; constantes `GLITCH_*`). Como os outros projéteis, cada cliente anima a bola localmente e só o dono detecta os acertos, avisando os outros pelas mensagens `glitchBall` (spawn) e `glitchStun` (atordoamento). O atordoamento usa o hitstun do jogo, com uma trava (`stunLockUntil`) para não ser cancelado ao pousar.

### Dica: o personagem sem nome

Entre no jogo sem escolher ninguém e você controla um retângulo de cara de paisagem (olhos desencontrados, boca torta e um tracinho embaixo). Ele não tem ataques nem pulo duplo, só anda e pula, e muda de cor conforme a ação: branco parado, vermelho andando, azul pulando. Ele também tem **corpo de gelatina**, com a malemolência de um travesseiro ou de um saco de pancada: o topo balança atrasado quando ele anda, para, vira ou leva um golpe, e o corpo incha e achata em pulos, pousos e golpes, oscilando até assentar. A cara de paisagem continua a mesma.

- **Como funciona:** o sprite é desenhado em 14 faixas horizontais ligadas por molas (a base presa ao chão, o topo livre) mais uma mola de incha/achata. Cada faixa é um paralelogramo, então o contorno não quebra. Isso mexe só no desenho: hitbox, velocidade e pulo não mudam, e o flash vermelho de dano acompanha a deformação. Funciona também para jogadores remotos, usando o movimento que chega pela rede.
- **Desempenho:** 14 `drawImage` por jogador desse tipo e nenhuma alocação por quadro; os demais personagens são desenhados como antes.
- **Ajuste:** constantes `SOFT_*` no bloco `CORPO MOLE` do `index.html` (`SOFT_DAMP` menor balança por mais tempo; `SOFT_MAX` maior leva o topo mais longe).

## Mapas

Cubos · Colinas · DORFic · **Vector**

O mapa **Vector** tem 3 plataformas flutuantes que se movem (duas sobem/descem, uma vai de um lado para o outro). Os outros mapas têm só o palco principal (Cubos tem plataformas flutuantes fixas).

## Multiplayer

Online via [PeerJS](https://peerjs.com/) (conexão P2P entre jogadores, com código de sala curto). Cada cliente simula a física dos projéteis localmente; só o dono de cada ataque detecta o acerto e avisa os outros.

O multijogador tem três modos, escolhidos antes do lobby: **Normal** (todos contra todos), **Torneio** (3 a 8 jogadores) e **2 vs 2** (exatamente 4 jogadores).

### Modo 2 vs 2

Duas duplas se enfrentam na mesma arena. A dupla que ficar de pé vence, e não há fogo amigo.

**Times.** Os times são montados no início de cada partida a partir da ordem do roster: **P1 + P2** (Time Vermelho, `#e53935`) contra **P3 + P4** (Time Azul, `#1e88e5`). A partida termina quando todos os jogadores vivos são do mesmo time. Dentro da partida, o nome de cada jogador aparece na cor do seu time, com contorno branco e uma seta acima.

**Lobby e seleção de personagem:**

- Os cards de jogador (`.ready-pcard`) usam a cor do **time** em vez da cor da porta (as classes `t0` e `t1` sobrescrevem `p1`–`p4`): P1 e P2 vermelhos, P3 e P4 azuis.
- Cada card ganhou uma etiqueta "Vermelho" ou "Azul" no canto, e um **"VS"** (`.ready-teamvs`) aparece entre as duas duplas.
- As luvas dos cursores na tela de pronto também seguem a cor do time.
- Os outros modos continuam com as cores por porta.

**Tela de vitória com a dupla.** Os **dois** jogadores do time vencedor aparecem lado a lado na cena 2.5D (`#victory-champion` e `#victory-champion2`, a ±95 px do centro), cada um com seu sprite e nome, pulando em sequência. O aro de luz no chão fica mais largo (`.duo`), a placa "1º" mostra o nome do time e os dois nomes (ex.: *Time Vermelho · Ana & Bia*) e a cor de destaque (confete, brilho) é a do time. Isso vale mesmo que o parceiro tenha sido eliminado antes do fim. No painel de colocações, os perdedores têm a borda inferior na cor do time deles.

#### Fundo "Frutiger Aero" animado

No lobby e na seleção de personagem do 2v2, o overlay recebe a classe `aurora` e o fundo é um conjunto de camadas (`.au-bg`) injetado por JavaScript, só com gradientes CSS (sem imagens externas). São 4 temas que alternam em *crossfade* num ciclo de 48 s (cada um fica visível cerca de 12 s, com transição de 4 s):

1. **Aurora verde-água:** degradê verde-água para azul, com arcos de luz verde-limão e azul claro, brilho amarelo-esverdeado e clarão branco na base.
2. **Cortina verde-azul:** luz no canto superior esquerdo, feixes verticais, arcos finos azuis e horizonte azul brilhante embaixo.
3. **Ondas azuis:** faixas translúcidas que deslizam e ondulam sobre um azul-ciano.
4. **Ciano com bolhas:** feixe de luz branca, bolhas subindo devagar e linha de horizonte.

Cada tema tem movimento próprio (feixes e arcos balançam, ondas deslizam, bolhas sobem). Por causa de problemas de travamento nas primeiras versões, o fundo segue regras de desempenho:

- Só `transform` e `opacity` são animados (camadas promovidas com `will-change`); nada de animar `filter`, `background-position` ou sombras grandes.
- Sem `backdrop-filter`/blur no vidro dos cards.
- A animação da grade azul do lobby é desligada nesse tema (`animation: none`), pois continuava rodando por baixo.
- Com `prefers-reduced-motion`, o fundo fica parado no primeiro tema.

#### Boneco 3D no fundo

Uma figura em wireframe (cúpula "meio oval" com uma esfera no topo, feita de 14 prismas hexagonais em CSS 3D) gira lentamente atrás do card (uma volta a cada 30 s). Só as arestas das 6 faces laterais são desenhadas (sem tampas), para pesar pouco. Em telas com menos de 800 px ela fica menor e centralizada embaixo. O markup é gerado por JavaScript a partir de uma lista de anéis `[largura, altura, y]` (classes `.bg-dome`, `.bd-rig`, `.bd-spin`, `.bd-prism`, `.bd-face`).

#### Cards e botões Frutiger Aero

Os botões e cards do 2v2 usam um visual de vidro:

- **Botões** (abas "Criar Sala"/"Entrar", `main-btn` do lobby e da seleção, "Voltar"): pílula com degradê, brilho branco embaixo (`radial-gradient` na base), reflexo diagonal (`::before`) e realce de vidro no topo (`::after`). Cores em **OKLCH** (`--hue`, `--sat`, `--glow-intensity`) com cor de reserva para navegadores sem suporte (`@supports`). Hover sobe 1 px, clique afunda 1 px. Os botões principais são verde-água (`--hue: 150`), as abas azuis (`215`) e a aba ativa verde brilhante (`145`). Os pseudo-elementos ficam atrás do texto (`z-index: -1` com `isolation: isolate`).
- **Card do lobby e painel da seleção:** vidro translúcido com borda branca, brilho curvo no topo e luz interna.
- **Demais elementos:** título "Multijogador", campo de código (pílula branca com sombra interna), caixa da sala, barra de status, título da seleção, contador de jogadores, círculo "VS" e slots de personagem (o selecionado pulsa em verde).

Todo esse tema fica restrito a `#lobby-overlay.aurora` e `#ready-overlay.aurora`, então os modos Normal e Torneio não mudam.

### Modo Torneio

Além do modo normal, o multijogador tem o modo **Torneio** (mínimo de 3 e máximo de 8 jogadores). O host monta a chave e o mapa é sorteado a cada luta, por isso a seleção de mapa some do lobby nesse modo.

Visual do lobby do torneio:

- **Tema de entardecer** com cilindros ao fundo e um **troféu 3D** girando no topo do cilindro grande.
- **Troféu em wireframe**: o troféu (feito de prismas hexagonais em CSS 3D) agora é desenhado só com as arestas, em tom dourado nas peças do copo e cinza claro nas bases, com faces transparentes. As tampas de topo e fundo de cada prisma não são mais renderizadas, e as faces que nunca aparecem em nenhum ângulo da rotação (coladas entre prismas ou escondidas dentro de outro prisma) também foram removidas (*occlusion culling* estático, calculado por ray-casting em todos os ângulos), para o troféu pesar menos na interface.
- **Fogos de artifício no fundo**: um `<canvas>` (`#lobby-fireworks`) fica atrás dos cilindros, do troféu e do card, com foguetes que sobem e explodem na metade de cima da tela (esfera, anel e bicolor, com rastro brilhante e cores variadas). A animação só roda enquanto o lobby do torneio está aberto e a aba está visível; ao sair, ela para e limpa a tela. A resolução e o número de partículas são limitados para não pesar.

Tela **VS** entre as lutas do torneio: agora dura **5,5 s** (antes 3,8 s). O tempo é controlado pela constante `VS_MS`, junto das demais temporizações do torneio (`BRACKET_DRAW_MS`, `BRACKET_WAIT_MS`, `RESULT_DELAY_MS`). As animações de entrada da tela terminam em cerca de 1,25 s, então o VS fica parado na tela pelo restante do tempo.

## Plataformas móveis (mapa Vector)

A posição das plataformas é uma função do relógio sincronizado (`vecNow()`), então todos os clientes calculam a mesma posição sem trocar mensagens.

Tudo que **fica parado** numa plataforma móvel agora acompanha o movimento dela, em vez de ficar flutuando no ar quando a plataforma sai de baixo:

- **Poça d'água** (FooshLooket) e **poça de tinta** (Dan): já guardavam o índice da plataforma + deslocamento e seguem a plataforma.
- **Frasco de esmalte** (Aggie): pousado, vai junto até explodir.
- **Respingos/poça de esmalte** (explosão): antes só colidiam com o palco e atravessavam as plataformas flutuantes; agora pousam nelas e acompanham as móveis.
- **Copo** (Doper): rolando ou já armado, vai junto com a plataforma.
- **Microfone** (Hofy): a velocidade de deslize passa a ser relativa à plataforma.
- **Estilhaços de vidro**: agora quicam nas plataformas flutuantes.

Implementação: helpers `rideAttach(obj, superfície, índice)` (ao pousar) e `rideCarry(obj, plats)` (uma vez por frame, antes de mover/colidir). O objeto guarda `ridePlat` (índice da plataforma, `-1` = solto), `rideX` e `rideY` (última posição da plataforma) e é deslocado pela diferença a cada frame. Para fazer um novo objeto "pegar carona", basta chamar `rideAttach` ao pousar, `rideCarry` no início do update e `ridePlat = -1` quando ele sair do chão.

## Demonstração (attract mode)

Depois de 15 s parado na tela de título, dois lutadores controlados por CPU lutam por 60 s com o motor real do jogo (nada é enviado pela rede) e depois volta ao título. Qualquer tecla ou toque encerra a demonstração. O mapa Vector não entra no sorteio da demonstração, porque depende do relógio do host.

Visual da demonstração:

- **Cartela de abertura "A VS B"**: faixas diagonais nas cores dos lutadores (vermelho × azul) entrando pelos lados, retrato com leve balanço, nome, subtítulo do personagem, "VS" gigante e o nome da arena. Os bots esperam ~2,7 s parados durante a cartela e a luta começa com um "LUTE!" e flash.
- **"K.O.!"** com flash, na cor do lutador, sempre que alguém perde uma vida.
- **Selo "● DEMONSTRAÇÃO"** (bolinha vermelha pulsando) no topo, com o aviso para tocar/pressionar uma tecla.
- **Barra de progresso** fina no topo da tela, que enche ao longo dos 60 s.
- **Ambiente**: vinheta nas bordas, linhas de varredura sutis, faixa de luz que atravessa a tela de tempos em tempos e partículas de brilho subindo.
- Os lutadores aparecem com o nome do personagem (em vez de "CPU 1/2"). Retrato ausente some em vez de mostrar imagem quebrada.

IA das CPUs (mais humana): cada lutador sorteia uma personalidade (agressividade, nervosismo, imprecisão) e um **plano de jogo** que muda a cada 5-9 s conforme o placar (pressionar, zonear, armar armadilhas ou provocar e punir). Ele também emenda golpes depois de acertar, arma uma armadilha e se afasta, anda em "toques" com pausas, dá pulinhos ociosos, erra o alcance de vez em quando, tem lapsos de atenção, reação variável e reage a levar dano (respira ou parte pra revanche).

Tudo isso fica no bloco `LUTA DE DEMONSTRAÇÃO` do `index.html` (CSS do overlay + `DEMO.intro()` / `DEMO.koFlash()`), sem alterar a IA das CPUs. O HUD de dano continua visível embaixo, por isso nada novo é posicionado ali.

## Tecnologias

HTML5 Canvas, JavaScript (sem build), PeerJS, Font Awesome (CDN) e Web Audio para os efeitos. O troféu do lobby do torneio e o boneco do fundo do 2v2 usam CSS 3D, os fogos de artifício usam um `<canvas>` 2D próprio, e o fundo e os botões do 2v2 usam apenas gradientes e animações CSS.

## Changelog

- **Corpo de gelatina no personagem sem nome:** o retângulo de cara de paisagem ganhou malemolência de travesseiro/saco de pancada (balanço atrasado do topo, incha e achata em pulos, pousos e golpes), feito em 14 faixas com molas e só no desenho, sem mudar hitbox nem física.
- **Suzan · bola jelly de bugs (↓ + X):** bola de gelatina glitchada que persegue os oponentes por 3 s e atordoa por 1 s (cooldown de 4 s); passa para outro alvo depois de atordoar alguém.
- **Novo personagem · Suzan:** nerd dos White Ones, com batida de notebook (Z) e `print("hello world!")` (X); sem pulo duplo.
- **2 vs 2 · cards por time:** os cards de jogador do lobby/seleção passam a usar a cor do time (P1+P2 vermelho, P3+P4 azul), com etiqueta de time e "VS" entre as duplas; as luvas dos cursores também seguem o time.
- **2 vs 2 · vitória em dupla:** a tela de vitória mostra os dois jogadores do time vencedor (antes só um), com placa do time, cor de destaque do time e aro de luz mais largo; os cartões de colocação ganham a cor do time.
- **2 vs 2 · fundo Frutiger Aero animado:** novo fundo de gradientes com 4 temas em *crossfade* (aurora verde-água, cortina verde-azul, ondas azuis e ciano com bolhas), feito só com `transform`/`opacity` para não travar; respeita `prefers-reduced-motion`.
- **2 vs 2 · boneco 3D no fundo:** figura em wireframe (cúpula + esfera, 14 prismas hexagonais) girando atrás do card, sem tampas para pesar menos.
- **2 vs 2 · cards e botões Frutiger Aero:** botões em pílula de vidro (cores OKLCH, brilho inferior, reflexo diagonal e realce no topo), cards em vidro translúcido e demais elementos do lobby e da seleção no mesmo estilo.
- **2 vs 2 · correções:** o "VS" entre os times ganhou classe própria (`.ready-teamvs`), pois compartilhava a classe do círculo "VS" do cabeçalho; a animação da grade do lobby é desligada no tema do 2v2.
- **Torneio · fogos de artifício:** o fundo do lobby do torneio ganhou fogos de artifício animados em canvas (explosões em esfera, anel e bicolor, com rastro), que só rodam enquanto o lobby está visível.
- **Torneio · troféu em wireframe:** o troféu do lobby agora é só arestas, sem preenchimento, e as faces que nunca aparecem (tampas e faces escondidas dentro de outros prismas) deixaram de ser renderizadas, melhorando o desempenho da interface.
- **Torneio · tela VS:** a duração da tela VS entre as lutas passou de 3,8 s para 5,5 s (`VS_MS`).
- **Plataformas móveis:** esmalte (frasco e respingos), copos e microfones não ficam mais estáticos no ar; respingos de esmalte e estilhaços passam a colidir com plataformas flutuantes.
- **Demonstração:** novo visual (cartela VS, "LUTE!", "K.O.!", selo, barra de progresso, vinheta, partículas e nomes reais dos personagens), sem mudar o comportamento da luta.
- **IA das CPUs:** comportamento mais humano (personalidade, plano de jogo, combos, armadilhas, erros e humor).
