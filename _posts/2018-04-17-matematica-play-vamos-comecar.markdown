---
layout: post
title:  "Matemática Play - Vamos começar ?"
date:   2018-04-17 00:00:00 +0530
categories: spring-boot
---

Depois de apresentar todas as ferramentas que iremos utilizar para construir nosso aplicativo **Matemática Play**, é hora de colocarmos a mão na massa.
Vamos seguir uma abordagem **iterativa e incremental**, onde partiremos de uma simples aplicação monolítica até chegarmos ao uso dos micro-serviços.


## Criando o projeto

Como estamos utilizando o **Spring Boot**, vamos usufruir das facilidades que o Spring nos fornece. A IDE que usarei nesta série é o [STS - Spring Tool Suite](https://spring.io/tools/sts/all) mas caso você use o Eclipse ou IntelliJ, pode-se usar um serviço do Spring para gerar a estrutura do nosso projeto, basta acessar  [Spring Initializr](http://start.spring.io/) e preencher os campos conforme imagem abaixo :

![alt text](https://raw.githubusercontent.com/kuiiz/kuiiz.github.io/master/asserts/posts/series/matematica-play/spring-initializer.png "Spring Initializr")

> Nota : Quando acessar o Spring Initializr clique em **Don't know what to look for ? Want more options ? Switch to the full version.** para que seja mostrado todos os campos


Logo em seguida clique no botão **Generate Project** para fazer o download do projeto :

![alt text](https://raw.githubusercontent.com/kuiiz/kuiiz.github.io/master/asserts/posts/series/matematica-play/spring-initializer-gerar-projeto.png "Spring Initializer")

Após o download, descompacte o arquivo zip no seu workspace e importe o projeto em sua IDE. A estrutura do projeto importado deve se parecer como imagem abaixo :

![alt text](https://raw.githubusercontent.com/kuiiz/kuiiz.github.io/master/asserts/posts/series/matematica-play/estrutura-do-projeto.png "Estrutura do projeto")


## Pronto é quando está no ar !

Você acabou de importar o projeto na sua IDE favorita e olha que surpresa, nenhum erro ! Será que o projeto funciona ?
Antes de tudo vamos criar um pacote chamado : 
```java
com.kuiiz.matematicaplay.operacao.web
```
e nele criar a classe : 
```java
HomeController
```

Esta classe será um controlador REST, para isso devemos anotá-la com a annotation : **@RestController**. Feito isso, crie o método **home** e anote-o com  **@GetMapping** definindo o valor **"/"**, assim quando chamarmos o endereço **http://localhost:8080/** nosso método será executado retornando a string com a mensagem: **Matemática Play (-:**

O código completo da classe ficará assim :

```java
package com.kuiiz.matematicaplay.operacao.web;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {
	@GetMapping("/")
	public String home() {
		return "<h1> Matemática Play (-: </h1>";
	}
}

```

Agora que temos um endpoint válido para receber requisições, vamos iniciar nosso aplicativo e vê-lo funcionar. 
Na pasta raiz do projeto, execute o seguinte comando : 
```java
./mvnw spring-boot:run
```
> Nota 1: Eu estou utilizando o Linux, caso você esteja com Windows, digite apenas :  **mvnw spring-boot:run** sem o **./**
> 
> Nota 2: O arquivo mvnw que veio junto com o projeto é um **wrapper do maven**, assim não sendo necessário que você tenha o maven instalado em sua máquina.




Agora acesse **http://localhost:8080/**  e você deverá ver a seguinte tela :

![alt text](https://raw.githubusercontent.com/kuiiz/kuiiz.github.io/master/asserts/posts/series/matematica-play/tela-inicial-configuraaoo-pronta.png "Configuração pronta !")



Agora sim, temos um aplicativo com **Spring Boot** devidamente configurado e funcionando. Não paramos por aqui, continue seguindo a série pois temos um longo caminho pela frente.


Se você chegou até aqui, obrigado.
Continuamos no próximo post.

Até {}!
