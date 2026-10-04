# 🥊 Renan Toons Wacky Wars

Jogo de luta multiplayer online no estilo **Super Smash Bros.**, feito em um único arquivo HTML com Canvas 2D e JavaScript puro. Até **4 jogadores** lutam em um palco, acumulando dano (%) e sendo lançados cada vez mais longe até caírem para fora da tela.

## ✨ Recursos

- **Multiplayer P2P** via WebRTC ([PeerJS](https://peerjs.com/)), sem servidor próprio: um jogador cria a sala e os outros entram com o código.
- **7 personagens jogáveis**, cada um com estilo próprio (veja abaixo).
- **4 mapas** com cenários e mecânicas diferentes: *Cubos*, *Colinas*, *DORFic* e *Vector*.
- **Combate estilo Smash**: dano em %, knockback crescente, hitstun, hitstop, tremor de câmera, faíscas, linhas de impacto e raio em golpes quase fatais.
- **3 vidas** por jogador; o último de pé vence.
- **Tela de vitória 2.5D** com o campeão em destaque.
- **Luta de demonstração:** parado na tela de título por ~15 segundos, duas CPUs lutam por 60 segundos usando o motor real do jogo (veja [Luta de demonstração](#-luta-de-demonstração)).
- **Galeria de personagens** no menu de modo de jogo, com sprite em destaque e a história de cada lutador.
- **Controles de toque** para celular e tablet (joystick + botões).
- **Vibração de controle** (Gamepad API) em golpes fortes, quando o navegador suporta.
- **Efeitos sonoros sintetizados** com a Web Audio API (nenhum arquivo de áudio necessário).
- Intro animada da logo *Renan Studios* (com botão de pular), tela de título, menu de modo de jogo e sala de espera com seleção de personagem e cursores.
- Mundo com tamanho fixo (1600×900) escalado para qualquer janela, então a física é idêntica para todos os jogadores.

## 🎮 Como jogar

1. Abra o jogo e clique em **"TOQUE PARA COMEÇAR"**.
2. Digite seu nome.
3. No menu de modo de jogo, escolha **Multijogador** (o modo *Um Jogador* ainda está em breve). A **Galeria** mostra os personagens e suas histórias.
4. **Criar Sala:** clique em *Criar Sala* e compartilhe o código exibido.
   **Entrar:** vá na aba *Entrar*, digite o código e clique em *Entrar na Sala*.
5. Escolha seu personagem e clique em **Pronto**. O host também escolhe o **mapa** e inicia a partida.
6. Após a contagem (3, 2, 1, LUTAR!), derrube os adversários. Quem cair para fora do palco perde uma vida.
7. Ao fim da partida, pressione **R** (ou toque na tela, no celular) para jogar novamente.

## ⌨️ Controles

O jogo usa teclado ou, em dispositivos com tela de toque, controles na tela.

| Tecla | Ação |
| --- | --- |
| ← / → | Andar |
| ↑ | Pular |
| ↓ | Modificador (muda o ataque do **Z** ou do **X** para alguns personagens) |
| Z | Ataque principal |
| X | Ataque secundário |
| R | Jogar novamente (fim de partida) |

### 📱 Controles de toque

Em dispositivos com tela de toque, a tela da luta mostra:

- **Joystick (esquerda):** esquerda/direita andam, para cima pula e para baixo funciona como **↓**.
- **Botões Z e X (direita):** equivalem às teclas **Z** e **X**.
- **Tela de vitória:** toque em qualquer lugar da tela para jogar novamente (equivale ao **R**).

Os controles só aparecem durante a luta.

## 🧑‍🎤 Personagens

| Personagem | Z | ↓ + Z | X | ↓ + X |
| --- | --- | --- | --- | --- |
| **Renanzinho** | Combo de soco (2 golpes). *No ar:* **pulo duplo**, soltando um balde | Arremessa um **balde na corda**, que só pode ser lançado de novo depois de voltar para a mão | Chute (finaliza o combo) | – |
| **Aggie** | Arremessa esmalte, que explode em líquido após ~2 s no chão | Arremessa alicate de unha (estilo bumerangue) | Arranhão de garra (curto alcance) | – |
| **Renan Antigo** | Atira balas rápidas. **Segure** para um tiro carregado | **Escudo** temporário (2 s) que reflete dano em quem chegar perto | **Dash com soco:** avança rápido e acerta quem estiver na frente (no ar, só um por salto) | – |
| **Blui e Reddie** | Soco | – | Cospe rajada de mini *jelly balls* com física de gelatina | – |
| **Doper** | Arremessa um copo que quica até parar; depois de parado, estilhaça se um oponente encostar, causando dano em área (até 3 copos no mapa) | – | **Empurrão de gerente:** golpe curto com um pequeno avanço, dano baixo e knockback alto | Toca o sax e solta uma nota musical voadora que ondula e causa dano |
| **Hofy** | **Canta** e solta uma nota que ondula e **atravessa** os oponentes | – | **Taca o microfone**, que desliza pelo chão até acertar alguém ou cair no abismo | – |
| **FooshLooket** | *No chão:* **cospe água** em arco; onde o jato pousa vira uma poça. *No ar:* **pulo duplo** | – | Vira uma **bola** e quica para a frente, causando dano ao acertar | – |

### Detalhes e curiosidades

- **Renan Antigo** é ~35% mais rápido que os demais (perfil *rushdown*).
- **Doper** (gerente do Blue Note Bar) é o maior do elenco; **Hofy** (dançarina, cantora e namorada do Doper) é um pouco menor que ele.
- **Hofy** não tem pulo duplo.
- **FooshLooket:** quem pisa na poça escorrega, leva dano, dá uma cambalhota e é jogado para fora dela (o dono não tropeça). Cada FooshLooket só mantém **uma poça** por vez (a nova apaga a antiga) e o cuspe tem recarga de ~3 s, sem travar o ataque **X**. O pulo duplo dele não solta balde.
- **Renanzinho** é o único com combo de três golpes (soco, soco e chute); os demais personagens misturam golpes simples com projéteis e armadilhas.

## 🗺️ Mapas

- **Cubos:** céu com cubos-montanha ao fundo e duas pequenas plataformas flutuantes sobre o palco.
- **Colinas:** campo aberto com colinas ao fundo, sem plataformas flutuantes.
- **DORFic:** cenário alaranjado de pôr do sol, sem plataformas flutuantes.
- **Vector:** palco de show com mecânicas próprias.
  - **3 plataformas móveis:** duas sobem e descem em contrafase, e uma varre o topo do cenário e **carrega quem estiver em cima**.
  - **2 caixas de som** no palco funcionam como **trampolins**: pisar nelas lança o jogador para cima e restaura o pulo duplo e o dash aéreo.
  - O movimento das plataformas é sincronizado pelo **relógio do host**, então todos veem a mesma coisa.

Somente o host escolhe o mapa na sala de espera.

## 🤖 Luta de demonstração

Se ninguém mexer em nada na tela de título por cerca de **15 segundos**, o jogo entra em modo de demonstração:

- Dois personagens aleatórios e um mapa aleatório (exceto o *Vector*, que depende do relógio do host) entram em cena, com fade preto na entrada e na saída.
- A luta dura **60 segundos** e usa o motor real do jogo (golpes, dano, KO e HUD). Nada é enviado pela rede.
- Ninguém é eliminado de vez: quem cai reaparece no palco e a luta continua até o tempo acabar.
- **Mover o mouse não conta.** Qualquer tecla, clique ou toque encerra a demonstração e volta ao título (essa primeira interação não "clica" no título por trás).

### IA das CPUs

As CPUs usam o **moveset completo** de cada personagem e tomam decisões em vez de agir aleatoriamente:

- **Modos de luta:** neutro, pressão, finalização (oponente com dano alto), jogo seguro (CPU perdendo), espera (contra escudo) e guarda de borda (oponente caiu).
- **Escolha de golpe por pontuação:** considera alcance, papel do golpe (*poke*, finalizador, zona, armadilha), dano do oponente, repetição recente e o que vem acertando na luta.
- **Estilos por personagem** (brigão, zoner, armadilheiro, equilibrado), além de tempo de reação e habilidade aleatórios para cada CPU.
- **Leitura do jogo:** esquivam de projéteis, fogem de esmaltes prestes a explodir, evitam pisar em poças e copos armados, reagem a golpes do oponente (escudo ou recuo) e voltam ao palco com pulo duplo ou dash quando caem.

## 🚀 Executando

O jogo é o arquivo `index.html`, mas ele carrega imagens de pastas ao lado dele. Mantenha esta estrutura:

```
index.html
game_logo.png
rs_logo.png
icon.png
Renanzinho Sprites/
Aggie Sprites/
Renan Antigo Sprites/
Blui & Reddie Sprites/
Doper Sprites/
Hofy Sprites/
FooshLooket Sprites/
```

Para jogar localmente, sirva a pasta com um servidor estático:

```bash
python3 -m http.server 8000
# abra http://localhost:8000
```

Para jogar online com amigos, hospede a pasta em qualquer serviço de hospedagem estática (GitHub Pages, Netlify, Vercel etc.) e compartilhe o link, cada jogador abre o mesmo endereço e usa o código da sala.

### Requisitos

- Navegador moderno com suporte a Canvas, WebRTC e Web Audio (Chrome, Edge, Firefox, Safari).
- Conexão com a internet: o **PeerJS** (via unpkg) e o **Font Awesome** (via cdnjs) são carregados por CDN, e o PeerJS usa um broker público para conectar os jogadores.
- Teclado ou tela de toque. Vibração de controle é opcional e depende do navegador.

## 🛠️ Detalhes técnicos

- **Rede:** topologia com host. O host retransmite as mensagens de estado (posição, animação, dano, vidas, ataques, projéteis) para todos os clientes. Limite de 4 jogadores por sala. Cada projétil tem um **dono**, e só o dono detecta o acerto e propaga o dano pela rede.
- **Sincronização de mapa:** o mapa *Vector* usa o relógio do host, com correção de deriva, para manter plataformas e animações iguais para todos.
- **Renderização:** Canvas 2D, com câmera que acompanha e dá zoom nos jogadores, parallax de fundo e sprites animados.
- **Física:** gravidade, pulo, pulo duplo e colisão com o palco e com plataformas flutuantes em coordenadas fixas do mundo.
- **Personagens:** registro `CHARACTERS` no código, com sprites e definições de ataque (`duration`, `damage`, `baseKnockback`, `growth`, `range`, `cooldown`). Atributos opcionais como `sizeScale` (tamanho do desenho e da hitbox) e `moveSpeedMultiplier` (velocidade) ajustam cada lutador. Há **11 vagas "Em Breve"** na seleção.
- **Demonstração:** o objeto `DEMO` reaproveita a física e os golpes do jogador real, trocando temporariamente quem é o "jogador local". Cada personagem tem um *kit* (`KITS`) que descreve seus golpes, alcance, papel tático e condições de uso.

## 🧩 Adicionando um personagem

1. Adicione uma pasta de sprites (`idle`, `walk`, etc.).
2. Registre o personagem em `CHARACTERS` com seus sprites e `attacks`.
3. Adicione um `.char-slot.playable` com `data-character` na tela de seleção.
4. Implemente os comandos de `Z`/`X` no handler de `keydown`.
5. Adicione uma entrada na lista da **Galeria** (nome, sprite, descrição e selo).
6. *(Opcional)* Registre um kit em `KITS` para o personagem poder aparecer nas lutas de demonstração.

## 📄 Créditos

Criado por **Renan Studios**. Arte, personagens e código originais.
