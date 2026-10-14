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

Cubos · Colinas · DORFic · **Vector** · **Deserto** · **Lado de Fora** · **Media Player**

O mapa **Vector** tem 3 plataformas flutuantes que se movem (duas sobem/descem, uma vai de um lado para o outro). O **Deserto** tem a plataforma principal ondulando (veja a seção abaixo). O **Media Player** tem a barra de tarefas como chão, o topo da janela como plataforma alta e duas plataformas que sobem e descem. Os outros mapas têm só o palco principal (Cubos tem plataformas flutuantes fixas).

## Mapa Media Player

Cenário adaptado do `Glossows_Media_Player.html`: um tocador de mídia retrô azul, com lista de playlists à esquerda, tela quadriculada no centro com uma caixa de som 3D, controles de EQ, balanço e volume à direita e a barra de controles e a barra de tarefas embaixo.

- **Chão:** a própria barra de tarefas, de ponta a ponta da janela.
- **Plataforma de cima:** o topo da janela central. Ela vai da borda esquerda da janela central até onde começam as configurações (volume, balanço, EQ). No lado das configurações não há colisão de plataforma, para dar para embarcar na plataforma móvel sem bugar.
- **Plataformas móveis:** duas, uma à esquerda e outra à direita da janela, que sobem e descem em contrafase (ciclo de 3,2 s de ida, com o mesmo movimento suave do CSS original). No ponto mais baixo ficam uns 100 px acima do chão, dentro do alcance do pulo, e no mais alto ficam na altura do topo da janela, para dar para passar de uma para a plataforma de cima. Carregam quem está em cima. A largura delas é a constante `MP_PLAT_W`.
- **Morte:** cair para fora da janela do media player (pelos lados ou por baixo) faz o jogador perder uma vida. Os limites vêm dos campos `deathLeft` e `deathRight` do mapa.
- **Camadas e parallax:** o fundo quadriculado e a caixa de som 3D são recortados dentro da janela e se deslocam um pouco menos que o resto, o quadriculado um pouquinho menos que a caixa de som. A moldura do media player passa por cima dos dois, como se estivessem atrás dela. O resto da arte fica travado ao mundo, alinhado com o chão e a colisão. Os fatores de parallax estão em `drawMediaPlayerBackground`.
- **Escala:** a arte é desenhada a 110% do tamanho original (`MP_S`), com a barra de tarefas em y = 800. O topo da janela fica acima da tela, e a câmera mostra essa parte quando os jogadores sobem.
- **Sincronia no online.** A posição das plataformas móveis usa o relógio do host (`vecNow()`, como o Vector e o Deserto), então todos veem e sentem as mesmas plataformas sem trocar mensagens.
- **Desempenho.** O HTML do mapa vira imagem SVG, que o navegador rasteriza de novo a cada desenho. Por isso as camadas são desenhadas uma única vez em canvas na memória (`mpAssets`) e reaproveitadas. No celular (`DORFIC_LITE`) o cache usa 80% da resolução. As posições das plataformas são calculadas uma vez por frame (cache de 4 ms), não há sombras com blur e o quadriculado é desenhado direto no canvas com um padrão repetido.
- **Implementação:** bloco `MAPA "Media Player"` do `index.html` (`getMediaPlayerStage`, `getMediaPlayerPlatforms`, `drawMediaPlayerBackground`, `drawMediaPlayerStageFill`, `drawMediaPlayerPlatform`; constantes `MP_*`). A barra de tarefas é redesenhada no espaço do mundo, exatamente sobre a plataforma principal, em `drawMediaPlayerStageFill`.

## Mapa Deserto

Cenário de arenito com céu azul, montanhas rosadas, torres e estruturas, e duas faixas de chão de areia, todas com *parallax* e rolando sozinhas para a esquerda. Por cima fica a plataforma de arenito (3 faixas de cor, contorno marrom e fileira de pontos laranja no meio).

### Plataforma ondulante

