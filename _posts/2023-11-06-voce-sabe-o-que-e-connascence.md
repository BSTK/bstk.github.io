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
não deveria ter a responsabilidade de ```enviar o email```, algo que pra mim não faz sentido. Ta certo que é um exemplo, mas
em código real já vi muita coisa parecida e você com certeza também.


Creio que ficaria assim melhor:
{% highlight java %}
final Email email = new Email();
email.setRecipient("foo@example.com");
email.setSender("me@me.com");
email.setSubject("Subject Batata");

/// Outro objeto com a responsabilidade de 'envia o email'
emailService.send(email);
{% endhighlight %}

Dessa forma ajuda a evitar esse tipo problema pois é delegado a outro objeto a a responsabilidade de envio. Claro que não extingue a possibilidade
da *CoE* existir mas ajuda.

  
**Conascência de tempo (CoT)** - O momento da execução de vários componentes é importante.
- O exemplo citado no livro é o caso comum envolvendo ```race condition``` onde dois threads executados ao mesmo tempo
afeta o resultado da operação em conjunta.


**Conascência de valores (CoV)** - Ocorre quando vários valores se relacionam entre si e precisam ser alterados em conjunto.
- Imagine que ao realizar certa tarefa, digamos alterar um ```status``` de uma entidade, temos que fazer isso em diversos lugares como
diferentes banco de dados. Essa operação só será concluida com sucesso se os diferentes lugares forem alterados em conjunto para manter a
integridade dos dados caso contrário gerando uma incosistência. Exemplos como esse são comuns quando envolve transações envolvendo sistemas 
distribuídos.


**Conascência da identidade (CoI)** - Ocorre quando vários componentes precisam fazer referência a mesma entidade.
- Um exemplo comum da CoI acontece quando dois componentes/serviços independentes precisam compartilhar e atualizar uma estrutura de dados
em comum como uma fila ou um cache distribuído.



Links:
- [Fundamentals of Software Architecture](https://fundamentalsofsoftwarearchitecture.com)
- [Connascence](https://connascence.io)
