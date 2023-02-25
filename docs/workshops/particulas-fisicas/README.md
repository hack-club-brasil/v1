---
title: 'Partículas Físicas'
description: 'Crie uma simulação de partículas físicas utilizando a p5.js'  
bg-image: "/workshops/particulas-fisicas/img/fundoparti.gif"
permalink: /workshops/particulas-fisicas/
order: 25
---

<center>Crie uma simulação de partículas físicas utilizando a p5.js</center>  
<center> Feito por <a href="https://github.com/SquarePear" target="_blank">@SquarePear</a></center>
<center>Traduzido por <a href="https://github.com/vitorvavolizza" target="_blank">@vitorvavolizza</a></center>

<br />

Neste workshop, você vai usar a p5.js, juntamente com 🌟 FÍSICA 🌟, para animar as partículas coloridas. Ao longo do caminho, você aprenderá:

- como usar a p5.js
- alguns conceitos básicos de física
- quão _divertida_ a programação criativa é

Todas as diferentes partículas atuarão como planetas em órbita. Todas elas aplicarão forças (gravidade) umas às outras, dependendo de sua massa e de sua distância umas das outras. A principal diferença aqui é que elas estão muito mais próximas umas das outras do que os planetas reais. Isto ocorre para não ser necessário esperar um ano inteiro para que uma partícula orbite a outra.

Quando terminar este workshop, você deve ter algo semelhante a isto!

