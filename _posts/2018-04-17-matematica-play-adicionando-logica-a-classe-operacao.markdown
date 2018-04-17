---
layout: post
title:  "Matemática Play - Adicionando lógica a classe Operação"
date:   2018-04-17 00:00:02 +0530
categories: spring-boot
---


Vamos voltar a classe ```Operacao``` e criar alguns métodos para dar alguma lógica a ela.

Vamos criar o método ```private void efetuarOperacao()``` que como o próprio nome diz, executa a operação matemática de acordo com os dados passados no construtor quando criamos um objeto.

Veja como ficou o código :
```java
private void efetuarOperacao() {
  switch (operador) {
	case SOMA : 		
		resultado = fatorA + fatorB; break;
	case SUBTRACAO: 	
		resultado = fatorA - fatorB; break;
	case MULTIPLICACAO: 
		resultado = fatorA * fatorB; break;
	case DIVISAO: 		
		validaDivisaoPorZero();
		resultado = fatorA / fatorB; break;
  }
}
```

Também criamos o método ```private void validaDivisaoPorZero()``` para validar caso o segundo fator da operação de divisão seja zero (0).

```java
private void validaDivisaoPorZero() {
  if (operador.equals(Operador.DIVISAO)) {
	if (fatorB == 0) {
	  throw new IllegalArgumentException("Na operação de divisão (/) ofatorB não pode ser zero (0)");
	}
  }		
}
```

Em seguida, sobrescreva o método ```toString()``` para que fique melhor apresentável na hora de depurar ou gerar log.


```java
@Override
public String toString() {
  return new StringBuilder()
		.append("Operacao=[ ")
		.append("fatorA = " + fatorA)
		.append(", fatorB = " + fatorB)
		.append(", operador = " + operador.getSimbolo())
		.append(String.format(", resultado (%s %s %s) = %s", fatorA,operador.getSimbolo(), fatorB, resultado))
		.append(" ]")
		.toString();
}
```

Antes de terminar está classe, adicione a chamada do método ```efetuarOperacao()``` no construtor : 


```java
public Operacao(int fatorA, int fatorB, Operador operador) {
  this.fatorA = fatorA;
  this.fatorB = fatorB;
  this.operador = operador;
  this.efetuarOperacao();
}
```

E aproveite para apagar o construtor padrão que criamos em posts anteriores. 

Este post será bem curto, apenas para completarmos essa parte de domínio. Nos próximo vamos criar nossas classes e interfaces de serviços e também um teste integrado mesmo sem ter a implementação principal construída  afim de usarmos **objetos simulados (mocks)** e toda facilidade de testes que o **Spring Boot** nos fornece.


Se você chegou até aqui, obrigado. Continuamos no próximo post.

Até {}!
