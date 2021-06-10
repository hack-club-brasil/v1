---

title: 'Modelos Animados 3D'
description: 'Faça modelos 3D utilizando uma biblioteca JavaScript simples'  
bg-image: "/workshops/modelos-3d/img/modelo.png"
permalink: /workshops/modelos-3d/
order: 29

---

<center>Faça modelos 3D utilizando uma biblioteca JavaScript simples</center>  
<center> Feito por <a href="https://github.com/wollygfx" target="_blank">@wollygfx</a> </center>
<center>Traduzido por <a href="https://github.com/vitorvavolizza" target="_blank">@vitorvavolizza</a></center>

<br />

# Modelos animados em 3D

Todos nós já quisemos fazer modelos 3D em algum momento, e embora pareça ser complicado, não é! Neste workshop, usaremos uma biblioteca JavaScript simples chamada [Zdog](https://zzz.dog/getting-started) que nos permitirá fazer tudo o que quisermos em questão de minutos.

Ao final deste workshop, você poderá fazer modelos em 3D como estes:

![exemplos](https://cloud-5a0ya05fk.vercel.app/0large.gif)

Aqui está uma [demonstração ao vivo](https://repl.it/@hcbjcentro/Modelos-3D) do que vamos fazer, você também pode encontrar o código final lá.

![Hack Club 3D logo](https://cloud-590c1rr82.vercel.app/0screen_recording_2020-10-21_at_7.00.53_am.gif)

## Configurando

Este workshop requer um conhecimento básico das seguintes linguagens: HTML & JS. Mas não se preocupe se você ficar preso em algum ponto do workshop, tudo é explicado da melhor maneira para que você possa entender!

Para este workshop usaremos o [Repl.it](https://repl.it), clique [aqui](https://repl.it/languages/html) para criar um ambiente de programação adequado para este workshop.

![Configuração](https://cloud-qbmylslty.vercel.app/0image.png)

## Parte do HTML

Muito bem, vamos começar! Primeiro, queremos criar dentro da tag `<body>` um `<canvas>` no qual, o modelo 3D que vamos criar será renderizado. Depois, coloque uma classe; sinta-se à vontade para nomeá-la como quiser... Eu vou nomeá-la como `modelo`.

```html
<canvas class="modelo"></canvas>
```

Agora temos que colocar o seguinte código dentro da tag `<body>`, este código nos permite usar a biblioteca Zdog sem ter que baixá-la. Saiba mais sobre a CDN [aqui](https://pt.wikipedia.org/wiki/Rede_de_fornecimento_de_conte%C3%BAdo).

```html
<script src="https://unpkg.com/zdog@1/dist/zdog.dist.min.js"></script>
```

Seu código HTML deve estar assim agora:

```html
<body>
  <canvas class="modelo"></canvas>
  <script src="https://unpkg.com/zdog@1/dist/zdog.dist.min.js"></script>
  <script src="script.js"></script>
</body>
```

_Nota: é muito importante manter essa ordem para ter certeza que tudo funciona perfeitamente_

## Parte do JavaScript

Agora que temos nosso arquivo HTML pronto, temos que trabalhar em nosso arquivo JavaScript.

![Cool gif](https://cloud-p49mi1lgl.vercel.app/0tumblr_e49d74c805eec46704d22c1da59ecded_cc93a056_500.gif)

### Configurando o canvas

Vamos começar com a parte divertida. Primeiro, vamos criar a variável principal e vamos dar-lhe um nome, eu vou nomeá-la como "ws".

```javascript
const ws = new Zdog.Illustration({
  element: '.modelo',
  resize: 'fullscreen',
})
```

Vamos explicar isso:

1. **Illustration** -> é a classe de nível superior que lida com o elemento `<canvas>`, armazenando todas as formas na cena, e exibindo essas formas no `element` (elemento).
2. **element** -> usado para indicar o elemento para ser renderizado com a tag `<canvas>`.
3. **resize** -> é usado para modificar o tamanho em que o modelo será renderizado, neste caso, o modelo 3D será renderizado em toda a tela. Se você quiser, você pode remover este elemento.

Neste momento, ainda não aparece nada, então vamos criar nosso primeiro modelo 3D.

### Criando o modelo

Agora, vamos criar uma forma, para este worskop quero fazer um simples cubo, mas você pode criar o que quiser. Aqui está uma lista de [formas](https://zzz.dog/shapes) que você pode criar com a Zdog.

_Note: Cada forma tem suas próprias propriedades ou elementos, você pode verificar [a lista completa aqui](https://zzz.dog/shapes)._

Vamos adicionar o seguinte código ao nosso arquivo JS:

```javascript
new Zdog.Box({
  addTo: ws,
  width: 100,
  height: 100,
  depth: 100,
  stroke: false,
  leftFace: '#da0',
  rightFace: '#e62',
  topFace: '#ed0',
  bottomFace: '#636',
})
```

Explicação:

1. `Box` (Caixa) é uma classe de forma, você pode substituí-la pela forma que você deseja usar...
2. Fizemos o modelo 3D (cubo) uma criança da Ilustração principal da Zdog (ws) usando o elemento addTo. Este elemento deve estar lá, caso contrário o modelo 3D não será renderizado.
3. Os elementos de `width` (largura), `height` (altura) e `depth` (profundidade) podem esticar ou encolher a forma de sua caixa:
    - Width: Define a largura do cubo
    - Height: Define a altura do cubo
    - Depth: Define a profundidade do cubo. Se o valor for 0, o cubo se renderizará como um quadrado 2d; portanto, certifique-se de dar-lhe um valor.

4. O elemento `stroke` (contorno) cria uma borda ao modelo 3D, ele funciona como uma camada externa que você pode usar para dar ao seu modelo 3D um aspecto arredondado. Brinque com ele!
5. Os elementos leftFace, rightFace, topFace e bottomFace dão cor a cada face (esquerda, direita, topo, baixo) do modelo 3D, tente usar cores diferentes para cada face, para que você possa apreciar muito melhor as animações que faz.

### Renderizando

Agora que criamos nosso modelo 3D, vamos renderizá-lo. Use a seguinte linha de código para renderizar o cubo que acabamos de criar.

```javascript
ws.updateRenderGraph()
```

Este código atualiza e renderiza nossa ilustração da Zdog que foi declarada na primeira variável, portanto, certifique-se de escrever o nome corretamente.

Agora vamos clicar no botão **Run** para ver o que acontece...

![Render imagem](img/cubo-cinza.png)

Parabéns, você acabou de fazer seu primeiro modelo 3D... Sim, talvez não seja o que você estava esperando. Vamos consertar isso animando-o.

![woah gif](https://cloud-kr2lyxjbx.vercel.app/0woah.gif)

### Animando o modelo

Adicione o seguinte código ao nosso arquivo JS:

```javascript
function animaModelo() {
  ws.rotate.y += 0.01
  ws.rotate.x += 0.01
  ws.updateRenderGraph()
  requestAnimationFrame(animaModelo)
}

animaModelo()
```

Explicação:

1. Acabamos de criar uma função que fará o modelo 3D girar, você pode nomear esta função como você quiser.
2. O `rotate.x` e o `rotate.y` definem a velocidade de rotação do modelo:
    - O cubo se moverá para cima e para baixo, dependendo do valor dado (- ou +, respectivamente)
    - O cubo se moverá para a direita e para a esquerda, dependendo do valor dado (- ou +, respectivamente)

3. O `ws.updateRenderGraph()` atualiza e renderiza sua ilustração Zdog que foi declarada na primeira variável, certifique-se de escrever o nome corretamente.
4. `requestAnimationFrame(animaModelo)` é como um loop, basicamente ele faz o modelo girar cada vez que frames são criados.
5. `animaModelo()` chama a função.

Agora você pode clicar em run novamente!

![modelo animado](img/cubo.gif)

Incrível, não é?

![Gif Incrível](https://cloud-hrs0t8jeh.vercel.app/0tenor.gif)

### Formas múltiplas

Se você quiser tentar fazer modelos mais complexos, você precisará usar múltiplas formas, [aqui](https://zzz.dog/#made-with-zdog) estão alguns exemplos do que você pode criar:

![exemplos](https://cloud-2jaw6a14x.vercel.app/0image.png)

Fazer múltiplas formas é muito fácil, é tão simples quanto juntar múltiplas formas até conseguir o que você quer. A parte mais difícil disto é colocar tudo onde deve estar, podemos fazer isto usando a propriedade: "translate". Vamos ver como funciona!

Eu quero tentar fazer o logotipo do Hack Club, mas você pode fazer o que quiser! Clique [aqui](https://repl.it/@hcbjcentro/Modelos-3D) para ver o código final.

Primeiro, vamos alterar a classe do nosso canvas no arquivo `index.html` de `modelo` para `hackclub`.

```html
...
<canvas class="hackclub"></canvas>
...
```

E adicionar uma cor de fundo para nosso projeto no arquivo `style.css`.

```css
.hackclub {
  background: #FDB;
}
```

Após isso, vamos voltar para o arquivo `script.js` e alterar o `element` selecionado pelo Zdog de `.modelo` para `.hackclub` e adicionar a propriedade `dragRotate` com o valor definido para `true` para podermos rodar nossa criação.

```js
const ws = new Zdog.Illustration({
  element: '.hackclub',
  resize: 'fullscreen',
  dragRotate: true
})
```

Depois, vamos alterar as características do nosso cubo para ele se transformar em um quadrado vermelho:

```javascript
new Zdog.Box({
  addTo: ws,
  width: 100,
  height: 100,
  color: '#ec3750',
  stroke: 20,
  translate: { z: -18 },
})
```

1. Como visto anteriormente, a propriedade `addTo` coloca a forma feita dentro da Ilustração principal da Zdog.
2. Utilizamos as propriedades de `width` (largura) e `height` (altura) para fazer um quadrado perfeito, não lhe dei `depth` (profundidade) porque não foi necessário.
3. O quadrado inteiro terá a mesma cor (vermelho), então podemos usar a propriedade da cor para dar à forma inteira uma única cor ao invés de atribuir uma cor a cada face.
4. Como mencionado anteriormente, a propriedade `stroke` (contorno) ajuda a forma a parecer um pouco arredondada.
5. A propriedade `translate` move o quadrado -18 dentro do eixo z. Você pode interpretar isto como se estivesse dando a uma forma uma profundidade, então a forma é movida para trás.

Ao executarmos estas poucas linhas, conseguiremos isto:

![quadrado vermelho](img/quadrado-hc.gif)

Ficou perfeito! Agora vamos criar a letra h, para isso vamos precisar de mais 3 caixas:

```javascript
new Zdog.Box({
  addTo: ws,
  depth: 20,
  width: 20,
  height: 80,
  color: '#fff',
  translate: { z: 18, x: -20 },
})
```

- Desta vez, a forma é movida para frente... Assim, criamos um espaço entre o quadrado vermelho e esta nova forma.
- A forma é movida para a esquerda dentro do eixo x

![resultado 1](https://cloud-kg0xtr3hs.vercel.app/0image.png)

```javascript
new Zdog.Box({
  addTo: ws,
  depth: 20,
  width: 20,
  height: 40,
  color: '#fff',
  translate: { z: 18, y: 20, x: 20 },
})
```

- Desta vez, criamos uma nova caixa, mas bem menor.
- Nós a movemos para a direita dentro do eixo x e um pouco para baixo dentro do eixo y.

![resultado 2](https://cloud-1nisp19i8.vercel.app/0image.png)

```javascript
new Zdog.Box({
  addTo: ws,
  depth: 20,
  width: 40,
  height: 20,
  color: '#fff',
  translate: { z: 18, x: 10 },
})
```

Nesta último, tudo o que tivemos que fazer foi mover a caixa para a direita dentro do eixo x, para que ela se misture com a caixa da direita.

![resultado 3](https://cloud-m2gpkvlqa.vercel.app/0image.png)

Agora, vamos atualizar a animação com algumas propriedades simples:

```javascript
function animaModelo() {
  ws.rotate.y += 0.01
  ws.updateRenderGraph()
  requestAnimationFrame(animaModelo)
}

animaModelo()
```

Esse é o resultado final:

![resultado final](https://cloud-d9lxnrldx.vercel.app/0screen_recording_2020-10-09_at_12.05.02_pm.gif)

### Hackeando

Parabéns! Você acabou de aprender o básico da Zdog, sinta-se à vontade para dar uma olhada nos recursos abaixo para melhorar seus conhecimentos...

![gif parabéns](https://cloud-d0aqa4icc.vercel.app/0bec38a05d56ac6ae2d9dec2f482ebff9.gif)

### Outros exemplos

Confira estes modelos 3D legais feitos por outras pessoas:

- [Relógio digital ao vivo feito com Zdog](https://codepen.io/jh3y/pen/vqYLKd)
- [Ametista do Universo Steven feita usando Zdog](https://codepen.io/Metahari/pen/dEQMBy)

### Recursos

- [Zdog website](https://zzz.dog)
- [Made with Zdog](https://codepen.io/collection/DzdGMe/?cursor=ZD0xJm89MSZwPTEmdj0z)
- [Plugin de texto para Zdog](https://jaames.github.io/zfont/)
- [Colocando dados no Zdog](https://observablehq.com/@zechasault/images-drawn-with-zdogs)

Agora que você terminou de construir este maravilhoso projeto, compartilhe sua bela criação com outras pessoas! Lembre-se, é só mandar a URL do seu projeto!

Você provavelmente conhece as melhores maneiras de entrar em contato com seus amigos e familiares, mas se você quiser compartilhar seu projeto com a comunidade brasileira do Hack Club, não há melhor lugar para fazer isso do que no Discord do Hack Club Brasil.✨

1. Clique [aqui][discord]{:target="_blank"} para fazer parte da nossa comunidade!
2. Depois, poste o link do seu projeto no canal `💡┇criações` para compartilhá-lo com todos os Hack Clubbers!

A comunidade te espera!🎉🎉

[discord]: http://bit.ly/discord-hc-brasil