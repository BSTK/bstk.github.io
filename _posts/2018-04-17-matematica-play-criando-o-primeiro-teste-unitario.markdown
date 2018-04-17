---
layout: post
title:  "Matemática Play - Criando o primeiro Teste Unitário"
date:   2018-04-17 00:00:00 +0530
categories: spring-boot
---


Nosso enum ```Operador``` não tem nada demais, então vamos adicionar o método ```toEnum(String simbolo)``` fazendo uma primeira versão e depois refatora-lo para usar alguns recursos do **Java 8** .

```java
public static Operador toEnum(String simbolo) {		
	Operador operador = null;
		
	for (Operador o : values()) {
		if (o.getSimbolo().equals(simbolo)) {
			operador = o;
			break;
		}
	}	
	return operador;
}
```

Este método será útil quando quisermos criar um objeto ```Operador``` através do seu simbolo (+, -, *, /). Um exemplo de uso seria :
```java
Operador soma = Operador.toEnum("+");
Operador subtracao = Operador.toEnum("-");
Operador multiplicacao = Operador.toEnum("*");
Operador divisao = Operador.toEnum("/");
```
Agora que nosso método está pronto, vamos criar um teste unitário para garantir que a lógica implementada esteja de fato correta.

Na pasta ```src/test/java``` crie o pacote :
> com.kuiiz.matematicaplay.operacao.domain

Em seguida, cria a classe ```OperadorTest``` que é o mesmo nome do nosso enum mas com sufixo ```Test```.

 Vamos incluir dois métodos de testes, um para testar o caminho feliz : 
 ```void testCriaUmOperadorDeAcorComSeuSimbolo()``` 
 e outro para o caminho de falha ```void testDeveRetornarNullPorSimboloInvalido()```.

A classe completa tem o seguinte código :

```java
package com.kuiiz.matematicaplay.operacao;

import static org.assertj.core.api.Assertions.assertThat;
import org.junit.Test;
import com.kuiiz.matematicaplay.operacao.domain.Operador;

public class OperadorTest {

	@Test
	public void testCriaUmOperadorDeAcorComSeuSimbolo() {
		assertThat(Operador.toEnum("+")).isEqualTo(Operador.SOMA);
		assertThat(Operador.toEnum("")).isEqualTo(Operador.SUBTRACAO);
		assertThat(Operador.toEnum("*")).isEqualTo(Operador.MULTIPLICACAO);
		assertThat(Operador.toEnum("/")).isEqualTo(Operador.DIVISAO);		
	}
	
	@Test
	public void testDeveRetornarNullPorSimboloInvalido() {
		assertThat(Operador.toEnum2("&")).isNull();
		assertThat(Operador.toEnum2("#")).isNull();
		assertThat(Operador.toEnum2("@")).isNull();
		assertThat(Operador.toEnum2("(")).isNull();
	}
}
```

Vamos algumas explicações. A annotation ```@Test``` é do framework **[JUnit](https://junit.org/junit5/)** e serve para dizer que aquele método será um caso de teste. Perceba que o nome do método é bem grande e muito descritivo, assim fica fácil entender de cara o que aquele caso está testando.

Estamos utilizando o  **[AssertJ](http://joel-costigliola.github.io/assertj/)** (```assertThat(), isEqualTo(), isNull()```) que fornece uma api fluente para nos ajudar a validar (asserts) nossos testes.

Lembrando que quando criamos nosso projeto usando o **Spring Initializr** ele já vem com a dependência  do ```spring-boot-starter-test``` que fornece todas as bibliotecas necessárias para o uso de testes automatizados.

Se você olhar o arquivo ```pom.xml```, verá a dependência :
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-test</artifactId>
	<scope>test</scope>
</dependency>
``` 

As coisas começaram a ficar legais, já criamos até nosso primeiro teste unitário. No próximo post, estaremos refatorando o método ```toEnum(String simbolo)``` para poder usar os recursos do Java 8.

Se você chegou até aqui, obrigado. Continuamos no próximo post.

Até {}!
