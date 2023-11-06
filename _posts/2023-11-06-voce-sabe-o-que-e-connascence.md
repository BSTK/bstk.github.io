---
layout: post
title: Connascence, uma métrica óbvia mas nem tanto Pt2
---

A leitura sempre mostra coisas que nem sabemos que existe, simplesmente pelo fato de nunca ter ouvido falar ou ler sobre, mas que estavam lá só esperando para serem descobertas.
Estou lendo o livro **Fundamentals of Software Architecture: An Engineering Approach** escrito por *Mark Richards, Neal Ford* e no terceiro
capitulo é mostrado o conceito de **Connascence**. Venha ver o que descobri.

Esse é o segundo post abordando a **Connascence**. Então caso não tenha lido a primeira parte, sugiro dar uma passada lá e conferir o conteúdo 
[Connascence, uma métrica óbvia mas nem tanto Pt1](http://brunoluz.com.br/2023/11/02/voce-sabe-o-que-e-connascence).
Na primeira parte, introduzi o conceito de *Conascência* e sobre o primeiro tipo: **Conascência Estática**.
Agora vamos dar uma olhada na **Conascência Dinâmica**

### Conascência Dinâmica
A Conascência Dinâmica refere-se ao acoplamento percebido em **tempo de execução**. Aqui temos 4 subtipos:

**Conascência de execução (CoE)** - A ordem de execução de vários componentes é importante.
- Aqui uso o exemplo que é citado no livro e que já vi acontecer em alguns códigos.

{% highlight java %}
/// Exemplo 1
final Email email = new Email();
email.setRecipient("foo@example.com"); 
email.setSender("me@me.com"); 
email.send(); /// Chamado antes de hora
email.setSubject("whoops");
{% endhighlight %}

O código acima não funcionará corretamente porque certas propriedades devem ser definidas
numa certa ordem para o bom funcionamento. O erro ocorre especificamente na linha ```email.send();``` que deveria ser 
a última linha do bloco de código. Esse tipo de erro não é pego pelo compilador (em linguagens compiladas) e 
só aparece em tempo de execução.
Não gostei muito do exemplo pois o ```design``` do código não ficou legal, talvez isso tenha levado ao erro. No caso o bjeto ```email```
não deveria ter a responsabilidade de ```enviar o email``` é algo que pra mim não faz sentido. Ta certo que é um exemplo, mas
em código real já vi muita coisa parecida e você concerteza também.


Creio que ficaria melhor assim:
{% highlight java %}
final Email email = new Email();
email.setRecipient("foo@example.com");
email.setSender("me@me.com");
email.setSubject("Subject Batata");

/// Outro objeto com a responsabilidade de 'envia o email'
emailService.send(email);
{% endhighlight %}

Creio que dessa forma ajuda a evitar esse tipo problema pois é delegado a outro objeto a a responsabilidade de envio. Claro que não extingue a possibilidade
da *CoE* mas ajuda.


  
**Conascência de tempo (CoT)** - Texto Aqui
- Texto Aqui


**Conascência de valores (CoV)** - Texto Aqui
- Texto Aqui


**Conascência da identidade (CoI)** - Texto Aqui
- Texto Aqui

Links:
- [Fundamentals of Software Architecture](https://fundamentalsofsoftwarearchitecture.com)
- [Connascence](https://connascence.io)
