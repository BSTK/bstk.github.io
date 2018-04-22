---
layout: post
title:  "Matemática Play - Criando a camada de serviços"
date:   2018-04-17 00:00:03 +0530
categories: spring-boot
---


Após construirmos a parte de dominio do nosso aplicativo, vamos dar mais um passo e começar a construir a camada de serviço.

Para isso, crie o pacote :
> com.kuiiz.matematicaplay.operacao.service

Nele criaremos as interfaces : ```OperacaoService```  e ```GeradorAleatorioService```

O código das interfaces ficou desta forma :
```java
public interface OperacaoService {
	Operacao criaUmaOperacaoAleatoria();
}
```
```java
public interface GeradorAleatorioService {
	int gerarFator();
	Operador gerarOperador();	
}
```
Vamos criar agora a implementação da interface ```OperacaoService```. Crie a classe  ```OperacaoServiceImpl```.
O código completo ficará desta forma : 

```java
@Service
public class OperacaoServiceImpl implements OperacaoService {
	
	@Autowired
	private GeradorAleatorioService geradorService;

	@Override
	public Operacao criaUmaOperacaoAleatoria() {
		
		int fatorA = geradorService.gerarFator();
		int fatorB = geradorService.gerarFator();
		Operador operador = geradorService.gerarOperador();
		
		return new Operacao(fatorA, fatorB, operador);
	}
}
```
A annotation ```@Service``` especifica  que nossa classe será um serviço gerenciado pelo **Spring** e assim poderá ser injetada em outras classes através do recurso de injeção de dependência. Além do mais ela é uma especialização da anotação ```@Component```.

No trecho abaixo temos a injeção do serviço ```GeradorAleatorioService```  no qual o **Spring** nos fornecerá uma instancia desta classe.

```java
@Autowired  
private GeradorAleatorioService geradorService;
```

O método ```criaUmaOperacaoAleatoria()``` é bem simples, ele criará um objeto ```Operacao``` com ajuda do serviço ```GeradorAleatorioService```  que cria os fatores (A e B) e um ```Operador``` de forma aleatória.

Com todo esse código, já é hora de criarmos um teste e executar a chamada do serviço e verificar se as operações estão sendo criadas da forma desejada.

Crie o pacote :
> com.kuiiz.matematicaplay.operacao.service

na pasta de testes ```src/test/java```

Agora crie a classe de teste : ```OperacaoServiceTest``` com o método ```void testCriaUmaOperacaoValidandoOResultado()```.

Vejamos o código abaixo :
```java
// Imports 
import static com.kuiiz.matematicaplay.operacao.domain.Operador.SOMA;
import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.BDDMockito.given;

// ... Demais imports

@SpringBootTest
@RunWith(SpringRunner.class)
public class OperacaoServiceTest {
	
	@MockBean
	public GeradorAleatorioService geradorService;
	
	@Autowired
	private OperacaoService operacaoService;
	
	@Test
	public void testCriaUmaOperacaoValidandoOResultado() {		
		
		/**
		 * Dado a chamada de geradorService.geraFator(), deve me retornar 100 e 50
		 */
		given(geradorService.gerarFator()).willReturn(100, 50);
		
		/**
		 * Dado a chamada de gerarOperador(), deve me retornar um operador (+ - * \/)
		 */
		given(geradorService.gerarOperador()).willReturn(SOMA);
		
		/**
		 * Quando eu chamar o método operacaoService.criaUmaOperacaoRadomica()
		 */
		Operacao operacao = operacaoService.criaUmaOperacaoAleatoria();
		
		/**
		 * Então devo ter os seguintes resultados
		 */
		assertThat(operacao.getFatorA()).isEqualTo(100);
		assertThat(operacao.getFatorB()).isEqualTo(50);
		assertThat(operacao.getOperador()).isEqualTo(SOMA);
		assertThat(operacao.getResultado()).isEqualTo(150);
	}
}
```
Este teste tem muitas coisas novas que não tínhamos visto até então. Vamos analisa-lo :

```@SpringBootTest``` está anotação será responsável por carregar o contexto do **Spring**, assim sendo possível fazer injeções de dependencias nos nossos testes através da anotação ```@Autowired```.

```@RunWith(SpringRunner.class)``` está anotação especifica quem rodará nosso teste, não sendo mais a forma padrão que o **JUnit** executa e sim algo apropriado ao **Spring**.

```@MockBean``` é muito importante aqui, pois ela diz ao **Spring** para injetar um *mock (objeto simulado)* da classe  ```GeradorAleatorioService``` ja que ainda não possuímos uma implementação concreta desta interface.

O restante do código é auto-explicativo através dos comentários inseridos em cada passo do teste. Novamente estamos usando o **[Assertj](http://joel-costigliola.github.io/assertj)** ```(assertThat())``` e algo novo que é o **[Mockito](http://site.mockito.org/)** ```(given())``` .

Perceba que estamos testando apenas uma operação de soma :
```java
assertThat(operacao.getOperador()).isEqualTo(SOMA);
```
O correto seria testarmos todas as opções disponíveis. Hora de refatorar !
Crie o método ```private void executaOperacao(int fatorA, Operador operador, int fatorB, int resultado)```.

```java
private void executaOperacao(int fatorA, Operador operador, int fatorB, int resultado) {
		
	/**
	 * Dado a chamada de geradorService.geraFator(), deve me retornar 100 e 50
	 */
	given(geradorService.gerarFator()).willReturn(fatorA, fatorB);
		
	/**
	 * Dado a chamada de gerarOperador(), deve me retornar a operação soma (Operador.DIVISAO)
	 */
	given(geradorService.gerarOperador()).willReturn(operador);
		
	/**
	 * Quando eu chamar o método operacaoService.criaUmaOperacaoRadomica()
	 */
	Operacao operacao = operacaoService.criaUmaOperacaoAleatoria();
		
	/**
	 * Então devo ter os seguintes resultados
	 */
	assertThat(operacao.getFatorA()).isEqualTo(fatorA);
	assertThat(operacao.getFatorB()).isEqualTo(fatorB);
	assertThat(operacao.getOperador()).isEqualTo(operador);
	assertThat(operacao.getResultado()).isEqualTo(resultado);
}
```
Agora nosso método de teste pode executar todas as operações sem repetir código : 
```java
@Test
public void testCriaUmaOperacaoValidandoOResultado() {		
		
	executaOperacao(100, SOMA, 50, 150);
	executaOperacao(100, SUBTRACAO, 50, 50);
	executaOperacao(100, MULTIPLICACAO, 50, 5000);
	executaOperacao(100, DIVISAO, 50, 2);		
}
```
Se você prestar atenção na chamado do método :
```java
executaOperacao(100, SOMA,  50,  150);
```
verá que ficou de forma bem semântica e de fácil compreensão, lendo-o método, temos algo como : 
>executa uma operação 100 SOMA 50 resultado 150



Bom chegamos ao final de mais um post, desta vez com um teste de integração bem completo utilizando tudo que já foi criado. Caso você tente executar o aplicativo agora, ele mostrará um erro dizendo que não pode injetar o bean ```GeradorAleatorioService```.

> **'com.kuiiz.matematicaplay.operacao.service.GeradorAleatorioService' that could not be found.**

Não se preocupe, já iremos corrigir isso no próximo passo (-:

Se você chegou até aqui, obrigado. Continuamos no próximo post.

Até {}!