A cada ciclo de 18 s, a plataforma **inteira** (topo incluso) ondula durante 7,2 s, com uma onda que viaja pela peça, e depois volta a ficar reta. O topo curvo é **o chão de verdade**: os personagens andam, pulam e pousam sobre a curva e as diagonais.

- **Desenho e física usam o mesmo cálculo** (`desertoLift`), então o que se vê é exatamente onde se pisa. O comentário antigo do código dizia que o topo era reto justamente para a física (chão plano) bater com o desenho; isso deixou de ser verdade.
- **A onda só sobe.** O sistema de relevo do jogo (`groundOffset(x, stage)` do mapa, lido por `getGroundYAt`) só aceita chão que sobe, nunca um vale abaixo do topo do palco. Por isso a peça sobe de 0 a cerca de 50 px (num palco de 620 px) e volta; nos vales da onda ela encosta no nível de repouso. A amplitude é a constante `DESERTO_WAVE_AMP` (fração do comprimento do palco, `0.04`; a peça sobe até 2× isso).
- **Inclinação suave.** A inclinação máxima da superfície é cerca de 0,31, bem abaixo do limite de parede da física (`SLOPE_MAX_RATIO = 2.5`), então não há degraus nem bloqueios.
- **O chão carrega quem está parado.** Como a onda é animada, o chão sobe por baixo de quem está apoiado. O pouso original foi feito para relevo estático e deixaria o personagem atravessar o chão; a regra `groundCarry` (limite `GROUND_CARRY_MAX`) faz o chão levá-lo junto. Só vale para mapas com `movingGround: true`, então os outros mapas não mudam. Na simulação (60, 30 e 15 fps, parado e andando) não houve nenhum atravessamento nem bloqueio.
- **Rede de segurança contra atravessar o chão.** O `groundCarry` só cobre subidas pequenas por frame. Se o chão subir por cima do jogador de uma vez (pico de lag, ressincronização do relógio do host, nascer em cima da onda), a regra `groundEmbed` empurra o jogador para a superfície em vez de deixá-lo atravessar. Ela vale quando os pés estão até 160 px abaixo da superfície e ainda acima do fundo do palco, então quem cai pela borda continua caindo. Vale para mapas com onda (`movingGround`) e com relevo (Colinas).
- **Poças acompanham a onda.** As poças de tinta (Dan) e de água (FooshLooket) ficam coladas ao chão e inclinam com ele (`puddleOnHill`, `puddleGroundY`). Projéteis e objetos arremessados já tratavam o corpo do palco como sólido.
- **Sincronia no online.** A onda usa o relógio do host (`vecNow()`, o mesmo do mapa Vector), então todos veem e sentem a mesma onda sem trocar mensagens. Funções: `desertoWaveTc`, `desertoWaveEnv`, `desertoLift`, `desertoLiftAt`.

### Otimização para celular

O fundo já era pintado uma vez em canvas offscreen e desenhado por *pattern fill*. Em aparelhos touch (`DORFIC_LITE`) foi otimizado ainda mais:

- **Resolução do ladrilho pela escala real da tela** (`desertoLayerScale`): acompanha a escala de render do aparelho (com folga para o zoom da câmera), em múltiplos de 1/8, e nunca passa de 1×, o valor antigo. Em paisagem a memória dos ladrilhos cai de 6,7 MB para cerca de 2,6 MB (−61%); em retrato, para cerca de 0,9 MB. Notebooks com touch e desktops ficam como antes (desktop segue em 2×). Ao girar o aparelho os ladrilhos são refeitos só se ficariam borrados, e a barra de URL do navegador não dispara rebuild.
- **Construção incremental e pré-aquecimento** (`desertoBuildStep`, `desertoPrewarm`): as 4 camadas são pintadas em tempo ocioso, uma por vez, logo depois do carregamento, para que escolher o Deserto não trave o primeiro frame. Se a partida começar antes, o desenho termina o que faltar. Os bitmaps antigos são liberados na hora (`desertoFreeLayers`).
- **Sem alocação por frame:** os gradientes do céu e da névoa ficam em cache.
- **Palco mais leve** (vale em todos os dispositivos): a onda é calculada uma vez por ponto (e não ~500 vezes por frame), a peça é desenhada com bordas retas quando está parada (cerca de 60% do ciclo), e os pontos laranja saem num único `fill` (de 29 para 4 `fill`s por frame). No celular a onda usa segmentos de 14 px em vez de 8 px (desvio máximo de ~0,36 px da curva exata). A geometria é idêntica à da versão anterior.

