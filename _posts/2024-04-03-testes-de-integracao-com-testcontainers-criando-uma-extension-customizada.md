---
layout: post
title: Testes de integração com Testcontainers - Criando uma "Extension Customizada"
---

[![Capa post](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/capa-post-test-containers-02.png?raw=true)](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/capa-post-test-containers-02.png?raw=true)

Continuando a explorar os *Testes de Integração com TestContainer e Spring Boot*, cheguei num ponto onde foi preciso criar uma *Extension*
do JUnit personalizada para resolver um problema que estava acontecendo ao executar mais de uma classe de teste.

Bom, esse é o segundo post sobre testes de integração e caso não tenha lido o primeiro, de uma olhada onde explico como iniciei e como cheguei até aqui.
Estou usando o mesmo projeto do post anterior, então caso queira acompanhar e seguir a linha de raciocinio é interessante dar uma lida antes de iniciar esse aqui.

> - [ Github: Projeto TestContainers com Spring Boot ](https://github.com/BSTK/okk-blog-posts/tree/main/okk-testcontainers-com-springboot)

> - [ Post 1: Testes de integração com Testcontainers e Spring Boot ](http://brunoluz.com.br/2024/03/28/testes-de-integracao-com-testcontainres-e-spring-boot)

## A prática

Dessa vez vamos criar um teste para a **atualização de uma conta bancária**, onde eu já tenho uma conta cadastrada, e desejo atualizar as informações dessa conta.
O endpoind para este teste é bem simples:

```java
PUT /v1/api/contas-bancarias/1
{
  "nome": "Conta Nubank",
  "agencia": "9922",
  "conta": "4444-1",
  "banco": "222",
  "gerente": "Assunção",
  "observacao": "Observações sobre a conta bancária"
}
```
É o mesmo payload do primeiro endpoint onde cadastramos uma nova conta, agora o que muda é o método que passou a ser um **PUT** e o **Id** no final da url. 
Esse **Id** serve para identificar a conta que queremos atualizar.

## Criando o teste de integração

Eu decidi criar uma classe de teste para cada endpoint que irei testar, dito isso, "**duplique**" o teste ```ContaBancariaResourceITTest``` e renemeie para ```ContaBancariaResourceAtualizarContaITTest```. Pronto, devemos ter agora duas classes de teste.

Agora remova o caso de teste (o método anotado com ```@Test```), deixando a classe apenas com o código de configuração. Neste ponto, deve estar dessa forma:

```java
/// IMPORTS

@Testcontainers
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class ContaBancariaResourceAtualizarContaITTest {

  @Container
  private static final PostgreSQLContainer<?> POSTGRESQL_DB = new PostgreSQLContainer("postgres:14.1")
    .withDatabaseName("testcontainers-db")
    .withUsername("testcontainers-db")
    .withPassword("testcontainers-db");

  @DynamicPropertySource
  static void propertyConfig(final DynamicPropertyRegistry registry) {
    registry.add("spring.datasource.url", POSTGRESQL_DB::getJdbcUrl);
    registry.add("spring.datasource.username", POSTGRESQL_DB::getUsername);
    registry.add("spring.datasource.password", POSTGRESQL_DB::getPassword);
    registry.add("spring.datasource.driverClassName", POSTGRESQL_DB::getDriverClassName);

    registry.add("spring.flyway.url", POSTGRESQL_DB::getJdbcUrl);
    registry.add("spring.flyway.user", POSTGRESQL_DB::getUsername);
    registry.add("spring.flyway.password", POSTGRESQL_DB::getPassword);
  }

  @LocalServerPort
  private Integer portaHttp;

  @Autowired
  private ContaBancariaRepository contaBancariaRepository;

  @BeforeEach
  void setUp() {
    contaBancariaRepository.deleteAll();
  }
}
```

Perceba que é o mesmo código da classe de teste anteiror. Agora é hora de criarmos o caso de teste. Ele ficou dessa forma:

```java
@Test
@DisplayName("Deve atualizar dados da conta bancária")
void t1() {
  /// 1 - ARANGE
  final var contaBancariaJaExste = ContaBancaria.builder()
    .nome("Conta Nubank")
    .agencia("0001")
    .conta("4444-1")
    .banco("222")
    .gerente("Assunção")
    .observacao("Observações sobre a conta bancária")
    .build();
  
  contaBancariaRepository.saveAndFlush(contaBancariaJaExste);
  
  /// 1 - ARANGE - Crio a URL de Request com o Id da conta ja cadastrada que quero atualizar
  final var urlRequest = String.format("http://localhost:%s/v1/api/contas-bancarias/%s", portaHttp, contaBancariaJaExste.getId());
  
  /// 1 - ARANGE - Request com os novos dados
  final var contaBancariaComDadosAtualizadosRequest = ContaBancariaRequest.builder()
    .nome("Conta Bradesco")
    .agencia("2222")
    .conta("9999-1")
    .banco("382")
    .gerente("Maria Pereira")
    .observacao("Conta despezas da casa")
    .build();
  
  /// 2 - ACTION
  final var response = RestAssured
    .given()
    .header("Content-Type", "application/json")
    .and()
    .body(Json.toString(contaBancariaComDadosAtualizadosRequest))
    .when()
    .put(urlRequest)
    .then()
    .extract()
    .response();
  
  /// 3 - ASSERTS - Verifico se realmente atulizou os dados, pois a "response" tem que ser o mesmo que foi enviado na "request"
  Assertions.assertThat(response.statusCode()).isEqualTo(HttpStatus.OK.value());
  Assertions.assertThat(response.jsonPath().getString("nome")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getNome());
  Assertions.assertThat(response.jsonPath().getString("agencia")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getAgencia());
  Assertions.assertThat(response.jsonPath().getString("conta")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getConta());
  Assertions.assertThat(response.jsonPath().getString("gerente")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getGerente());
  Assertions.assertThat(response.jsonPath().getString("banco")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getBanco());
  Assertions.assertThat(response.jsonPath().getString("observacao")).isEqualTo(contaBancariaComDadosAtualizadosRequest.getObservacao());
  
  /// 3 - ASSERTS - Verifico se tem apenas uma conta, afinal foi uma atualização e não um novo cadastro
  Assertions
    .assertThat(contaBancariaRepository.findAll())
    .hasSize(1);

  /// 3 - ASSERTS - Agora verifico se a conta que está no banco os dados sãpo iguais aos novos que pedi para atualizar
  contaBancariaRepository
    .findById(contaBancariaJaExste.getId())
    .ifPresent(resultado -> {
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getNome()).isEqualTo(resultado.getNome());
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getAgencia()).isEqualTo(resultado.getAgencia());
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getConta()).isEqualTo(resultado.getConta());
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getBanco()).isEqualTo(resultado.getBanco());
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getGerente()).isEqualTo(resultado.getGerente());
      Assertions.assertThat(contaBancariaComDadosAtualizadosRequest.getObservacao()).isEqualTo(resultado.getObservacao());
    });
}
```

- Na sessão de ```ARANGE```, estou criando uma nova conta e inserindo no banco, afinal o banco de dados está zerado. Também estou criando a ```urlRequest``` e aproveitando o ```Id``` da conta recém cadastrada. Crio também a ```request``` com os novos dados, ou seja, os dados que vou atualizar. Nesso exemplo ficou assim:

```java
/// CONTA INICIAL
ContaBancaria.builder()
  .nome("Conta Nubank")
  .agencia("0001")
  .conta("4444-1")
  .banco("222")
  .gerente("Assunção")
  .observacao("Observações sobre a conta bancária")
  .build();

/// CONTA COM DADOS ATUALIZADOS
ContaBancariaRequest.builder()
  .nome("Conta Bradesco")
  .agencia("2222")
  .conta("9999-1")
  .banco("382")
  .gerente("Maria Pereira")
  .observacao("Conta despezas da casa")
  .build();
```

- Na sessão ```ACTION```, faço a requisição usando o ```RestAssured```, mas dessa vez enviando um ```PUT```.

- Na sessão ```ASSERTS```, faço minhas asserções.
  
  - **Primeiro**: Confirmos se a ```response``` que foi retornada tenha os mesmo dados da qual eu enviei
  - **Segundo**: Verifico se tem apenas uma conta, afinal foi uma atualização e não um novo cadastro
  - **Terceiro**: Agora verifico se a conta que está no banco, os dados são iguais aos novos que pedi para atualizar

Ok, rodando todos os testes, os dois irão executar, subuir container, fazer inserts, select, updates e tudo funcionando.

## Código duplicado não dá. É preciso refatorar.

Neste ponto, estamos com praticamente duas classes de testes idênticas e todo código de configuração é igual, ou seja **duplicado** e isso não é bom. Vamos refatorar e criar uma classe que servirá apenas para as configurações, assim podemos ```extende-la```, deixando nossas classes de teste apenas com o código que é de seu interesse, os casos de teste.

Crie uma nova classe no pacote raiz dos teste, e dê o nome de ```AppTestContainer```. Essa classe será responsavél por conter todo código de configuração, seu conteúdo ficou assim:

```java
/// IMPORTS

@Testcontainers
public class AppTestContainer {

  @Container
  protected static final PostgreSQLContainer<?> POSTGRESQL_DB = new PostgreSQLContainer("postgres:14.1")
    .withDatabaseName("testcontainers-db")
    .withUsername("testcontainers-db")
    .withPassword("testcontainers-db");

  @DynamicPropertySource
  static void propertyConfig(final DynamicPropertyRegistry registry) {
    registry.add("spring.datasource.url", POSTGRESQL_DB::getJdbcUrl);
    registry.add("spring.datasource.username", POSTGRESQL_DB::getUsername);
    registry.add("spring.datasource.password", POSTGRESQL_DB::getPassword);
    registry.add("spring.datasource.driverClassName", POSTGRESQL_DB::getDriverClassName);

    registry.add("spring.flyway.url", POSTGRESQL_DB::getJdbcUrl);
    registry.add("spring.flyway.user", POSTGRESQL_DB::getUsername);
    registry.add("spring.flyway.password", POSTGRESQL_DB::getPassword);
  }

  @BeforeAll
  protected static void startContainer() {
    POSTGRESQL_DB.start();
  }

  @AfterAll
  protected static void stopContainer() {
    POSTGRESQL_DB.stop();
  }
}
```

Agora podemos usar nas duas classes de testes, removendo assim o código duplicado:

- ```ContaBancariaResourceAtualizarContaITTest```

```java
/// IMPORTS

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class ContaBancariaResourceAtualizarContaITTest extends AppTestContainer {

  // ....
}
```

- ```ContaBancariaResourceITTest```

```java
/// IMPORTS
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class ContaBancariaResourceITTest extends AppTestContainer {

  // ....
}
```

Pronto, podemos executar cada uma delas que tudo continuará funcionando numa boa, agora sem a repetição de código. Repetir código nunca é bom, mesmo que seja o código de teste.

## Deu ruim. Está quebrando tudo.

Se você executar as duas ```classes de teste, uma de cada vez``` vai funcionar perfeitamente. O problema ocorre quando você tenta executar todas as classes de teste de uma só vez. Isso você consegue na IDE (IntelliJ) clicando no pacote os estão seus testes ( que no nosso caso é ```dev.bstk.testcontainerscomspringboot.api```) e pedindo para rodar todos:

[![POST-TEST-CONTEINER-II-EXECUTANDO-TODO-OS-TESTES](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/POST-TEST-CONTEINER-II-EXECUTANDO-TODO-OS-TESTES.png?raw=true)](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/POST-TEST-CONTEINER-II-EXECUTANDO-TODO-OS-TESTES.png?raw=true)

Vai ocorrer o seguinte erro:

[![POST-TEST-CONTEINER-II-EXECUTANDO-TODO-OS-TESTES](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/POST-TEST-CONTEINER-II-TESTE-FALHANDO.png?raw=true)](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/ POST-TEST-CONTEINER-II-TESTE-FALHANDO.png?raw=true)

Isso está acontecendo porque quando é executando a primeira classe de teste, o TestContainer sobe ```UM CONTAINER DO POSTGRESQL```, ai quando todos os casos de testes dessa primeira classe finalizar, ele tenta ```PARAR O CONTAINER DO POSTGRESQL```. Até ai ok, mas isso vai se repetir quando for executar os casos de teste da segunda classe de teste, assim, ele tenta mais uma vez subir um novo container mas o primeiro ainda não foi finalizado totalmente, assim não conseguindo subir um novo container.

É meio confuso, então vamos tentar visualizar:

```java
CLASSE DE TESTE A
  - SOBE CONTAINER A
    - EXECUTA CASO DE TESTE 1
    - EXECUTA CASO DE TESTE 2
  - TENTA FINALIZAR CONTAINER A

CLASSE DE TESTE B
  - SOBE CONTAINER B
  - AINDA NÃO FINALIZOU O CONTAINER A 
    - EXECUTA CASO DE TESTE 1
    - ERRO DE CONEXÃO, NÃO TEM UM CONTAINER ATIVO
```

O legal seria ter apenas um container para todas as classes de teste, algo assim:

```java
- SOBE CONTAINER_POSTGRESQL

    CLASSE DE TESTE A
    - EXECUTA CASO DE TESTE 1
    - EXECUTA CASO DE TESTE 2

    CLASSE DE TESTE B
    - EXECUTA CASO DE TESTE 1
    - EXECUTA CASO DE TESTE 2

    CLASSE DE TESTE C
    - EXECUTA CASO DE TESTE 1
    - EXECUTA CASO DE TESTE 2

- FINALIZA CONTAINER_POSTGRESQL
```

Bom, dá pra fazer isso ai e consegui por meio de uma ```extensão customizada do JUnit```.

## Criando nossa extensão customizada

Para que possamos ter o comportamento de apenas um container para todos os teste, vamos criar um ```extension```.
Você já deve ter usando uma, algo como: 

```java
@ExtendWith(SpringExtension.class)
@ExtendWith(MockitoExtension.class)
```

Pois bem, vamos criar a nossa, chamando de ```AppTestExtension``` e usaremos dessa forma:
```java
@ExtendWith(AppTestExtension.class)
```

Nossa extensão ficou assim:

```java
public class AppTestExtension implements BeforeAllCallback, ExtensionContext.Store.CloseableResource {

  private static boolean START = false;

  @Override
  public void beforeAll(ExtensionContext extensionContext) {
    if (!START) {
      START = true;
      AppTestContainer.startContainer();
      extensionContext.getRoot().getStore(GLOBAL).put(AppTestExtension.class.getName(), this);
    }
  }

  @Override
  public void close() {
    AppTestContainer.stopContainer();
  }
}
```

- O método ```beforeAll()``` será executado ```antes``` de cada classe de teste começar a executar. Porém aqui colocamos uma flag booleana, assim podemos fazer com que o conteúdo desse método seja executado apenas uma vez. Um ```Hackzinho maroto!```.
Bom, assim fica um ponto ideal para iniciarmos na mão o nosso container:

```java
AppTestContainer.startContainer();
```

- O método ```close()``` esse sim, será executado quando todas as classes de testes terminar de executar, novamente um ponto ideal para finalizarmos o container:
```java
AppTestContainer.stopContainer();
```

Para que seja feito assim foi preciso ajustar alguns pontos na classe ```AppTestContainer```, e os pontos são esses:

- Deixei os métodos ```startContainer``` e ```stopContainer``` como publicos e estáticos.

```java
public static void startContainer() {
    POSTGRESQL_DB.start();
    log.info("\n\n");
    log.info("***********************************************");
    log.info("**** INICIANDO O CONTAINER : POSTGRESQL_DB ****");
    log.info("***********************************************");
    log.info("\n\n");
  }

public static void stopContainer() {
  POSTGRESQL_DB.stop();
  log.info("\n\n");
  log.info("*************************************************");
  log.info("**** FINALIZANDO O CONTAINER : POSTGRESQL_DB ****");
  log.info("*************************************************");
  log.info("\n\n");
}
```

- Deixei a definição do nosso container como ```privado``` e adicionei a configuração: ```.withReuse(true)``` (Bom, fiz uns teste aqui e não precisou, funciona sem, mas na documentação diz algo que é bom usar)

```java
private static final PostgreSQLContainer<?> POSTGRESQL_DB = new PostgreSQLContainer<>("postgres:14.1")
    .withDatabaseName("testcontainers-db")
    .withUsername("testcontainers-db")
    .withPassword("testcontainers-db")
    .withReuse(true);
```

> Aviso: Para usar ```.withReuse(true)```, você precisa configurar um arquivo oculto chamado ```.testcontainers.properties ``` que está na pasta home do seu usuário, deixando ele assim:

```java
# Adicionar linha:
testcontainers.reuse.enable=true
```

Pronto, agora executando todos os teste, eles voltam a funcionar direitinho, e só com um container para todos os testes.

[![POST-TEST-CONTEINER-II-EXECUTANDO-TODO-OS-TESTES](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/POST-TEST-CONTEINER-II-TESTE-FUNCIONANDO.png?raw=true)](https://github.com/BSTK/bstk.github.io/blob/master/assets/image/POST-TEST-CONTEINER-II-TESTE-FUNCIONANDO.png?raw=true)

Procure o log da IDE as marcações que deixamos e veja que só será executado apenas uma vez!

```java
INFO - INICIANDO O CONTAINER : POSTGRESQL_DB

INFO - FINALIZANDO O CONTAINER : POSTGRESQL_DB
```

Ficou extenso esse post, muita coisa mas é divetido. Criar testes de integração, extensões e etc é sair do básico e ir um pouco além.

Bom é isso ai, espero que tenha curtido e até a próxima!

Compartilhe, dê uma estrela la no GitHub.

Links:
 - [ Github: Projeto TestContainers com Spring Boot ](https://github.com/BSTK/okk-blog-posts/tree/main/okk-testcontainers-com-springboot)

 - [ Post 1: Testes de integração com Testcontainers e Spring Boot ](http://brunoluz.com.br/2024/03/28/testes-de-integracao-com-testcontainres-e-spring-boot)