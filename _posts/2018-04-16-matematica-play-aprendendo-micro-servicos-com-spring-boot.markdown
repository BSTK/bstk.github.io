---
layout: post
title:  "Matemática Play - Aprendendo micro-serviços com Spring Boot"
date:   2018-04-16 00:00:00 +0530
categories: spring-boot
---


Se você é um desenvolvedor Java assim como eu já deve ter ouvido falar em **Spring Boot** e também **micro-serviços** e com certeza já deve ter mexido muito com o ecossistema **Spring**.

## O que é o Matemática Play

**Matemática Play** é um aplicativo que estou desenvolvendo afim de aplicar os conhecimentos adquiridos no estudo sobre : *Construindo micro-serviços com Spring Boot* e assim quero aqui compartilhar com vocês tudo que aprendi.
O funcionamento do aplicativo é bem simples : Uma plataforma gamificada onde tentaremos acertar o máximo de operações matemáticas que pudermos. Você terá cerca de 5 segundos para resolver algo do tipo **18 x 92 = ?** ou **73 - 18 = ?** (Olha ! Não vale usar a calculadora. É trapaça !)
No decorrer do jogo as coisas vão se complicar um pouquinho e você será tentado a se tornar o melhor em resolver estes tipos de operações, ficando assim o máximo de tempo na lista dos 10 melhores jogadores. Teremos pontuação, badges/insígnias, rankings e muito mais.

Gostou da ideia ? Que tal dar uma passadinha rápida e já ir jogando. Acesse o link : [AQUI VEM UM LINK DO MATEMÁTICA PLAY HOSPEDADO NO HEROKU].
> Nota : Só lembrando que este aplicativo ainda está em desenvolvimento, então coisas inconvenientes e erros grosseiros podem acontecer


## Tecnologias utilizadas

Farei todo o desenvolvimento do app utilizando uma abordagem RESTFul e todo ferramental que o **Spring** nos oferece. Segue abaixo uma lista de ferramentas que iremos usar :

 - **Java (8)** : A linguagem principal no qual será escrito nosso aplicativo. Iremos usar Java na sua versão 8
 
 - **Spring Boot** : Facilita a criação de aplicativos independentes baseados no framework Spring, que você pode executar de maneira rápida e objetiva.
 
 - **RabbitMQ** : Serviço de fila (message-broker) open source escrito em Erlang que implementa o protocolo AMQP (Advanced Message Queuing Protocol).

 - **Eureka** : Serviço baseado em REST que é usado principalmente na nuvem da AWS para localizar serviços com o objetivo de balanceamento de carga e de servidores de camada intermediária.
 
 - **Ribbon** :  Uma biblioteca Inter Process Communication (chamadas de procedimento remoto) com balanceadores de carga de software incorporados. O modelo de uso principal envolve chamadas REST com suporte a vários esquemas de serialização.
 
 - **Zuul** : Serviço que fornece roteamento dinâmico, monitoramento, resiliência e segurança.

 - **JUnit** : Framework open-source criado por Erich Gamma e Kent Beck, com suporte à criação de testes automatizados na linguagem de programação Java.

 - **Mockito** : Framework que irá auxilizar nos testes unitários para construção de mocks (objetos simulados).
 
 - **Cucumber** : Auxilia no desenvolvimento de testes de aceitação automatizados escritos em um estilo de desenvolvimento orientado a comportamento (BDD)
 
 - **Bootstrap** :  Framework web open source para desenvolvimento de componentes visuais  para sites e aplicações web usando HTML, CSS e JavaScript
 
 - **Heroku**: Plataforma como serviço (PaaS) que permite aos desenvolvedores criar, executar e operar aplicativos inteiramente na nuvem.
 
 E muitos outros que não estão listados aqui mas que com certeza aparecerá em algum momento.

Se você chegou até aqui, obrigado. Continuamos no próximo post.

Até {}!
 