---
layout: post
title: Testes de integração com TestContainrs e Spring Boot
---

Eu sempre gostei de fazer testes em meus projetos, principalmente testes unitários que além ser um boa prática quando estamos escrevendo software é também uma forma de garantir a qualidade. De toda forma somente testes unitários não são suficientes, então foi preciso buscar outras formas de testar a aplicação, dai comecei a olhar para os testes de integração.

A bola da vez está sendo aprender e mexer com **TestContainres** e a forma como ela ajuda e muito na hora de lidar com as **dependências externas**, sejam elas banco de dados, messages brokers, serviços de cache ou qualquer outra coisa que possa subir num container Docker.

### O que é TestContainrs

Aqui eu deixo a definição do próprio site deles
> Testcontaines é um framework código aberto para fornecer instâncias leves e descartáveis ​​de bancos de dados, corretores de mensagens, navegadores da web ou praticamente qualquer coisa que possa ser executada em um contêiner Docker.

Você pode conhecer mais sobre a ferramente aqui: [ https://testcontainers.com ](https://testcontainers.com/)


Dito isso, não precisamos mais "mockar" nossas dependências e sim usamos de forma real, fazendo inserts reais no banco de dados, publicando/consumindo mensagens reais de brokers e etc. O melhor é que ao invés de usar coisas que "simulam" um banco de daos por exemplo com H2 o que certamente é diferente do banco de dados real de produção, usamos o mesmo banco, com a mesma versão, não tendo ai uma diferença do seu teste para o que de fato é executado no ambiente de produção.

### A prática

Na aplicação de exemplo, vou testar de forma integrada um endpoind de uma api rest com Spring Boot e com banco de dados PostgreSql.

> Você precisar ter o Docker instalado em sua máquina

### Configurando o projetos
Sugiro dar uma olhada no projeto completo do github:

- [ Projeto TestContainers com Spring Boot ](https://github.com/BSTK/okk-blog-posts/tree/main/okk-testcontainers-com-springboot)

No ```pom.xml``` além das dependencias principais para que a api funcione (Spring Web, Spring Data, Drive de Conexao, JUnit), adicione as seguintes dependencias referentes ao TestContainer e Rest Assured, que servirá para fazermos a requisição para api :

```xml
<!-- Test Containers -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-testcontainers</artifactId>
    <scope>test</scope>
</dependency>

<!-- Test Containers JUnit -->
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
</dependency>

<!-- Test Containers PostgreSql -->
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <scope>test</scope>
</dependency>

<!-- Rest Assured -->
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>${rest-assured.version}</version>
    <scope>test</scope>
</dependency>
```

O endpoint que vamos testar é bem simples: ele recebe uma request, valida alguns dados obrigatórios e insere uma nova conta bancará. Temos também uma validação para que não permita cadastrar uma conta que já exista.

```json
POST http://localhost:8080/v1/api/contas-bancarias

{
  "nome": "Conta Nubank",
  "agencia": "9922",
  "conta": "4444-1",
	"banco": "222",
  "gerente": "Pedro Diniz",
  "observacao": "Observações sobre a conta bancária AA"
}
```

## Criando o teste de integração

Bom, vamos criar uma classe comum de teste assim como criamos para testes de unidade. Vou chamar está classe de ```ContaBancariaResourceITTest```, o ```IT``` ali no meio é de ```Integration Test```. Aqui está a classe com a configuração básica:

```java
/// IMPORTS

@Testcontainers
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class ContaBancariaResourceITTest {

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

  /// ...
}
```

Vamos analisar cada uma dessas annotations:

- ```@Testcontainers```

    - Bom, aqui vou sar a própria explicação da documentação JavaDoc da annotatiom:
      *@Testcontainers é uma extensão JUnit Jupiter para ativar a inicialização e parada automática de contêineres usados em cada caso de teste. A extensão encontra todos os campos anotados com Container e chama seus métodos de ciclo de vida do contêiner*

- ```@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)```

    - A ```@SpringBootTest``` serve para subir todo o contexto do Spring, assim posso injetar meus beans normalmente sem a necessidade de criar mocks. A configuração ```webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT``` faz com que quando o contexto web estiver disponivél ele suba numa por aleatória, dessa forma não precisamos setar hardcode um porta. Isso é legal pois quando esses testes são executados numa esteira CI/CD ou algo do tipo não corremos o risco de configurar uma porta que ja esteja sendo utilizada por outra aplicação qualquer que seja

- ```@Container```

    - Com essa annotation definimos um container no qual queremos usar que neste caso é do postgresql. Ali conseguimos configurar o nome do banco de dados, o usuário e a senha. Uma coisa legal também é que estou utilizando a mesma imagem de container que usuria em produção: ```new PostgreSQLContainer("postgres:14.1")```. Se você der uma olhada no docker composer que acompanha esse projeto, percenerá que é o mesmo nome de imagem.

- ```@DynamicPropertySource```

    - Aqui estamos configurando/sobrescrevendo as propriedades de datasource, flyway de acordo com o que foi configurado no container do banco de dados. Senão fizessemos assim, o Spring pegaria exatamente o que está configurado no arquivo
      ```application.properties``` e possa ser que daria errado

- ```@LocalServerPort private Integer portaHttp;```

    - Aqui injetando a porta que foi definida de forma randomica como explicado acima.

- ```@Autowired private ContaBancariaRepository contaBancariaRepository;```

    - Aqui injetamos o ```ContaBancariaRepository``` para que possamos fazer querys no banco de dados e validar algumas coisas com nossos ```Asserts```. Exemplo: Quando executarmos o teste para cadastrar uma nova conta, ao final da chamada da api deve ter no banco um registro no qual nós acabamos de cadastrar.

- ```@BeforeEach```
    - Essa anotação faz com que o método ```setUp()``` seja executado toda vez antes de cada caso de teste, assim é um ponto ideal para limparmos o banco de deixar zerado para que inserts de outros testes não interfira no teste que está sendo executado no momento.

Bom, de configuração é isso (bastante coisa), agora vamos de fato para o caso de teste

```java
/// ...

@Test
@DisplayName("Deve cadastrar uma nova conta bancária")
void t1() {
  /// 1 - ARANGE
  final var urlRequest = String.format("http://localhost:%s/v1/api/contas-bancarias", portaHttp);
  final var novaContaBancariaRequest = ContaBancariaRequest.builder()
    .nome("Conta Nubank")
    .agencia("0001")
    .conta("4444-1")
    .banco("222")
    .gerente("Assunção")
    .observacao("Observações sobre a conta bancária")
    .build();

  /// 2 - ACT
  final var response = RestAssured
    .given()
    .header("Content-Type", "application/json")
    .and()
    .body(Json.toString(novaContaBancariaRequest))
    .when()
    .post(urlRequest)
    .then()
    .extract()
    .response();

  /// 3 - ASSERTS
  Assertions.assertThat(response.statusCode()).isEqualTo(HttpStatus.CREATED.value());
  Assertions.assertThat(response.jsonPath().getString("nome")).isEqualTo(novaContaBancariaRequest.getNome());
  Assertions.assertThat(response.jsonPath().getString("agencia")).isEqualTo(novaContaBancariaRequest.getAgencia());
  Assertions.assertThat(response.jsonPath().getString("conta")).isEqualTo(novaContaBancariaRequest.getConta());
  Assertions.assertThat(response.jsonPath().getString("gerente")).isEqualTo(novaContaBancariaRequest.getGerente());
  Assertions.assertThat(response.jsonPath().getString("banco")).isEqualTo(novaContaBancariaRequest.getBanco());
  Assertions.assertThat(response.jsonPath().getString("observacao")).isEqualTo(novaContaBancariaRequest.getObservacao());

  Assertions
    .assertThat(contaBancariaRepository.findAll())
    .hasSize(1);
}
```

O caso de teste é bem simples e bastante parecido com um teste unitário, onde seguimos o padrão dos três AAA´s: Arrange, Act, Asserts. A primeira parte:

- ```1 - ARRANGE```, é onde configuramos tudo que precisamos para fazer a requisição real. Nesse caso eu crei um objeto de ```ContaBancariaRequest``` e populei ele com os dados que desejo cadastrar.

- ```2 - ACT```, bom agora sim fazemos a requisição a nossa api e aqui uso o ```RestAssured```, que além de fazer as requisições também nos fornece uma interface fluente para extrair algumas informações.

- ```3 - ASSERTS```, requisição feita, resposta obtida é hora das asserções e aqui uso a lib AssertJ que tem uma (lidississima rsr) interface fluênte para fazermos nossos asserts. No final eu ainda faço um ```select``` para garantir que quando chamamos a api de cadastro de contas, apenas um conta seja cadastrada por vez!

Pronto, agora é só executar os testes. Tudo funcionando direitinho. O que vai acontecer é: o TestContainer irá subir um container docker do postgresql, nosso teste será executado fazendo uma interação real com esse banco de dados e assim que todos os casos de testes finalizar o container será destruido.

## Agora é sua vez

Tudo pronto, agora é sua vez, tente criar um caso de teste para o cenário de erro quando já tiver uma conta cadastrar e tentamos cadastrar a mesma conta. Tanta ai, mas se quiser saber o resultado, veja o repositório no Git Hub pois lá o teste estará completo.

Bom é isso ai, espero que tenha curtido e se vemos no próximo post onde teremos que criar uma extension do JUnit para usarmos nos nossos teste. Não perca!

Compartilhe, dê uma estrela la no GitHub e até a próxima.

