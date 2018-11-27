---
layout: post
title:  "Carmais - Descobrindo o Omnifaces"
date:   2018-11-28 17:00:00 +0530
---



Nosso ```menu.xhtml``` era simplesmente um monte de elementos ```li``` deixando muito código repetido. E como sabemos, código repetido não é uma boa idéia. Mas uma vez refatoramos nosso menu para deixar muito mais elegante o código e de forma programática. 

Veja como ficou a nova versão


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

[#10 - Carmais - Criando o menu](https://youtu.be/0e1kvxKFLI8)

[#11 - Carmais - Adicionando itens ao menu](https://youtu.be/29ZCVmf93Rs)

Até mais !

[]'s