![Projeto final](https://cloud-oddjiiq5k.vercel.app/0summary.gif)

Esse workshop deve levar cerca de 15-20 minutos para completar.

## Preparando a instalação

Primeiro você precisa montar seu projeto. Criei um [boilerplate](https://pt.wikipedia.org/wiki/Boilerplate_code) básico da p5.js que você pode usar para começar a programar facilmente.

1. Vá para [https://repl.it/@hcbjcentro/boilerplate-p5js](https://repl.it/@hcbjcentro/boilerplate-p5js)
2. E clique em "fork" para fazer uma cópia para que você possa editar

## Fazendo uma partícula

O primeiro passo neste projeto é criar uma classe para gerenciar as partículas. Antes de fazer isso, você deve criar um novo arquivo para armazenar a classe para facilitar a legibilidade.

-> 1. Crie um novo arquivo chamado `particula.js`.

-> 2. Adicione este arquivo entre as tags `<body>` `/body>` do arquivo `index.html`:


```html
...
<body>
  <script src="particula.js"></script>
  <script src="desenho.js"></script>
</body>
...
```

-> 3. Adicione a definição da classe no `particula.js`

```javascript
class Particula {}
```

A [classe](https://pt.wikipedia.org/wiki/Classe_(programa%C3%A7%C3%A3o)) é apenas uma forma de conectar um conjunto de variáveis e funções em um pacote. Agora que você tem uma classe de partícula, você precisa definir o que a partícula pode fazer.

Dentro de sua recém-criada classe `Particula`, adicione:

```javascript
class Particula {
  constructor (x, y, massa) {
    // Configura partícula
  }

  desenha() {
    // Desenha partícula
  }

  aplicaForca(forca) {
    // Aplica força à partícula
  }

  fisica(particula) {
    // Usa partícula
  }

  atualiza() {
    // Atualiza partícula
  }
}
```

Acabamos de acrescentar uma lista de funções que os objetos desta classe podem executar. Com a estrutura básica montada, vamos passar por cada função individualmente e adicionar o código.

```javascript
...
constructor(x, y, massa) {
  this.posicao = createVector(x, y)
  this.aceleracao = createVector(0, 0)
  this.velocidade = createVector(0, 0)
  this.massa = massa

  // raio = (massa / π) ** 0.5
  this.raio = Math.sqrt(this.massa / PI) * ESCALA
  // define uma cor aleatória para a partícula
  this.cor = color(random(0, 255), random(0, 255), random(0, 255))
}
...
```

O [construtor](https://pt.wikipedia.org/wiki/Construtor) é usado para criar uma instância de uma classe. Neste caso, nós o usamos para configurar todas as variáveis quando fazemos uma nova partícula. A função [`createVector()`](https://p5js.org/reference/#/p5/createVector) é fornecida pela p5 para fazer facilmente um [objeto vetorial](https://p5js.org/reference/#/p5.Vector). O que é apenas uma linha 2d ou posição com algumas funções para ajudar a modificar facilmente os valores.

A seguir, vamos adicionar o código para desenhar a partícula.

```javascript
...
desenha() {
  // Retira contorno
  noStroke()
  // Define o preenchimento para a cor da partícula
  fill(this.cor)
  // Desenha a partícula
  ellipse(this.posicao.x, this.posicao.y, this.raio * 2)
}
...
```

Esta função é muito mais simples. Ela define a cor utilizando a função [`fill()`](https://p5js.org/reference/#/p5/fill) e depois desenha a partícula utilizando a função [`ellipse()`](https://p5js.org/reference/#/p5/ellipse) da p5.

A seguir, vamos adicionar o código para aplicar força à partícula.

```javascript
...
aplicaForca(forca) {
  // aceleração = força / massa
  this.aceleracao.add(p5.Vector.div(forca, this.massa))
}
...
```

Aqui vemos a primeira fórmula da física. Ela calcula a aceleração com que o objeto deve ter com base na força aplicada. Utilizamos [a segunda lei do movimento de Newton](https://pt.wikipedia.org/wiki/Leis_de_Newton#Segunda_lei_de_Newton) -> `F=ma` ou `Força = massa * aceleração` para calcular esta aceleração.

A seguir, vamos adicionar o código para aplicar a física do mundo real a uma partícula.

```javascript
...
fisica(particula) {
  // Não aplicar para a própria partícula
  if (this === particula) return

  // massa1 * massa2
  let massa = this.massa * particula.massa
  // raio1 + raio2
  let raio = this.raio + particula.raio
  // Distância entre as partículas
  let distancia = this.posicao.dist(particula.posicao)

  // Não aplicar se as partículas estiverem se tocando
  if (distancia <= raio) return

  // força = G * massa1 * massa2 / distancia ** 2
  let forca = p5.Vector.sub(this.posicao, particula.posicao)
    .setMag(G * massa  / (distancia ** 2))

  // Aplica a força
  particula.aplicaForca(forca)
}
...
```

Esta parece complicada, mas se você pensar nela em termos de física, na verdade é surpreendentemente simples.

- A primeira linha apenas garante que ela não esteja tentando se comparar.
- A seguir, declaramos algumas variáveis.
- A segunda declaração if / else foi usada apenas para parar as forças quando as partículas estão colidindo. Isto não é 100% necessário, mas as coisas provavelmente se quebrarão se elas chegarem muito perto.
- A variável `forca` aponta da partícula afetada para a afetadora.
- Em seguida, a magnitude é definida para a quantidade de força utilizando a [lei da gravitação universal](https://pt.wikipedia.org/wiki/Lei_da_gravita%C3%A7%C3%A3o_universal).
- Finalmente, a força é aplicada à partícula.

Finalmente, vamos escrever um pouco de código para atualizar as propriedades da partícula:

```javascript
...
atualiza() {
  let deltaVelocidade = p5.Vector.mult(this.aceleracao, deltaTime)

  this.velocidade.set(this.velocidade.add(deltaVelocidade))

  this.posicao.set(this.posicao.add(p5.Vector.mult(this.velocidade, deltaTime)))

  this.aceleracao.set(0, 0)
}
...
```

Isto também pode parecer um pouco intimidante no início, mas se você dividir em partes, é muito mais fácil de entender.

- A primeira linha apenas calcula a mudança na aceleração. [`deltaTime`](https://p5js.org/reference/#/p5/deltaTime) é utilizado para manter as forças constantes, não importa quantos quadros por segundo você esteja recebendo.
- A próxima linha adiciona a aceleração à velocidade.
- Então a velocidade é adicionada à posição, novamente utilizando `deltaTime` para manter constantes as mudanças.
- Finalmente, a aceleração é zerada. A velocidade permanece por causa do momento.

Boa! Você configurou a classe `Particula`! Agora você pode voltar para o `desenho.js` e configurar o gerenciamento das suas partículas.

## Gerenciando as Partículas

Abra o arquivo `desenho.js` na barra lateral esquerda. Neste momento, o código deve estar parecido com este:

```javascript
function setup () {
  createCanvas(400, 400)
}

function draw () {
  background(51, 51, 51)
}
```

A função [`setup()`]() é executada uma vez no início do programa para fazer qualquer configuração que você possa precisar. Nós a utilizamos para criar a tela utilizando [`createCanvas()`]() e criar uma tela de 400x400 pixels para que possamos usá-la.

A função [`draw()`]() é executada em cada frame do programa. Ela pode ser utilizada para atualizar a animação a cada frame. Nós a utilizaremos para realizar as atualizações físicas e desenhar as partículas para a tela.

Este arquivo é onde vamos escrever toda a lógica do programa, desde a criação das `Partículas` que acabamos de programar, até a aplicação da física a elas e a animação das mesmas.

Primeiro, vamos criar o conjunto de partículas que estaremos exibindo. Em seguida, precisamos atualizá-las e exibi-las em cada frame dentro da `draw()`.

Primeiro, precisamos definir algumas constantes e certificar um conjunto de partículas que estaremos exibindo. Acima da função `setup()`, no início do arquivo, adicione:

```javascript
// Constantes
const G = 6.67e-11
const ESCALA = 0.001

// Array para guardar partículas
let particulas = []
```

Na física, `G` refere-se à [Constante Gravitacional](https://pt.wikipedia.org/wiki/Constante_gravitacional_universal), que é 6,67 x 10<sup>-11</sup> N⋅m<sup>2</sup>⋅kg<sup>-2</sup> (que vamos escrever como 6,67e-11 no JavaScript). Esta constante é usada como base para todos os tipos de equações divertidas.

Agora, vamos montar nossa tela. Na função `setup()`, acrescente:

```javascript
function setup() {
  createCanvas(400, 400)

  // Itere para criar cada partícula
  for (let i = 0; i < 10; i++) {
    let x = random(0, width)
    let y = random(0, height)
    let massa = random(2e8, 1e9)

    // Adicione a nova partícula para a lista
    particulas.push(new Particula(x, y, massa))
  }
}
```

Tudo o que estamos fazendo na função `setup()` é criar um local aleatório na tela, bem como uma massa aleatória, para a partícula. Então, criamos uma nova `Particula` com esses dados e a adicionamos à matriz de partículas que acabamos de criar.

Todos os blocos de construção estão no lugar. Finalmente, vamos utilizar a função `draw()` para animar as partículas que criamos de acordo com as regras da física. Na função `draw()`, acrescente:

```javascript
function draw() {
  // Define o fundo do canvas para um cinza escuro
  background(51, 51, 51)

  // Itere sobre todas as partículas duas vezes
  for (const particulaA of particulas)
    for (const particulaB of particulas)
      if (particulaA !== particulaB) particulaA.fisica(particulaB)

  // Itera sobre as partículas novamente
  for (const particula of particulas) {
    // Atualiza a partícula com a nova aceleração e velocidade
    particula.atualiza()
    // Desenha a partícula no canvas
    particula.desenha()
  }
}
```

- Primeiro, definimos o fundo de nossa tela para um cinza escuro
- Em seguida, fazemos o loop através do conjunto de partículas duas vezes. Se as duas partículas no loop forem diferentes, utilizamos a função `fisica()` que escrevemos anteriormente para aplicar a física a elas
- Depois disso, fazemos um loop através da matriz de partículas novamente e atualizamos cada objeto `Particula` com os novos dados como resultado da execução da `fisica` sobre as partículas.
- E finalmente, para ver as mudanças, desenhamos a partícula.

Estamos passando por isso devagar e passo a passo, mas não se esqueça que, como todo esse código está na função `desenho`, ele é executado dezenas (às vezes centenas) de vezes por segundo. É assim que é criada a animação suave que você verá em um segundo.

```javascript
const G = 6.67e-11
const ESCALA = 0.001

// Array to store particles
let particulas = []

function setup() {
  createCanvas(400, 400)

  // Itere para criar cada partícula
  for (let i = 0; i < 10; i++) {
    let x = random(0, width)
    let y = random(0, height)
    let massa = random(2e8, 1e9)

    // Adicione a nova partícula para a lista
    particulas.push(new Particula(x, y, massa))
  }
}

function draw() {
  // Define o fundo do canvas para um cinza escuro
  background(51, 51, 51)

  // Itere sobre todas as partículas duas vezes
  for (const particulaA of particulas)
    for (const particulaB of particulas)
      if (particulaA !== particulaB) particulaA.fisica(particulaB)

  // Itera sobre as partículas novamente
  for (const particula of particulas) {
    // Atualiza a partícula com a nova aceleração e velocidade
    particula.atualiza()
    // Desenha a partícula no canvas
    particula.desenha()
  }
}
```

## Você Acabou!

Clique no botão verde "Run" no topo da página. Você deve ver as partículas coloridas se moverem!

E com isso, agora você deve ter uma simulação - embora muito básica - da gravidade entre partículas. Esta simulação é mais ou menos o que aconteceria na vida real com estas massas e distâncias. Os valores em si são um pouco exagerados para serem visualmente mais interessantes.

- [Código Final](https://repl.it/@VitorVavolizza/Particulas-Fisicas)
- [Projeto final](https://particulas-fisicas.vitorvavolizza.repl.co)

## O que fazer a seguir?

Tente mudar alguns valores e veja como eles afetam a simulação. Adicione algumas características extras como criar uma partícula com um clique, antipartículas, ou talvez aplicar este conceito de uma maneira totalmente diferente?

1. [Adicionar no Clique](https://repl.it/@hcbjcentro/Particulas-no-Clique): Adicione uma partícula sempre que você clicar na tela
2. [Anti-Partículas](https://repl.it/@hcbjcentro/Anti-Particulas): Anti-partículas que repelem outras partículas em vez de atrair
3. [Texto Físico](https://repl.it/@hcbjcentro/Texto-de-Particulas): Letras de traços com partículas

Agora que você terminou de construir este maravilhoso projeto, compartilhe sua bela criação com outras pessoas! Lembre-se, é só mandar a URL do seu projeto!
