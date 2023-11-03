---
layout: post
title: Connascence, uma métrica obvia mas nem tanto
categories: [content, demo]
---

A leitura sempre mostra coisas que nem sabemos que existe, simplesmente pelo fato de nunca ter ouvido falar ou ler sobre, mas que estavam lá só esperando para ser descoberta.
Estou lendo o livro **Fundamentals of Software Architecture: An Engineering Approach** escrito por *Mark Richards, Neal Ford* e no terceiro
capitulo é mostrado o conceito de **Connascence**. Venha ver o que descobri.

Como bons dev's que somos :-), passamos boa parte do nosso tempo programando (ou deveriamos) e coisas
que parecem ser obvias não são ta obvias assim.
Você ja ouviu falar em *"Connascence"*? Eu nunca tinha ouvido falar. Não sei se essa palavra tem tradução para o Português,
e ao tentar traduzi-la no Google Translater, sai algo como: *"Connascência / Conascência"*.
Bom, *Connascence* trata-se de uma "métrica de acoplamento" criada
por Meilir Page-Jones e publicado no livro *"What Every Programmer Should Know About
Object-Oriented Design"* de 1996.

De modo geral, significa:

> Dois componentes são conascentes se uma alteração em um deles exigir que o outro seja
> modificado para manter a correção geral do sistema

E quando se diz: *Dois componentes são conascentes ...* podemos pensar em componentes
como classes, pacotes e até mesmo (micros)serviços diferentes. Existem dois tipos de Conascência: *Conascência Estática* e *Conascência Dinâmica*.

### Conascência Estática
A Conascência Estática refere-se ao acoplamento no nivél do código-fonte. Coisas que são percebidas
no momento da escrita/leitura do código.

Aqui temos 5 subtipos:

1. *Conascência de nome (CoN)* - Vários componentes devem concordar com o nome de uma entidade.
  - Por exemplo o nome dos métodos, que representa a forma mais comum de acoplamento entre uma definição e uma chamada

2. *Conascência de tipo (CoT)* - Vários componentes devem concordar com o tipo de uma entidade
  - Aqui podemos dizer sobre o recurso comum em linguagens estaticamente tipadas de limitar
variaveis e parâmetros a tipos especificos.

3. *Conascência de Significado (CoS) ou Connascence de Convenção (CoC)* - Vários componentes devem concordar com o
significados de valores especificos.
  
  - Uma das formas mais comuns são aqueles números mágicos espalhados pelo código que em componentes diferentes
devem ter o mesmo valor e significado para que funcione perfeitamente.
```java
class A {
  final int pessoaFisica = 1;
  final int pessoaJuridica = 2; 
}

...

class B {
  final int pessoaFisica = 1;
  final int pessoaJuridica = 2; 
  
  ... 
    
  if (pessoaFisica == 1) { ... }
  if (pessoaJuridica == 2) { ... }
}
```
  - Em linguagens mais antigas que não possi um tipo de dado ```boolean```. Imagina o tanto de problemas ao inverter esses valores
```java
int FALSE = 0;
int TRUE = 1;
```
  > Aqui uma forma que vejo para se corrigir a CoS/CoC, ter um bom funcionamento e um código fácil de manter, seria o uso de constantes. No Java temos os ```Enums``` que ajudam muito nos casos
envolvendo tipos, números mágicos e etc. Também temos a opção da queria ```ConstantsUtil``` (essa aqui todo projeto você acaba encontrando)


4. *Conascência de posição (CoP)* - Vários componentes devem concordar com a ordem dos valores.
  - Um exemplo de quando isso acontece é quando temos um método que possui dois parâmetros do mesmo tipo e que só
pode funcionar direito se a ordem for obedecida. O código abaixo compila, irá rodar normalmente porém não funciona da maneira que deveria.
    Sim, pode parecer algo banal, bobo mas como diria um grande colega: "É raro mas acontece sempre!"

```java
Response buscarDadosRelatorio(final String codigoRelatorio, final String email) { ... }

final Response responseAA = buscarDadosRelatorio("1B", "email@gmail.com");
final Response responseBB = buscarDadosRelatorio("email@gmail.com", "1B");
```

> Essa parece ser mais dificil de resolver, até mesmo por se tratar do tipo de dado e algo da pŕopria linguagem.
> Uma forma de corrigir seria fazer validações no método que recebe os parâmetros antes de serem utilizados.

5. *Conascência de Algoritmo (CoA)* - Vários componentes devem concordar com um algoritmo específico.

  - Aqui é citado um exemplo de quando um algoritmo hash de segurança por exemplo é executado no servidor e no cliente, e
ambos devem produzir resultados idênticos. Obviamente isso demonstra um alto grau de acoplamento, pois se em 
uma das parte mudar algum detalhe, a funcionalidade não irá funcionar corretamente.

Em um próximo post, continuarei falando da *Conascência Dinâmica*, que por sua natureza ser algo que 
acontece apenas em *tempo de execução* é muito mais dificil de se perceber.

Como disse parece coisas obvias, pequenas, mas que acontece bastante. A medida que a base de código e 
a quantidade de desenvolvedores aumenta, a *Conascência* acaba aparecendo com certa frequência.

