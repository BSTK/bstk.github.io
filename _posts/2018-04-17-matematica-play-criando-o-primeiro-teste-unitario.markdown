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

As coisas começaram a ficar legais, já criamos até nosso primeiro teste unitário. 

Bom é hora de refatorar, hora de melhorar nosso método ```toEnum(String simbolo)```, veja abaixo como ficou a nova versão :

```java
	public static Operador toEnum(String simbolo) {
	    Assert.isTrue("+-*/".contains(simbolo), "O simbolo " + simbolo + " é um operador inválido");
		return Stream.of(values())
					.filter(o -> o.getSimbolo().equals(simbolo))
					.findFirst()
					.get();
	}
```

Usamos a classe utilitária ```Assert.isTrue()``` para validar caso seja informado um caractere inválido (todos diferentes de + - * / ). Usamos também a api ```Stream``` paar filtar (**```filter(o -> o.getSimbolo().equals(simbolo))```**) na lista de enuns qual deles é correspondente ao simbolo passado por parâmetro e pegar o primeiro elemento .
> ***`filter(o -> o.getSimbolo().equals(simbolo))`*** está fazendo a mesma coisa que fizemos com o **if** na primeira versão do método.

Caso você execute o teste unitário, perceberá que o método de teste ```testDeveRetornarNullPorSimboloInvalido()``` irá falhar pois alteramos a lógica do código.

> java.lang.IllegalArgumentException: O simbolo & é um operador inválid

Para corrigir isso, vamos refatora-lo também, está é a nova versão :
```java
@Test
public void testDeveRetornarNullPorSimboloInvalido() {
		
	assertThatThrownBy(() -> { Operador.toEnum("&"); })
				.isInstanceOf(IllegalArgumentException.class).hasMessageContaining("operador inválido");
				
	assertThatThrownBy(() -> { Operador.toEnum("#"); })
				.isInstanceOf(IllegalArgumentException.class).hasMessageContaining("operador inválido");
				
	assertThatThrownBy(() -> { Operador.toEnum("@"); })
				.isInstanceOf(IllegalArgumentException.class).hasMessageContaining("operador inválido");
				
	assertThatThrownBy(() -> { Operador.toEnum("("); })
				.isInstanceOf(IllegalArgumentException.class).hasMessageContaining("operador inválido");
}
```
O que fizemos agora é validar no teste para que quando executarmos o método ```toEnum``` passando um simbolo invalido, será lançada uma exceção ```IllegalArgumentException``` contendo na mensagem de erro o trecho ```operador inválido``` .

O teste irá passar, tudo voltará a funcionar mas não ficou legal. Perceba que temos muita coisa se repetindo. É hora de refatorar novamente.

Veja agora como ficou retirando todo código repetido:

```java
@Test
public void testDeveRetornarNullPorSimboloInvalido() {
	assertThatError("&");
	assertThatError("#");
	assertThatError("@");
	assertThatError("(");	
}
	
	
private void assertThatError(String simbolo) {
	assertThatThrownBy(() -> { Operador.toEnum(simbolo); })
				.isInstanceOf(IllegalArgumentException.class).hasMessageContaining("operador inválido");
}
```

Criamos um novo método ```assertThatError(String simbolo)``` que isola todo o código que se repetia, passando por parâmetro o simbolo que desejamos testar.

É, fizemos bastante coisa até aqui, foi um post e tanto, mas você percebeu o como é legal ir refatorando nosso código até chegar numa versão que achamos ser a melhor. Pode ser que você olhe o código como está e queira modificar algo, vai lá, siga em frente, mas lembre-se de executar os testes a cada mudança para garantir que tudo permanece funcionando.

Se você chegou até aqui, obrigado. Continuamos no próximo post.

Até {}!