> Esses números vêm de simulação e cálculo, não de medição em aparelho. Se algo estranho aparecer no celular, o painel Performance do Chrome conectado ao aparelho é a leitura mais confiável. Na intro (câmera com zoom 1,8×) o fundo distante pode ficar um pouco mais suave no celular; para mais nitidez, aumente a folga `1.25` em `desertoLayerScale`, ao custo de memória.

## Mapa Lado de Fora

Cenário adaptado do `Céu.html` (CSS para canvas): céu azul com nuvens à deriva, muro de tijolos ciano, telhado cinza e, na frente, a plataforma verde onde se luta.

- **Plataforma sem fim à esquerda.** O chão se estende muito além da tela e só termina na direita (`LADO_RIGHT`, x = 1300). A área de luta começa em x = 300.
- **Câmera travada à esquerda.** A borda esquerda da tela nunca passa de `LADO_CAMERA_LEFT` (x = 100), mesmo com zoom. O limite vem do campo `cameraLeft` do mapa, então outros mapas podem usá-lo.
- **Morte pela esquerda.** Quem cruzar `LADO_DEATH_LEFT` (x = 50, logo depois da borda da tela) não cai: morre e perde uma vida. O campo `deathRight` do mapa faz o mesmo pela direita (usado no Media Player).
- **Parallax.** Céu (0,01), nuvens (0,05) e muro de tijolos (0,5) rolam em velocidades diferentes, e as nuvens também se movem sozinhas.
- **Implementação:** bloco `MAPA "Lado de Fora"` do `index.html` (`ladoForaAssets`, `drawLadoForaBackground`, `drawLadoForaStageFill`; constantes `LADO_*`). Os gradientes ficam em cache, sem alocação por quadro.

## Início da partida (entradas e 3, 2, 1, LUTAR!)

Os jogadores entram em cena um por vez, depois vem a contagem 3, 2, 1, LUTAR!, e só então os controles são liberados.

- **Ponto de nascimento na altura real do chão.** Cada jogador nasce no seu ponto da entrada (`introSlot`) na altura do chão **naquele ponto** (`getGroundYAt`), e não na altura de repouso do palco. Em mapas com ladeira (Colinas) ou com a onda ativa (Deserto), o chão pode estar acima do repouso, e nascer na altura de repouso deixava o jogador enterrado, caindo através do chão e perdendo uma vida antes da luta.
- **Ninguém perde vida na entrada nem na contagem.** Enquanto os controles estão travados (`introLock`), se alguém sair da área jogável por qualquer motivo, ele volta ao ponto de nascimento sem perder vida e sem tocar o som de queda. Depois do LUTAR!, a regra normal volta: cair para fora tira uma vida.

## Caixa de sugestões

Na tela **Modo de Jogo** há um quarto botão, **Sugestões** (com uma lâmpada), no mesmo visual dos outros itens do menu: ícone redondo, texto vermelho em itálico, entrada animada e destaque ao passar o mouse. Ele abre uma janelinha onde o jogador escreve o nome e a sugestão (novo personagem, mapa, golpe, correção...).

- **Regras:** nome obrigatório, sugestão com 5 a 1000 caracteres, intervalo de 60 s entre envios e um campo escondido contra robôs. As teclas digitadas na janela não movem o personagem nem acionam atalhos do jogo.
- **Envio:** com o **EmailJS** configurado (3 chaves no topo do bloco `CAIXA DE SUGESTÕES`), a sugestão chega direto no e-mail do Renan, sem o jogador sair do jogo. Sem as chaves, abre uma nova mensagem do **Gmail** no navegador, já preenchida com destinatário, assunto e texto (antes abria o app de e-mail padrão).

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

