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

> Hofy e Dan não têm pulo duplo.

## Mapas

Cubos · Colinas · DORFic · **Vector**

O mapa **Vector** tem 3 plataformas flutuantes que se movem (duas sobem/descem, uma vai de um lado para o outro). Os outros mapas têm só o palco principal (Cubos tem plataformas flutuantes fixas).

## Multiplayer

Online via [PeerJS](https://peerjs.com/) (conexão P2P entre jogadores, com código de sala curto). Cada cliente simula a física dos projéteis localmente; só o dono de cada ataque detecta o acerto e avisa os outros.

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

HTML5 Canvas, JavaScript (sem build), PeerJS, Font Awesome (CDN) e Web Audio para os efeitos. O troféu do lobby usa CSS 3D e os fogos de artifício usam um `<canvas>` 2D próprio.

## Changelog

- **Torneio · fogos de artifício:** o fundo do lobby do torneio ganhou fogos de artifício animados em canvas (explosões em esfera, anel e bicolor, com rastro), que só rodam enquanto o lobby está visível.
- **Torneio · troféu em wireframe:** o troféu do lobby agora é só arestas, sem preenchimento, e as faces que nunca aparecem (tampas e faces escondidas dentro de outros prismas) deixaram de ser renderizadas, melhorando o desempenho da interface.
- **Torneio · tela VS:** a duração da tela VS entre as lutas passou de 3,8 s para 5,5 s (`VS_MS`).
- **Plataformas móveis:** esmalte (frasco e respingos), copos e microfones não ficam mais estáticos no ar; respingos de esmalte e estilhaços passam a colidir com plataformas flutuantes.
- **Demonstração:** novo visual (cartela VS, "LUTE!", "K.O.!", selo, barra de progresso, vinheta, partículas e nomes reais dos personagens), sem mudar o comportamento da luta.
- **IA das CPUs:** comportamento mais humano (personalidade, plano de jogo, combos, armadilhas, erros e humor).
