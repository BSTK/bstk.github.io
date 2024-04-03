---
layout: post
title: Testes de integração com Testcontainers - Criando uma "Extension Customizada"
---

[![Capa post](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/capa-post-test-containers-02.png?raw=true)](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/capa-post-test-containers-02.png?raw=true)

Continuando a explorar os *Testes de integração com TestContainer e Spring Boot*, cheguei num ponto onde foi preciso criar uma *Extension*
do JUnit personalizada para resolver um problema que estava acontecendo ao executar mais de uma classe de teste.

Bom, esse é o segundo post sobre testes de integração e caso não tenha lido, de uma olhada no primeiro post onde explico como iniciei e como cheguei até aqui.
Estou usando o mesmo projeto do primeiro post, então caso queira acompanhar e seguir a linha de raciocio é interessante dar uma lida no 
primeiro post antes de iniciar esse aqui.

> - [ Projeto TestContainers com Spring Boot ](https://github.com/BSTK/okk-blog-posts/tree/main/okk-testcontainers-com-springboot)

> - [ Testes de integração com Testcontainers e Spring Boot ](http://brunoluz.com.br/2024/03/28/testes-de-integracao-com-testcontainres-e-spring-boot)
