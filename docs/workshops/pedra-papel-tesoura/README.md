---

title: 'Pedra Papel Tesoura'
description: 'Crie um jogo de pedra papel tesoura com Python'  
bg-image: "/workshops/pedra-papel-tesoura/img/rps.png"
permalink: /workshops/pedra-papel-tesoura/
order: 7

---

<center>Crie um jogo de pedra papel tesoura com Python</center>  
<center> Feito por <a href="https://github.com/JackTDC" target="_blank">@JackTDC</a> </center>
<center>Traduzido por <a href="https://github.com/vitorvavolizza" target="_blank">@vitorvavolizza</a></center>

<br />

Você já se perguntou como construir um jogo com Python? Você sempre quis construir seu próprio jogo, mas não o fez porque achava que era difícil? Bem, hoje vou mostrar como construir um e mostrar o quão simples pode ser!

![homepage](https://cloud-8p13u30yt.vercel.app/rps.png)

Aqui está a [demo ao vivo][demo-ao-vivo] e o [código final][codigo-final].

[demo-ao-vivo]: https://repl.it/@hcbjcentro/pedra-papel-tesoura#main.py
[codigo-final]: https://repl.it/@hcbjcentro/pedra-papel-tesoura#main.py
[repl_it]: https://repl.it

## Parte 1: Pré-requisitos

Você deve saber um pouquinho de:
- Python

## Parte 2: Configuração

### Configurando seu ambiente de código no Repl.it

O [Repl.it](https://repl.it) é um editor de código online onde você pode construir seu jogo. Você não precisa usar o Repl.it, mas eu sugiro que você faça, pois ele configura tudo e não é necessário instalar nada.

Para começar, vá para [repl.it/languages/python](https://repl.it/languages/python). Seu ambiente de programação irá girar em apenas alguns segundos!

Você deve ver algo como o seguinte:

![arquivo main.py no Repl.it](https://cloud-lukrtynqs.vercel.app/repl.png)

## Parte 3: Fazendo o jogo
![GIF](https://cloud-931ehqeec.vercel.app/coding.gif)

### 1) Importando Módulos
Primeiro, precisamos importar o módulo `randint`. Para saber mais sobre o pacote randint, clique [aqui][randint].

[randint]: https://www.journaldev.com/36085/randint-method-in-python/

Na primeira linha do arquivo `main.py`, digite `from random import randint`. Isso importará o módulo randint.

### 2) Atribuindo valores às variáveis
Vamos fazer uma lista com todas as entradas que um jogador pode inserir. Em seu arquivo `main.py`, adicione:
```py
t = ["ped", "pap", "tes"]
```
'ped' significa 'Pedra', 'pap' significa 'Papel' e 'tes' significa 'Tesoura'.

Agora, vamos escrever um código para que o jogador possa inserir seu nome.
```py
nome = input("Digite seu nome:")
```

Agora, queremos dar ao jogador a opção de resetar o jogo.
```py
print("Digite 'resetar' para resetar a pontuação")
```

Para fazermos um sistema de pontuação, escreveremos o seguinte código:
```py
Voce = 0
PC = 0
```


Esse é o código até agora:
```py
from random import randint

t = ["ped", "pap", "tes"]

nome = input("Digite seu nome:")
print("Digite 'resetar' para resetar a pontuação")

Voce = 0
PC = 0
```

### 3) Mantendo o jogo em um loop
![loop](https://cloud-nmra250be.vercel.app/loop.gif)

Para isso, usarei um loop `while`, mas você também pode usar loops `for`!

Na parte inferior do arquivo `main.py`, adicione isto:
```py
while True: 
```

Agora, vamos escrever um código que escolha pedra, papel ou tesoura aleatoriamente para o computador.
```py
computador = t[randint (0,2)]
```

Em seguida, usaremos `input` para permitir que o jogador escolha `Pedra`, `Papel` ou `Tesoura`.
```py
jogador = input("Pedra, Papel, Tesoura?(ped, pap, tes)")
```

Existem 5 possibilidades:
1. O jogador escolhe `Pedra`
2. O jogador escolhe `Papel`
3. O jogador escolhe `Tesoura`
4. O jogador escreve `resetar`
5. A palavra inserida é inválida

### 4) Produzindo resultados separados para todas as possibilidades

Para facilitar as coisas estarei criando funções para imprimir se o jogador ganhar ou perder.
Primeiro vou escrever o seguinte código logo antes do loop `while`:
```py
mensagem = ""
```
Isso nos ajudará a imprimir mensagens diferentes, chamando a mesma função.

Também antes do loop `while` criarei a função que será chamada somente quando o jogador ganhar, para fazer isso usarei o seguinte código:
```py
def ganhou():
  global Voce
  Voce+=1
  print (mensagem)
  print('Computador =', PC, '\n', nome, '=', Voce)
```
Depois, irei fazer a função para quando o jogador perder.
```py
def perdeu():
  global PC
  PC+=1
  print (mensagem)
  print('Computador =', PC, '\n', nome, '=', Voce)
```
Agora chamaremos essas funções nos lugares certos.

Em primeiro lugar, vamos supor que o resultado padrão é um empate. Para fazer isso, digite o seguinte código dentro do loop `while`:
```py
while True:
    computador = t[randint (0,2)]
    jogador = input("Pedra, Papel, Tesoura?(ped, pap, tes)")
    if jogador == computador:
        print("Empate!")
        print('Computador =',PC)
        print(nome,'=',Voce)
```
Depois, colocamos um pouco de código para cada ação que pode ser tomada.
```py
    elif jogador == "ped":
        if computador == "pap":
            mensagem = "Você perdeu!, Papel cobre Pedra"
            perdeu()
        else:
            mensagem ="Você ganhou!, Pedra destrói Tesoura"
            ganhou()
```
Já esse, seria o código para papel e tesoura.
```py
    elif jogador == "pap":
        if computador == "tes":
            mensagem = "Você perdeu!, Tesoura corta Papel"
            perdeu()
        else:
            mensagem ="Você ganhou!, Papel cobre Pedra"
            ganhou()
    elif jogador == "tes":
        if computador == "ped":
            mensagem = "Você perdeu!, Pedra destrói Tesoura"
            perdeu()
        else:
            mensagem ="Você ganhou!, Tesoura corta Papel"
            ganhou()
```
Para a última parte, assumiremos que o jogador quer resetar o jogo ou digitou algo inválido.
```py
    elif jogador == "resetar":
      Voce=1*0
      PC=1*0
      print("A pontuação foi resetada!")
    else:
        print("Essa jogada foi inválida. Selecione uma opção válida!")
```

O código final ficará assim:

```py
from random import randint

t = ["ped", "pap", "tes"]

nome = input("Digite seu nome:")
print("Digite 'resetar' para resetar a pontuação")

Voce = 0
PC = 0

mensagem = ""

def ganhou():
  global Voce
  Voce+=1
  print (mensagem)
  print('Computador =', PC, '\n', nome, '=', Voce)

def perdeu():
  global PC
  PC+=1
  print (mensagem)
  print('Computador =', PC, '\n', nome, '=', Voce)

while True: 
  computador = t[randint (0,2)]
  jogador = input("Pedra, Papel, Tesoura?(ped, pap, tes)")
  if jogador == computador:
        print("Empate!")
        print('Computador =',PC)
        print(nome,'=',Voce)
  elif jogador == "ped":
        if computador == "pap":
            mensagem = "Você perdeu!, Papel cobre Pedra"
            perdeu()
        else:
            mensagem ="Você ganhou! Pedra destrói Tesoura"
            ganhou()
  elif jogador == "pap":
        if computador == "tes":
            mensagem = "Você perdeu!, Tesoura corta Papel"
            perdeu()
        else:
            mensagem ="Você ganhou!, Papel cobre Pedra"
            ganhou()
  elif jogador == "tes":
        if computador == "ped":
            mensagem = "Você perdeu!, Pedra destrói Tesoura"
            perdeu()
        else:
            mensagem ="Você ganhou!, Tesoura corta Papel"
            ganhou()
  elif jogador == "resetar":
      Voce=1*0
      PC=1*0
      print("A pontuação foi resetada!")
  else:
      print("Essa jogada foi inválida. Selecione uma opção válida!")

```

## 5) O Fim
**Eeee, parabéns! Você acabou de fazer seu próprio jogo usando Python!**

![Parabéns](https://cloud-b32jvmwl1.vercel.app/congo.gif)

Se você ainda não criou uma conta no [repl.it](https://repl.it), certifique-se de fazer uma conta para salvar esta criação maravilhosa!

Se você estiver enfrentando dificuldades para se inscrever, assista a [esse vídeo](https://www.youtube.com/watch?v=Mtqp4CUepk0) (em inglês).

Aqui estão algumas ideais do que você pode fazer:

- Considere alterar e adicionar mais recursos!
- Torne-o em um jogo para dois jogadores.
- Acabe o jogo em 3 turnos, ao invés de rodá-lo infinitamente.
- Você pode dar ao texto um efeito de máquina de escrever. (Se você não sabe como fazer, assista [este][máquina de escrever] vídeo)

**Projetos de alguns Hack Clubbers que hackearam esse workshop:**

- [Adicionando novas ações](https://repl.it/@hcbjcentro/pedra-papel-tesoura-lagarto-spock)
- [Mudando a interface](https://repl.it/@hcbjcentro/outra-ui-ppt)
- [Tornando o jogo para 2 jogadores](https://repl.it/@hcbjcentro/ppr-2)

[máquina de escrever]: https://youtu.be/2h8e0tXHfk0

Agora que você terminou de construir este maravilhoso projeto, compartilhe sua bela criação com outras pessoas! Lembre-se, é só mandar a URL do seu projeto!