## Plataformas móveis (mapas Vector e Media Player)

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

Depois de 15 s parado na tela de título, dois lutadores controlados por CPU lutam por 60 s com o motor real do jogo (nada é enviado pela rede) e depois volta ao título. Qualquer tecla ou toque encerra a demonstração. Os mapas Vector e Media Player não entram no sorteio da demonstração, porque dependem do relógio do host. O Deserto entra no sorteio; a onda dele ainda não foi testada com os bots, então, se algum travar nela, basta excluí-lo do sorteio como é feito com o Vector.

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

HTML5 Canvas, JavaScript (sem build), PeerJS, Font Awesome (CDN) e Web Audio para os efeitos. O troféu do lobby do torneio e o boneco do fundo do 2v2 usam CSS 3D, os fogos de artifício usam um `<canvas>` 2D próprio, e o fundo e os botões do 2v2 usam apenas gradientes e animações CSS. O mapa Media Player usa uma imagem SVG gerada a partir do HTML da arte (`foreignObject`), desenhada uma vez em canvas na memória. O EmailJS (opcional) envia as sugestões por e-mail.

## Changelog

- **Início da partida · spawn corrigido:** o jogador agora nasce na altura real do chão no seu ponto de entrada, e não na altura de repouso do palco. Antes, em mapas com ladeira ou com a onda do Deserto ativa, ele nascia enterrado, atravessava o chão e perdia uma vida ainda no 3, 2, 1. Durante as entradas e a contagem, quem sair da área volta ao ponto de nascimento sem perder vida.
- **Deserto · correção do chão:** o chão não deixa mais o jogador atravessar quando a onda sobe de repente por cima dele (lag, ressincronização do relógio do host, nascer na onda). A regra `groundEmbed` empurra o jogador de volta para a superfície, e também vale para mapas com relevo. Quem cai pela borda continua caindo.
- **Novo mapa · Media Player:** tocador de mídia retrô adaptado do `Glossows_Media_Player.html`. A barra de tarefas é o chão, o topo da janela central é uma plataforma alta e há duas plataformas móveis que sobem e descem em contrafase. A caixa de som 3D e o fundo quadriculado têm parallax em camadas atrás da moldura. Cair fora da janela faz perder uma vida. Arte desenhada uma vez em canvas na memória, para não travar.
- **Mapas · campo `deathRight`:** além do `deathLeft` do Lado de Fora, os mapas agora também podem definir um limite de morte à direita.
- **Demonstração:** o Media Player também fica fora do sorteio de mapas, por depender do relógio do host.
- **Novo mapa · Lado de Fora:** plataforma verde sem fim à esquerda, com céu, nuvens, muro de tijolos e telhado em parallax; câmera travada à esquerda e morte ao passar da borda da tela.
- **Caixa de sugestões:** novo botão no menu de modo de jogo, no mesmo estilo dos demais, que abre um formulário de nome e sugestão; envio por EmailJS ou, como plano B, por uma mensagem nova do Gmail (antes abria o app de e-mail padrão).
- **Deserto · plataforma ondulante também no topo:** a onda que antes só mexia na face de baixo agora move a peça inteira, e o topo curvo é o chão da física (personagens andam sobre curvas e diagonais sem bugs). A onda só sobe (0 a ~50 px) e usa o relógio do host, então é igual para todos no online.
- **Deserto · física do chão animado:** o chão passa a carregar quem está parado sobre ele (`groundCarry`) em vez de deixá-lo atravessar; poças de tinta e de água acompanham a onda. Só afeta o Deserto.
- **Deserto · otimização para celular:** ladrilhos do fundo com resolução pela escala real da tela (−61% de memória em paisagem), pintura das camadas em tempo ocioso (sem travada ao entrar no mapa), gradientes em cache e palco mais leve (29 → 4 `fill`s por frame, bordas retas quando parado), com a mesma aparência.
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
