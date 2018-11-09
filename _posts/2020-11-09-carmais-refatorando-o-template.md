---
layout: post
title:  "Carmais - Refatorando o template"
date:   2018-11-09 18:00:00 +0530
---

Depois de iniciado nosso template é hora da nossa primeira ```refatoração```. Refatorar para melhorar o que já está bom.

Nesse tarefa de refatorar vamos organizar nossa estrutura (x)html para dar um ar mais profissional e deixar cada coisa no seu lugar.


## Menos é mais

```Menos``` código é ```mais``` fácil de manter, é mais fácil de se achar e muito mais organizado. Foi o que fizemos, separamos nosso html em novos arquivos para que cada um fizesse aquile que fosse de sua responsabilidade.

> **header.xhtml** : Aqui ficará todo html que renderiza o header de nossa aplicação. Basicamente uma navbar

![header.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/header.png)

> **menu.xhtml**  : Deixamos aqui o html responsavél pela renderização do menu, neste caso uma ```aside```

![menu.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/menu.png)

> **footer.xhtml** : E aqui todo html referente ao footer

![footer.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/footer.png)

> **principal.xhtml** : Este é o responsavél por ser nosso ```template``` de fato. Foi nele onde juntamos todos os demais arquivos num só. 

![principal.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/principal.png)


## Rascunhos

Eu criei um arquivo chamado ```rascunho.txt``` que contém ```xhtml``` básico que usaremos em todas as páginas.

```xml
<!-- XHTML BASICO -->
<!DOCTYPE html> 
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
<h:head>
</h:head>
<h:body>
</h:body>

<!-- XHTML FRAGMENT -->
<ui:fragment xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://java.sun.com/jsf/facelets">
	
</ui:fragment>

<!-- XHTML PAGINAS CONTEUDO -->
<ui:composition xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://java.sun.com/jsf/facelets">
	
</ui:composition>

<!-- PAGINAS CONTEUDO -->
<div class="box-body">
          
</div>

```

## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

### Não deixe de acompanhar o video no canal do YouTube

[#07 - Carmais - Refatorando template](https://youtu.be/--75m24rK6w)

Até mais !

[]'s