---
layout: post
title:  "Carmais - Testando a configuração"
date:   2018-10-31 18:00:00 +0530
---

Depois de configurar vários arquivos ```xml```, vamos testar nosso web app para ver se tudo que fizemos saiu de forma correta para que possa ser executado Tomcat.

Nada de muito complicado será feito nesta aula, apenas o teste básico da configuração.


## Project faces

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
