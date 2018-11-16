---
layout: post
title:  "Carmais - Continuando a refatoração do template"
date:   2018-11-16 17:00:00 +0530
---

Dando continuidade a nossa ```refatoração```, foi foi criado o arquivo ```menu.xhtml``` onde ficará a parte (x)html referente ao menu.

Criamos também a área de ```conteúdo``` que servirá para renderizar as demais páginas que fazem parte do aplicativo.


## Novas tags : ui:insert e ui:define

No video eu acabei comentendo um erro e trocando as tags ```ui:insert``` e ```ui:define``` de lugar e significado, então vamos a uma explicação e correção :


### ui:insert

Está tag definirá uma área no nosso template que poderá ser substituida pelo conteúdo da página que estiver usando o template. 
Veja um exemplo, onde eu defino duas áreas **titulo** e **conteudo-do-corpo** :

```xml
<!-- meu-template.xhtml -->
<!DOCTYPE html> 
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
	  xmlns:ui="http://java.sun.com/jsf/facelets">
<h:head>
	<ui:insert name="titulo" />
</h:head>
<h:body>
	<ui:insert name="conteudo-do-corpo" />
</h:body>

</ui:fragment>
```


### ui:define

Já esta tag é realmente o conteúdo que irá ser renderizado nas áreas demarcadas com a tag anterior  **ui:insert** :

```xml
<!-- minha-pagina.xhtml -->
<ui:composition xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://java.sun.com/jsf/facelets"
	template="meu-template.xhtml">

	<ui:define name="titulo">
		Um titulo de exemplo
	</ui:define>

	<ui:define name="conteudo-do-corpo">
		Conteudo do corpo aquii !!!!
	</ui:define>
	
</ui:composition>
```

Dessa forma fica fácil criar e utilizar os templates com JSF e Facelets.


## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

### Não deixe de acompanhar o video no canal do YouTube

[#08 - Carmais - Continuando a refatoração do template](https://youtu.be/URL-AQUI)

Até mais !

[]'s
