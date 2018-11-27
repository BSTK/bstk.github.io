---
layout: post
title:  "Carmais - Menu dinâmico"
date:   2018-11-27 17:00:00 +0530
---

Nosso ```menu.xhtml``` era simplesmente um monte de elementos ```li``` deixando muito código repetido. E como sabemos, código repetido não é uma boa idéia. Mas uma vez refatoramos nosso menu para deixar muito mais elegante o código e de forma programática. 


## MenuController ... Menu ... MenuItem

Essas três classes : ```MenuController ... Menu ... MenuItem``` é todo código que precisaremos pra deixar nosso menu dinâmico, totalmente controlado por código Java.

Veja como ficou cada uma delas :

[MenuController](https://github.com/BSTK/carmais/blob/master/src/main/java/br/com/carmais/core/menu/MenuController.java)

[Menu](https://github.com/BSTK/carmais/blob/master/src/main/java/br/com/carmais/core/menu/Menu.java)

[MenuItem](https://github.com/BSTK/carmais/blob/master/src/main/java/br/com/carmais/core/menu/MenuItem.java)


Repare como ficou muito mais legal nosso menu e com total controle :

```java

@PostConstruct
public void inicializar() {
  List<MenuItem> itens = Arrays.asList(
	new MenuItem("/app/meu-perfil/meu-perfil.xhtml", "fa fa-dashboard", "Meu perfil", true),
	new MenuItem("/app/meus-anuncios/meus-anuncios.xhtml", "fa fa-dashboard", "Meus anúncios",false),
	new MenuItem("/app/minhas-negociacoes/minhas-negociacoes.xhtml", "fa fa-dashboard", "Minhas negociações", false),
	new MenuItem("/app/vender-meu-veiculo/vender-meu-veiculo-planos.xhtml", "fa fa-dashboard", "Vender meu veículo", false)
  );

  setMenu(new Menu(itens));
}

```

Agora veja como ficou o novo xhtml

```xml
<!-- menu.xhtml -->
 ...

<h:form prependId="false">  
  <ul class="sidebar-menu" data-widget="tree">
    <ui:repeat var="item" value="#{ menuController.menu.itens }">
  	  <li class="treeview">
  	  	<h:commandLink action="#{ menuController.navegarPara(item.url) }">
  	  		<i class="#{ item.cssIcone }"></i>
  	  		<span>#{ item.label }</span>
  	  	</h:commandLink>
  	  </li>
  	</ui:repeat>
  </ul>   
</h:form>	
 ...

```

Agora não temos mais o html chumbado e sim os dados vindo do nosso código Java. Podemos carregar esse menu de um banco de dados NoSQL, de um arquivo JSON, de um serviço e etc. No nosso caso ele está ```hardcode``` no Java mas iremos remove-lo mais pra frente.


## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

### Não deixe de acompanhar o video no canal do YouTube

[#12 - Carmais - Menu dinâmico](https://youtu.be/qYVsUJDGx-c)

Até mais !

[]'s
