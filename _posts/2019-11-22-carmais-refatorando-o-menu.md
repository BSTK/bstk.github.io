---
layout: post
title:  "Carmais - Continuando a refatoração do template"
date:   2018-11-22 17:00:00 +0530
---

**Refatoração**, não sei se perceberam mas gosto desta palavra quando estou desenvolvendo. Refatorar, refatorar .. refatorar ! Nada melhor do que pegar um código com uma primeira versão e ir refatorando até chegar em algo que seja aceitável e bem escrito.

Hoje temos nosso arquivo ```menu.xhtml``` que nada mais é que o html padrão do template **AdminLTE**, nossa missão é deixá-lo enxuto e apenas com o conteúdo necessário do menu.

Abaixo o xhtml que fizemos nos videoas #10 e #11.


```xml
<!-- menu.xhtml -->
 ...

<h:form prependId="false">  
	<ul class="sidebar-menu" data-widget="tree">
	    <li>
	      <h:commandLink action="/app/vender-meu-veiculo/vender-meu-veiculo.xhtml">
	      	<i class="fa fa-dashboard"></i>
	        <span>Vender meu veículo</span>
	      </h:commandLink>
	    </li>
	    <li>
	      <h:commandLink action="/app/meus-anuncios/meus-anuncios.xhtml">
	        <i class="fa fa-dashboard"></i>
	        <span>Meus anúncios</span>
	      </h:commandLink>
	    </li>
	    <li>
	      <h:commandLink action="/app/minhas-negociacoes/minhas-negociacoes.xhtml">
	        <i class="fa fa-dashboard"></i>
	        <span>Minhas negociações</span>
	      </h:commandLink>
	    </li>
	    <li>
	      <h:commandLink action="/app/meu-perfil/meu-perfil.xhtml">
	        <i class="fa fa-dashboard"></i>
	        <span>Meu perfil</span>
	      </h:commandLink>
	    </li>
	</ul>   
</h:form>	
 ...

```

Não podemos nos esquecer que deixamos um **bug** neste menu, porém não se preocupe que iremos corrigi-lo em breve.


## Links úties

[Repositório no Github](https://github.com/BSTK/carmais)

### Não deixe de acompanhar o video no canal do YouTube

[#10 - Carmais - Criando o menu](https://youtu.be/URL-AQUI)

[#11 - Carmais - Continuando a refatoração do template](https://youtu.be/URL-AQUI)

Até mais !

[]'s
