---
layout: post
title:  "Matemática Play - User Story Requisitos do usuário"
date:   2018-04-17 00:00:00 +0530
categories: spring-boot
---

## User Story - Requisitos do usuário

Vamos usar o conceito de **user story (história de usuário)** para guiar o desenvolvimento do app. Se você nunca ouviu falar no termo **user story**, deixo aqui dois artigos ([O que é a User Story?](http://www.knowledge21.com.br/sobreagilidade/user-stories/o-que-e-user-story/) e [Como é a User Story?](http://www.knowledge21.com.br/sobreagilidade/user-stories/como-e-user-story/)) explicando o conceito de forma bem detalhada. Uma explicação rápida seria : **user story** é uma forma simples que busca descrever a necessidade do usuário. 

## User Story - Uma Operação Simples
> Como usuário do aplicativo, quero apresentar uma operação matemática (soma, subtração, multiplicação, divisão) aleatória para que eu possa resolve-lá e que não seja muito fácil.

Então precisamos implementar o requisito do usuário, vamos dividir em pequenas tarefas :
 - Criar um serviço básico com a lógica de negócios. 
 - Criar um endpoint básico da API para acessar esse serviço (API REST). 
 - Criar uma página da Web básica para solicitar aos usuários que resolvam a operação.


## Criando o domínio - Classe de Operação

Para que possamos concluir  todas as tarefas, vamos começar criando uma classe de domínio que representará uma operação (10 + 5 = 15, 20 * 2 = 40) e também um enum para representar os operadores (soma(+), subtração(-), multiplicação(x) e divisão(/)).

Crie o pacote :
> com.kuiiz.matematicaplay.operacao.domain

No pacote acima, cria a classe ```Operacao``` e o enum ```Operador```

Agora vamos colocar alguns atributos em nossa classe :

```java
public class Operacao {

	private int fatorA;
	private int fatorB;
	private int resultado;
	private Operador operador;
	
	/**
	 * Operacao
	 */
	public Operacao() {}

	/**
	 * Operacao
	 * @param fatorA
	 * @param fatorB
	 * @param operador
	 */
	public Operacao(int fatorA, int fatorB, Operador operador) {
		this.fatorA = fatorA;
		this.fatorB = fatorB;
		this.operador = operador;
	}
}
```
E também adicionar código ao nosso enum :

```java
public enum Operador {

	SOMA("soma", "+"),
	SUBTRACAO("subtração", "-"),
	MULTIPLICACAO("multiplicação", "*"),
	DIVISAO("divisão", "/");
	
	private final String descricao;
	private final String simbolo;
	
	/**
	 * Operador
	 * @param descricao
	 * @param simbolo
	 */
	private Operador(String descricao, String simbolo) {
		this.descricao = descricao;
		this.simbolo = simbolo;
	}
}
```
Logo em seguida, crie os ```get's e set's``` para a classe ```Operacao```e como nosso enum possui todos os atributos final, crie apenas os métodos ``get's``.

Bom, temos algum código até aqui mas que não faz absolutamente nada por enquanto.  No próximo post estaremos incluindo alguns métodos com alguma lógica e também inciando com os testes de unidade.

Se você chegou até aqui, obrigado. Continuamos no próximo post.
Até {}!

