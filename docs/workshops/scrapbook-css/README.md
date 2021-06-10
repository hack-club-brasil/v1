---
title: 'Scrapbook Incrível com CSS'
description: 'Customize seu scrapbook com CSS'
bg-image: '/workshops/scrapbook-css/img/scrapbook.png'
permalink: /workshops/scrapbook-css/
order: 11
---

<center>Customize seu scrapbook com CSS</center>  
<center>Feito por <a href="https://github.com/sampoder" target="_blank">@sampoder</a></center>
<center>Traduzido por <a href="https://github.com/vitorvavolizza" target="_blank">@vitorvavolizza</a></center>

<br />

![GIF de cair o queixo](https://media2.giphy.com/media/MuTenSRsJ7TQQ/200.gif)

_^ o que vai acontecer quando as pessoas virem o seu scrapbook_

Uma das coisas incríveis de construir projetos para a web é como é fácil torná-los bonitos! Para estilizar projetos da web, usamos uma linguagem chamada CSS. Ela nos permite fazer tudo, desde mudar a cor de fundo até animar o texto!

<a href="https://scrapbook.hackclub.com/sampoder"><img src="https://cloud-5j06exp7f.vercel.app/screenshot_2020-09-12_at_7.29.11_pm.png" width="380" alt="Sam Poder's Scrapbook Profile"></a>

Hoje, vamos aprender CSS personalizando um perfil do [Scrapbook](https://scrapbook.hackclub.com/)(^ como o meu acima). O scrapbook é uma plataforma onde Hack Clubbers podem compartilhar fotos do que estão fazendo ou do dia a dia. Se você não tem um perfil ainda, faça login no Slack do Hack Club, visite o canal [#scrapbook](https://hackclub.slack.com/archives/C01504DCLVD/), tire uma foto (ou use uma antiga) e poste!

Em seguida, você vai abrir o [Customizador de Scrapbook](https://scrapbook.hackclub.com/customizer/). Digite seu nome de usuário na parte superior e exclua qualquer código na coluna direita. Clique em **‘Go’**.

Agora estamos prontos para começar!

## Cores mágicas!

Para tornar a configuração de cores mais fácil, o Scrapbook usa variáveis ​​CSS. As variáveis ​​CSS funcionam como as variáveis ​​em qualquer outra linguagem de programação. Você define a variável e então ao invés de colar o valor em todos os lugares você adiciona `var(--nome-da-variável)`. Isso faz com que, se você quiser alterar a cor de um tema (como estamos fazendo hoje!), Você possa fazer isso com uma linha.

Para o scrapbook, existem 3 variáveis ​​de cores principais com as quais vamos querer brincar hoje. Estas são a cor de fundo, a cor do texto e a cor das mensagens (que são as colors-elevated abaixo).

Para informar ao navegador a cor exata que desejamos, podemos usar códigos hexadecimais ou valores RGB. Para este workshop, vamos usar códigos HEX. Para nos ajudar a conseguir isso, podemos [dar um Google por “seletor de cores”](https://www.google.com/search?q=seletor+de+cores). A partir daqui, você pode brincar com os sliders e escolher sua cor preferida. Escolha 3 cores a serem usadas para o perfil.

Em seguida, na coluna direita, digite:

```css
:root {
  --colors-background: SUACOR;

  --colors-text: SUACOR;

  --colors-elevated: SUACOR;
}
```

Substitua `SUACOR` pelos códigos hexadecimais que você encontrou, certifique-se de incluir o símbolo hash (hashtag) no início.

Você ainda pode ver um pouco de cinza, esses são elementos que usam a variável `--colors-muted`, você consegue descobrir como alterá-la com base no que aprendemos acima?

Usei estas cores:

```css
:root {
  --colors-background: #a633d6;

  --colors-text: #fff;

  --colors-elevated: #228b23;
}
```

Para criar isso:

<img src="https://cloud-loib1whnd.vercel.app/screenshot_2020-09-12_at_8.01.02_pm.png" width="380" alt="Green and purple themed Scrapbook">

## Fontes diferenciadas

E se o seu scrapbook usasse uma fonte cursiva? Ou uma fonte punk maluca? As fontes podem dar à sua página muito mais personalidade, então vamos trocá-las.

O [Google Fonts](https://fonts.google.com/) é um ótimo lugar para começar, role por todas as fontes e encontre uma de sua preferência! Clique nela e, para cada um de seus estilos, clique em “Select this style”. Então, na barra que apareceu à direita, clique em `Embed` e escolha `@import`. Copie a primeira caixa de texto sem as tags `<style>`. Você deve ter algo como:

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;1,100;1,300&display=swap');
```

Como provavelmente escolhemos fontes diferentes, não se preocupe se nossos links forem diferentes. Agora cole este trecho de texto no topo do seu arquivo de código (o que você estava escrevendo no lado direito).

Semelhante às cores, o Scrapbook usa variáveis ​​CSS para tornar super simples para nós alterarmos as fontes.

Adicione o seguinte trecho de código logo abaixo da linha `--colors-elevated`.

```css
:root {
  --colors-background: YOURCOLOUR;

  --colors-text: YOURCOLOUR;

  --colors-elevated: YOURCOLOUR;

  /* Adicione o código abaixo */

  --fonts-body: 'Nome da fonte', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI';
}
```

Substitua `‘Nome da fonte’` pelo nome de sua fonte, apenas mantenha as aspas! Deixamos as outras fontes lá como backup, caso a fonte que escolhemos não carregue. Eles são chamados de [Fallback Fonts](https://css-tricks.com/css-basics-fallback-font-stacks-robust-web-typography/)

Eu usei um estilo techno, com a fonte [Audiowide](https://fonts.google.com/specimen/Audiowide?category=Display&sidebar.open=true&selection.family=Audiowide).

<img src = "https://cloud-loib1whnd.vercel.app/screenshot_2020-09-12_at_8.01.02_pm.png" width="380" alt ="Scrapbook com tema verde e roxo">

## **Animações CSS loucas - Parte 1 ✌︎**

Temos uma fonte extravagante e algumas cores mágicas, mas este site ainda precisa de movimento! Vamos animá-lo!
Primeiro, vamos fazer nosso texto com uma animação de arco-íris.

As animações são feitas com keyframes, onde você define o estado do objeto (nas propriedades CSS) em diferentes estágios da animação. Se você tem alguma experiência com animação de vídeo, o keyframe funciona de forma muito semelhante aqui!

Vamos definir essa animação:

```css
@keyframes arco-iris {
  14% {
    color: red;
  }
  28% {
    color: orange;
  }
  42% {
    color: yellow;
  }
  56% {
    color: green;
  }
  70% {
    color: cyan;
  }
  84% {
    color: blue;
  }
  100% {
    color: purple;
  }
}
```

O que isto faz é ao chegar aos 14% da animação, a cor será #ff0000 (vermelho), ao chegar aos 28% será #ffa500 (laranja) e assim por diante...

Agora, precisamos informar ao navegador qual elemento deve ter a animação aplicada a ele. Identificamos um elemento com um nome de classe e um `.` antes dele. Para começar, digitaremos isto:

```css
.header-title-name {
}
```

Entre as duas chaves, podemos digitar coisas que mudam o que todos os elementos com a classe: `header-title-name` têm/farão. Dentro delas, vamos pedir ao navegador para adicionar nossa animação ao elemento. Podemos fazer isso adicionando:


```css
animation: arco-iris 5s infinite;
```

Adicionaremos isso dentro das chaves. O que isso fará é aplicar a animação do arco-íris e executá-la por uma quantidade infinita de vezes por 5 segundos cada.

Por enquanto está assim:

<img src="https://cloud-1fmtzoja5.vercel.app/ezgif-5-db525cfe2a47.gif" width="380" alt="Texto animado do arco-íris" />

Eu tenho um desafio para você. Vamos fazer a imagem de perfil girar!

Aqui estão algumas coisas que você precisa saber:

- A classe para a foto do perfil é `header-title-avatar`
- Para girar uma imagem, use: `transform: rotate(20deg)`

Acha que consegue fazer isso? A solução está [aqui](#solução-girar).

<img src="https://cloud-ojh0xf17r.vercel.app/ezgif-5-5540a1713ebc.gif" widt="380" alt="Spinning profile picture">

## **Animações CSS loucas - Parte 2 ✌︎**

As animações também podem acontecer quando você passa o mouse sobre um elemento. Isso ocorre por meio de pseudo classes, uma palavra que você pode adicionar ao seu seletor para indicar um estado específico. Por exemplo, `.header-title-avatar: hover` se aplica quando estamos passando com o mouse pelo avatar.

O que vamos fazer nesta seção, é bem simples. Quando passarmos o mouse sobre um post, ele ficará maior. Como cada post tem uma classe de `post`, podemos fazer isso com o seguinte:

```css
.post :hover {
  transform: scale(1.02);
}
```

No entanto, quando fazemos isso, é muito direto e nada suave.

Para corrigir isso, adicionaremos isso ao nosso CSS:

```css
.post {
  transition: all 0.2s ease-in-out;
}
```

O que isso significa é que a transição é tranquila no começo e no final, tornando-a suave e lisinha!

<img src="https://cloud-af3nqoqwq.vercel.app/ezgif-4-effdbb2d1794.gif" width="380" alt="Aumentar a animação flutuante para cada postagem">

## Hackeando

É hora de você assumir controle, há tantas coisas épicas que você pode fazer com CSS! Então vá em frente e faça seu scrapbook exclusivo o para você!! Aqui estão alguns guias:

- Faça um fundo gradiente. Você não poderá fazer isso com as variáveis ​​CSS, mas poderá fazer isso aplicando o fundo à tag body. Estou falando demais, [este artigo explica os gradientes CSS](https://css-tricks.com/css3-gradients/)!
- Troque o texto no site com CSS. Confira este [artigo sobre o assunto](https://css-tricks.com/swapping-out-text-five-different-ways/#css-only-way) para ter uma ideia melhor do que estou falando! Isso é o que eu chamo de hack ;)
- Anime tudo! Você viu como podemos animar elementos com CSS, mas na verdade apenas mudamos a cor e tornamos algo maior. Existem muitas outras propriedades animáveis, verifique esta [lista extensa](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties). Além disso, verifique as referências de classes logo abaixo para saber como alterar cada elemento na página.

## Publicando

Para publicar, clique em `Add to Gist`. Copie o link. Agora vá para o canal [#scrapbook-css](https://hackclub.slack.com/archives/C015M6U6JKU/) no Slack e cole o link em uma nova mensagem para aplicar à seu scrapbook!

Depois de deixar seu perfil bonito, que tal compartilhá-lo no [#scrapbook](https://hackclub.slack.com/archives/C01504DCLVD/)? Marque-me com ([@sampoder](https://hackclub.slack.com/archives/DT08DHJKF/)), adoraria ver seu projeto!

## Guia de referências

Aqui estão todas as classes que você precisa saber para fazer um scrapbook esplêndido:

- `header` é toda a seção acima de seus posts.
- `header-col-1` é a seção com seu avatar, nome, streak (se configurado para exibição), links de redes sociais e perfil de áudio (se configurado).
- `header-title-avatar` é o seu avatar, é um `<img>`.
- `header-title-name` é o cabeçalho com seu nome.
- `header-content` contém todos os seus links de redes sociais e seu streak (se definido para exibição).
- `header-streak` é o indicador de seu streak.
- `header-links` são seus links de redes sociais.
- `header-link` é um link de rede social, provavelmente existem vários na seção de links.
- `header-webring` é uma seção que contém cada pessoa em seu webring, caso você tenha criado um.
- `header-webring-mention` é uma menção a uma pessoa em seu webring, provavelmente há várias na seção webring.
- `header-chart` é o gráfico que mostra com que frequência você publica.
- `posts` é a seção com todos os seus posts.
- `post` é um post.
- `post-header` é a parte do post com a data/hora.
- `post-attachments` é a seção com cada imagem, vídeo ou arquivo de áudio que você anexou à sua postagem do Scrapbook.

## Inspiração

Para terminar, gostaria de deixar alguns perfis de Scrapbook personalizados incríveis.

- O deus dos perfis de scrapbook: o scrapbook do Caleb é incrível demais para eu descrever. Honestamente, apenas [dê uma olhada](https://scrapbook.hackclub.com/caleb/). - [Código](https://gist.github.com/cjdenio/05d70b36875472a87d665ddb6c25aa1b)
- Ghoshwolf fez um [scrapbook com tema retrô](https://scrapbook.hackclub.com/ghostwolf). - [Código](https://gist.github.com/TheOnlyGhostwolf/56614e8d810a9599e87551bc327f410e)
- Yash Karthik fez um [scrapbook roxo, com algumas joias escondidas](https://scrapbook.hackclub.com/yashkarthik95/). - [Código](https://gist.github.com/YashKarthik/7ed6509dfd736fabb1a2b2503e2ee5ab)
- Eu também tenho [meu próprio](https://scrapbook.hackclub.com/sampoder) perfil personalizado do Scrapbook! O tema está sempre mudando, no momento da escrita é de temática soviética. - [Código](https://scrapbook.hackclub.com/api/css/?url=https%3A%2F%2Fdeadspryintelligence.sampoder.repl.co%2Fstyle.css)

Eu realmente espero que você tenha gostado deste workshop, happy hacking!

## Solução Girar

```css
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.header-title-avatar {
  animation: rotate 5s infinite;
}
```

Agora que você terminou de construir este maravilhoso projeto, compartilhe sua bela criação com outras pessoas! Lembre-se, é só mandar a URL do seu projeto!

Você provavelmente conhece as melhores maneiras de entrar em contato com seus amigos e familiares, mas se você quiser compartilhar seu projeto com a comunidade brasileira do Hack Club, não há melhor lugar para fazer isso do que no Discord do Hack Club Brasil.✨

1. Clique [aqui][discord]{:target="_blank"} para fazer parte da nossa comunidade!
2. Depois, poste o link do seu projeto no canal `💡┇criações` para compartilhá-lo com todos os Hack Clubbers!

A comunidade te espera!🎉🎉

[discord]: http://bit.ly/discord-hc-brasil