# 🥊 Renan Toons Wacky Wars

Jogo de luta multiplayer online no estilo **Super Smash Bros.**, feito em um único arquivo HTML com Canvas 2D e JavaScript puro. Até **4 jogadores** lutam em um palco, acumulando dano (%) e sendo lançados cada vez mais longe até caírem para fora da tela.

## ✨ Recursos

- **Multiplayer P2P** via WebRTC ([PeerJS](https://peerjs.com/)), sem servidor próprio: um jogador cria a sala e os outros entram com o código.
- **5 personagens jogáveis**, cada um com estilo próprio (veja abaixo).
- **3 mapas** com cenários diferentes: *Cubos*, *Colinas* e *DORFic*.
- **Combate estilo Smash**: dano em %, knockback crescente, hitstun, hitstop, tremor de câmera, faíscas, linhas de impacto e raio em golpes quase fatais.
- **3 vidas** por jogador; o último de pé vence.
- **Tela de vitória 2.5D** com o campeão em destaque.
- **Controles de toque** para celular e tablet (joystick + botões).
- **Efeitos sonoros sintetizados** com a Web Audio API (nenhum arquivo de áudio necessário).
- Intro animada da logo *Renan Studios*, tela de título e sala de espera com seleção de personagem e cursores.
- Mundo com tamanho fixo (1600×900) escalado para qualquer janela, então a física é idêntica para todos os jogadores.

## 🎮 Como jogar

1. Abra o jogo e clique em **"TOQUE PARA COMEÇAR"**.
2. Digite seu nome.
3. **Criar Sala:** clique em *Criar Sala* e compartilhe o código exibido.
   **Entrar:** vá na aba *Entrar*, digite o código e clique em *Entrar na Sala*.
4. Escolha seu personagem e clique em **Pronto**. O host também escolhe o **mapa** e inicia a partida.
5. Após a contagem (3, 2, 1, LUTAR!), derrube os adversários. Quem cair para fora do palco perde uma vida.
6. Ao fim da partida, pressione **R** (ou toque na tela, no celular) para jogar novamente.

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

| Personagem | Z | ↓ + Z | X |
| --- | --- | --- | --- |
| **Renanzinho** | Combo de soco (2 golpes) | – | Chute (finaliza o combo). *Z no ar* faz o **pulo duplo**, soltando um balde |
| **Aggie** | Arremessa esmalte, que explode em líquido após ~2 s no chão | Arremessa alicate de unha (estilo bumerangue) | Arranhão de garra (curto alcance) |
| **Renan Antigo** | Atira balas rápidas (rushdown, curto alcance) | Escudo temporário que reflete dano em quem chegar perto | – |
| **Blui e Reddie** | Soco | – | Cospe rajada de mini *jelly balls* com física de gelatina |
| **Doper** | Arremessa um copo que quica até parar; depois de parado, estilhaça se um oponente encostar, causando dano em área | – | **Empurrão de gerente:** golpe curto com um pequeno avanço, dano baixo e knockback alto. **↓ + X:** toca o sax e solta uma nota musical voadora que ondula e causa dano |

> O Renan Antigo é ~35% mais rápido que os demais. O Doper (gerente do Blue Note Bar) é um pouco maior que os outros.

## 🗺️ Mapas

- **Cubos**
- **Colinas**
- **DORFic**

Somente o host escolhe o mapa na sala de espera.

## 🚀 Executando

O jogo é o arquivo `index.html`, mas ele carrega imagens de pastas ao lado dele. Mantenha esta estrutura:

```
index.html
game_logo.png
rs_logo.png
Renanzinho Sprites/
Aggie Sprites/
Renan Antigo Sprites/
Blui & Reddie Sprites/
Doper Sprites/
icon.png
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
- Teclado ou tela de toque.

## 🛠️ Detalhes técnicos

- **Rede:** topologia com host. O host retransmite as mensagens de estado (posição, animação, dano, vidas, ataques) para todos os clientes. Limite de 4 jogadores por sala.
- **Renderização:** Canvas 2D, com câmera que acompanha e dá zoom nos jogadores, parallax de fundo e sprites animados.
- **Física:** gravidade, pulo, pulo duplo e colisão com o palco em coordenadas fixas do mundo.
- **Personagens:** registro `CHARACTERS` no código, com sprites e definições de ataque (`duration`, `damage`, `baseKnockback`, `growth`, `range`), facilitando adicionar novos lutadores (há 14 vagas "Em Breve" na seleção).

## 🧩 Adicionando um personagem

1. Adicione uma pasta de sprites (`idle`, `walk`, etc.).
2. Registre o personagem em `CHARACTERS` com seus sprites e `attacks`.
3. Adicione um `.char-slot.playable` com `data-character` na tela de seleção.
4. Implemente os comandos de `Z`/`X` no handler de `keydown`.

## 📄 Créditos

Criado por **Renan Studios**. Arte, personagens e código originais.
