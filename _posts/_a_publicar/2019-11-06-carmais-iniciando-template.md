---
layout: post
title:  "Carmais - Iniciando o template"
date:   2018-11-06 18:00:00 +0530
---

É hora de dar vida ao projeto. Vamos iniciar a criação do template que será utilizado pela área administrativa de nosso app. Eu decidi utilizar o ```adminLTE```, bem conhecido na web e bastante utilizado.


## AdminLTE

Acesse o site [https://adminlte.io](https://adminlte.io/) e faça o download da última versão. também é possivél ver um [Live Preview](https://adminlte.io/themes/AdminLTE/index2.html) de como é o template e sua infinidade de componentes.


## Configurando

Eu utilizei o arquivo ```AdminLTE-2.4.5/pages/layout/fixed.html``` que trará uma barra fixa no top

![admin-lte-html-fixed.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/admin-lte-html-fixed.png)


Como nossas páginas são ```.xhtml``` devemos fechar todas as tag's que foram abertas, algo como : 
```<meta />``` ou ```<meta></meta>``` e este foi o maior esfoço que fizemos.

O resultado ficará igual na imagem acima e claro, estamos fazendo o template então, algumas refatorações serão bem vindas para que possamos deixar o código html mais organizado e devidamente separado.


## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

[https://adminlte.io](https://adminlte.io/)

[Live Preview](https://adminlte.io/themes/AdminLTE/index2.html)

### Não deixe de acompanhar o video no canal do YouTube

[#06 - Carmais - Iniciando o template](https://youtu.be/GxatXImKJy8)

Até mais !

[]'s


















Antes de começar, verifique se a opção está setada como na imagem abaixo :
Caminho : ```carmais > Properties > Prject Facets```

![project-faces.png](https://raw.githubusercontent.com/BSTK/bstk.github.io/master/asserts/img/project-faces.png)

## Código do home.xhtml

Veja como ficou o código do nosso ```home.xhtml```

``` xhtml
<!DOCTYPE html> 
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
<h:head>
    <title>
      #{ homeController.mensagem }
    </title>
</h:head>
<h:body> 
	<h1>
	  #{ homeController.mensagem }	
    </h1>
</h:body> 
</html>
```

## Código do HomeController

Veja como ficou o código do nosso ```HomeController```

``` java
@Named
@ViewScoped
public class HomeController implements Serializable {

  /**
   * serialVersionUID
   */
   private static final long serialVersionUID = 1L;
   
   private String mensagem;
	
   @PostConstruct
   public void inicializar() {
   	 setMensagem("Projeto Configurado !!");
   	 System.out.println("Projeto Configurado !!");
   }

   public String getMensagem() {
   	 return mensagem;
   }

   public void setMensagem(String mensagem) {
	 this.mensagem = mensagem;
   }
}
```

## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

### Não deixe de acompanhar o video no canal do YouTube

[#05 - Carmais - Testando a configuração](https://youtu.be/GxatXImKJy8)

Até mais !

[]'s
