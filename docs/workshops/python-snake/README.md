---
title: 'Snake, o jogo da cobrinha'
description: 'Crie o clássico Snake com Python'  
bg-image: "/workshops/python-snake/img/snakepy.png"
permalink: /workshops/python-snake/
order: 5
---

<center>Snake, o jogo da cobrinha</center>  
<center>Feito por <a href="https://github.com/kyryloren" target="_blank">@kyryloren</a></center>
<center>Traduzido por <a href="https://github.com/vitorvavolizza" target="_blank">@vitorvavolizza</a></center>

<br />

Snake é um jogo que a maioria de nós jogou naqueles antigos telefones Nokia. É também um desafio clássico de programação para aprender uma nova linguagem. Por mais complicado que possa parecer à primeira vista, é bastante fácil de codificar e ocupa menos de 50 linhas. Vamos tentar recriar este jogo no terminal.

[![Snake e demo no repl.it](https://cloud-h8v6zt88z.vercel.app/0snakereplit.gif)](https://repl.it/@kyryloorlov/Snake-Game)

Você pode verificar e visualizar o código-fonte [aqui](https://repl.it/@hcbjcentro/snakepy).

## Começando

Vamos usar o [repl.it](https://repl.it), um editor de código online gratuito para este projeto. Crie um novo projeto Python acessando [https://repl.it/languages/python3](https://repl.it/languages/python3).

## Declarando dependências

Vamos usar as bibliotecas `random` e `curses` para nos ajudar. o `random` é um módulo que nos permitirá dar posições aleatórias aos frutos que irão aparecer no mapa. A `curses` ajudará a gente a lidar com a interface do usuário e a mecânica do jogo.

Importe essas duas bibliotecas adicionando as seguintes linhas no início do arquivo `main.py`:

```python
import random
import curses
```

## Inicializando a tela
Abaixo de nossas importações, vamos pular uma linha e começar nosso jogo. Primeiro, temos que declarar de alguma forma que nosso terminal pode ser usado como uma interface de usuário. Usaremos a `curses` para definir o cursor, a largura da tela, a altura da tela e quão rápido a cobra irá se mover.

```python
# Define a tela
s = curses.initscr()

# Coloca o cursor na posição 0 para ficar invisível
curses.curs_set(0)

# Obtém a largura e a altura
sh, sw, = s.getmaxyx()

# Cria uma nova janela com a altura e largura no canto superior esquerdo
w = curses.newwin(sh, sw, 0, 0)

# Ativa todas as teclas
w.keypad(1)

# Determina o quão rápido a cobra se move
w.timeout(100)
```

Ótimo! Definimos as dimensões da tela e configuramos o cursor para ficar invisível no canto superior esquerdo. Vamos passar para a lógica da cobra. Usaremos essas variáveis e definições ao criar o resto do programa.

![Happy snake](https://cloud-5uzl1njgm.vercel.app/0snek.gif)

## Inicializando a cobra e a comida
Pule uma linha do código acima e vamos definir a cobra, sua posição e comida.

```python
# A posição X inicial da cobra
cbr_x = sw / 4

# A posição Y inicial da cobra
cbr_y = sh / 2

# Cria as partes iniciais do corpo da cobra
cobra = [
     [cbr_y, cbr_x],
     [cbr_y, cbr_x - 1],
     [cbr_y, cbr_x - 2]
]

# Define a primeira comida no centro da tela
comida = [sh / 2, sw / 2]
```

Pense na cobra como um grupo de blocos. O primeiro bloco é a cabeça. A cabeça está nas posições iniciais `cbr_x` e `cbr_y`. Isso significa que a próxima parte do corpo deve estar 1 a menos na posição X do que a cabeça. A próxima parte do corpo deve estar a 2 X a menos da cabeça. A próxima 3 X e assim por diante. Cada item da lista `cobra` é uma posição inicial da parte do corpo.

Vamos adicionar essa comida à tela.

```python
# Adicione a comida à tela

w.addch(int(comida[0]), int(comida[1]), curses.ACS_PI)
```

No código acima, usamos as posições X e Y do alimento, bem como o caractere ASCII PI para definir o primeiro alimento. Estamos usando o símbolo PI como alimento.

Vamos adicionar a direção inicial da cobra. Vamos configurá-lo para dar certo.
```python
tecla = curses.KEY_RIGHT
```

![Dança](https://cloud-qco33gkwh.vercel.app/0dancing.gif)

## Lidar com movimento e lógica do jogo
Agora, vamos lidar com a lógica do jogo. Queremos que o seguinte código seja executado continuamente, então vamos colocá-lo dentro de um loop infinito:

```python
# Loop infinito sendo repetido toda vez que a cobra se move
while True:
  prox_tecla = w.getch()
  tecla = tecla if prox_tecla == -1 else prox_tecla
```

O código acima verifica cada tecla seguinte. Se a próxima tecla for igual a -1, deixamos a variável `tecla` como está, caso contrário, definimos a variável `tecla` para o valor `prox_tecla`.

Vamos cuidar do caso em que o jogador perdeu. Como um jogador perderia no Snake?

```python
# Loop infinito sendo repetido toda vez que a cobra se move
while True:
  prox_tecla = w.getch()
  operacao_errada = True if (prox_tecla==-1 or prox_tecla==curses.KEY_DOWN and tecla == curses.KEY_UP
                          or tecla==curses.KEY_DOWN and prox_tecla == curses.KEY_UP
                          or prox_tecla==curses.KEY_LEFT and tecla == curses.KEY_RIGHT
                          or tecla==curses.KEY_LEFT and prox_tecla == curses.KEY_RIGHT) else False  
  tecla = tecla if operacao_errada else prox_tecla

  # Cuida do caso quando o jogador perde
  if cobra[0][0] in [0, sh] or cobra[0][1]  in [0, sw] or cobra[0] in cobra[1:]:
      # Fecha a janela da curses e finaliza o programa
      curses.nocbreak()
      s.keypad(False)
      curses.echo()
      curses.endwin()
      print("Ops, você perdeu!")
      break
      quit()
```

As primeiras linhas deste trecho testam todas as combinações possíveis que podemos mover e para onde estamos nos movendo atualmente. Fazemos isso para garantir que o jogo não pare se, por exemplo, estamos indo para frente e pressionamos para ir para trás.

No código acima, também definimos uma instrução if para verificar se:
- A posição Y da cobra está fora dos limites da tela
- A posição X da cobra está fora dos limites da tela
- A cobra está dentro dela mesma

Se alguma das situações acima acontecer, fechamos a janela da curses e encerramos o programa.

Agora, vamos determinar a nova cabeça da cobra com base em nosso movimento.

```python
while True:
  # Código que escrevemos anteriomente...

  nova_cabeca = [cobra[0][0], cobra[0][1]]

  # Jogador pressiona tecla para baixo
  if tecla == curses.KEY_DOWN:
      nova_cabeca[0] += 1
  # Jogador pressiona tecla para cima
  if tecla == curses.KEY_UP:
      nova_cabeca[0] -= 1
  # Jogador pressiona tecla para esquerda
  if tecla == curses.KEY_LEFT:
      nova_cabeca[1] -= 1
  # Jogador pressiona tecla para direita
  if tecla == curses.KEY_RIGHT:
      nova_cabeca[1] += 1

  # Insere nova cabeça na cobra
  cobra.insert(0, nova_cabeca)
```

O código acima é como realmente controlamos a cobra. Começamos pegando a velha cabeça da cobra e verificando qual tecla está sendo pressionada. Se a tecla for pressionada, pegamos a posição Y da cobra e adicionamos 1 a ela. Se a tecla para cima for pressionada, pegamos a posição Y da cobra e subtraímos 1. Se a tecla esquerda for pressionada, pegamos a posição X da cobra e subtraímos 1. Se a tecla direita for pressionada, pegamos a posição X da cobra e adicionamos 1.

Depois de tudo isso, temos algo assim:
![Comida de cobra](https://cloud-78ocq7ahv.vercel.app/0screen_shot_2020-12-22_at_9.10.17_am.png)

## Lidando com a lógica da comida
Agora vamos lidar com a cobra correndo na comida. O código a seguir deve ir para a instrução `while True`, abaixo do código acima.

```python
# Checa se a cobra encostou em uma comida
if cobra[0] == comida:
    # Já que a cobra comeu a comida, precisamos colocar uma nova posição para a comida
    comida = None
    while comida is None:
      # Randomiza a posição de uma comida nova
      nf = [
          random.randint(1, sh-1),
          random.randint(1, sw-1)
      ]
      # Coloca a comida se a nova comida não está dentro da cobra
      comida = nf if nf not in cobra else None
    # Posiciona a nova comida na tela
    w.addch(comida[0], comida[1], curses.ACS_PI)
else:
    # Cuida de quando a cobra não encosta na comida
    cauda = cobra.pop()
    w.addch(int(cauda[0]), int(cauda[1]), ' ')

try:
    w.addch(int(cobra[0][0]), int(cobra[0][1]), curses.ACS_CKBOARD)
except:
    print("Ops, você perdeu!")
```

No código acima, verificamos se a cobra bateu na comida. Se sim, precisamos definir uma nova posição para o alimento e tornar a cobra mais longa. Usamos as dimensões de largura e altura da tela para randomizar as coordenadas para a nova posição da comida. Essas coordenadas aleatórias têm uma chance de serem colocadas exatamente onde a cobra está. Então, para evitar essa confusão, só colocamos o novo alimento se não estiver na cobra. Caso contrário, apenas repetimos o loop.

Observe na parte inferior que envolvemos a lógica para adicionar uma parte do corpo em um bloco try-except. Esta é uma maneira hackeante de garantir que, quando perdemos o jogo, o programa diga algo e não simplesmente trave.

Depois, de qualquer forma, adicionaremos a cabeça da cobra à tela.

## O produto final:

![A comida da cobra perto do corpo da cobra](https://cloud-1l5hoiqcf.vercel.app/0image3.png)

**Terminamos!** Esta é a aparência de seu código agora:

```python
import random
import curses

s = curses.initscr()
curses.curs_set(0)
sh, sw, = s.getmaxyx()
w = curses.newwin(sh, sw, 0, 0)
w.keypad(1)
w.timeout(100)
cbr_x = sw / 4
cbr_y = sh / 2
cobra = [[cbr_y, cbr_x], [cbr_y, cbr_x - 1], [cbr_y, cbr_x - 2]]
comida = [sh / 2, sw / 2]
w.addch(int(comida[0]), int(comida[1]), curses.ACS_PI)
tecla = curses.KEY_RIGHT
while True:
  prox_tecla = w.getch()
  operacao_errada = True if (
    prox_tecla == -1
    or prox_tecla == curses.KEY_DOWN and tecla == curses.KEY_UP
    or tecla == curses.KEY_DOWN and prox_tecla == curses.KEY_UP
    or prox_tecla == curses.KEY_LEFT and tecla == curses.KEY_RIGHT 
    or
    tecla == curses.KEY_LEFT and prox_tecla == curses.KEY_RIGHT) else False
  tecla = tecla if operacao_errada else prox_tecla
  if cobra[0][0] in [0, sh] or cobra[0][1] in [0, sw] or cobra[0] in cobra[1:]:
    curses.nocbreak()
    s.keypad(False)
    curses.echo()
    curses.endwin()
    print("Ops, você perdeu!")
    break
    quit()
  nova_cabeca = [cobra[0][0], cobra[0][1]]
  if tecla == curses.KEY_DOWN:
    nova_cabeca[0] += 1
  if tecla == curses.KEY_UP:
    nova_cabeca[0] -= 1
  if tecla == curses.KEY_LEFT:
    nova_cabeca[1] -= 1
  if tecla == curses.KEY_RIGHT:
    nova_cabeca[1] += 1
  cobra.insert(0, nova_cabeca)
  if cobra[0] == comida:
    comida = None
    while comida is None:
      nf = [
        random.randint(1, sh-1),
        random.randint(1, sw-1)
      ]
      comida = nf if nf not in cobra else None
    w.addch(comida[0], comida[1], curses.ACS_PI)
  else:
    cauda = cobra.pop()
    w.addch(int(cauda[0]), int(cauda[1]), ' ')
    
  try:
    w.addch(int(cobra[0][0]), int(cobra[0][1]), curses.ACS_CKBOARD)
  except:
    print("Ops, você perdeu!")
```

Agora você deve conseguir jogar Snake no terminal! Você pode executar o código no link [https://repl.it/@hcbjcentro/snakepy](https://repl.it/@hcbjcentro/snakepy).

![Hackeando](https://cloud-adi3v03or.vercel.app/0hacking.gif)

## Hackeando
Agora você tem controle total sobre o código. Vá em frente e experimente-o para ver se consegue encontrar maneiras de torná-lo mais divertido. Aqui estão algumas ideias:

- Adicionar uma opção de jogar novamente: [Demo + Código](https://repl.it/@hcbjcentro/snake-novamente)
- Adicione cores à cobra: [Demo + Código](https://repl.it/@hcbjcentro/snake-com-coress)
- Crie uma pontuação: [Demo + Código](https://repl.it/@hcbjcentro/snake-com-pontuacao#main.py)

Divirta-se jogando e pegando maçãs! Happy Hacking!

Agora que você terminou de construir este maravilhoso projeto, compartilhe sua bela criação com outras pessoas! Lembre-se, é só mandar a URL do seu projeto!

Você provavelmente conhece as melhores maneiras de entrar em contato com seus amigos e familiares, mas se você quiser compartilhar seu projeto com a comunidade brasileira do Hack Club, não há melhor lugar para fazer isso do que no Discord do Hack Club Brasil.✨

1. Clique [aqui][discord]{:target="_blank"} para fazer parte da nossa comunidade!
2. Depois, poste o link do seu projeto no canal `💡┇criações` para compartilhá-lo com todos os Hack Clubbers!

A comunidade te espera!🎉🎉

[discord]: http://bit.ly/discord-hc-brasil