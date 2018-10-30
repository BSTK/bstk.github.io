---
layout: post
title:  "Carmais - Criando o projeto"
date:   2018-10-30 18:00:00 +0530
---

Chegou a hora de colocar a mão no código e começar a dar vida ao nosso web app **Carmais**.
Neste vídeo é abordada toda a parte de configuração incial do projeto bem como a definição de algumas das dependências (maven) principais. 


Como mexemos em muitos arquivos de configuração, abaixo vou listar todos eles bem como seu conteúdo

## web.xml

Localizado na pasta **webapp/WEB-INF**, o arquivo web.xml (também conhecido como **Deployment Descriptor Elements**)
serve para descrever como implantar nossa aplicação em um web container (que no nosso caso é o Tomcat) como Tomcat, Jetty e etc.
O mínimo que precisamos no **web.xml** e uma tag de abertura e fechamento `<web-app> ... </web-app>`.

Para nossa configuração, o **web.xml** ficou dessa forma :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">	
	
	<!-- Faces Servlet -->
    <servlet>
		<servlet-name>Faces Servlet</servlet-name>
		<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	
	<!-- Mapemento do Faces Servlet -->
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.xhtml</url-pattern>
	</servlet-mapping>
	
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.jsf</url-pattern>
	</servlet-mapping>
	
	<!-- Configuração do CDI no Tomcat -->
	<listener>
   		<listener-class>org.jboss.weld.environment.servlet.Listener</listener-class>
	</listener>

	<!-- Configuração do CDI no Tomcat -->
	<resource-env-ref>
   		<resource-env-ref-name>BeanManager</resource-env-ref-name>
   		<resource-env-ref-type>javax.enterprise.inject.spi.BeanManager</resource-env-ref-type>
	</resource-env-ref>
	
</web-app>
```

## faces-config.xml

Também localizado na pasta **webapp/WEB-INF** o arquivo **faces-config.xml** é necessário para algumas configurações especificas do JSF, como por exemplo : propriedades de localização, regras de navegação, novos renders e etc.


Para nossa configuração, o **faces-config.xml** ficou dessa forma :

```xml
<?xml version="1.0"?>
<faces-config version="2.2" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-facesconfig_2_2.xsd">
</faces-config>
```

## context.xml

Localizado na pasta **webapp/META-INF**, usamos este apenas para fazer com que o Weld (implementação do CDI) funcione corretamente no Tomcat.


Para nossa configuração, o **context.xml** ficou dessa forma :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <Manager pathname=""/>
    <Resource name="BeanManager" auth="Container" 
        type="javax.enterprise.inject.spi.BeanManager" 
        factory="org.jboss.weld.resources.ManagerObjectFactory"/>
</Context>
```

## beans.xml

Localizado na paste **resources/META-INF**, este arquivo serve para habilitar o CDI (Contexto e Injeção de Dependência) quando estamos utilizando Java EE. É apenas uma arquivo de marcação, por isso seu conteúdo etsá vazio.


Para nossa configuração, o **beans.xml** ficou dessa forma :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
       version="1.1" bean-discovery-mode="all">      
</beans>
```

## pom.xml

O **pom.xml** (Project Object Model - Modelo de Objeto do Projeto) está localizado na raiz do nosso projeto e será utlizado pelo maven para construção do projeto. É nele que listamos as bibliotecas de terceiros (conhecido como dependências) e alguns plugins que ajudam no processo de compilação.  


Para nossa configuração, o **pom.xml** ficou dessa forma :

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	
	<modelVersion>4.0.0</modelVersion>
	<groupId>carmais</groupId>
	<artifactId>carmais</artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<packaging>war</packaging>
	
	<properties>
		
		<!-- Java Version -->
		<java.version>1.8</java.version>

		<!-- Encolding UTF-8 -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- Maven Compiler -->
		<maven-compiler-version>3.5.1</maven-compiler-version>
	
		<!-- Java EE -->
		<javaee-api.version>7.0</javaee-api.version>
		
		<!-- JSF -->
		<javax.faces.version>2.3.0</javax.faces.version>
		
		<!-- Weld (Implementação do CDI) -->
		<weld.version>2.4.8.Final</weld.version>
		
		<!-- JSTL -->
		<jstl.version>1.2</jstl.version>

		<!-- PrimeFaces -->
		<primefaces.version>6.0</primefaces.version>
		
		<!-- Commons Lang3 -->
		<commons.lang.version>3.8</commons.lang.version>
		
	</properties>
	
	<dependencies>
		
		<!-- Java EE -->
		<dependency>
			<groupId>javax</groupId>
			<artifactId>javaee-web-api</artifactId>
			<version>${javaee-api.version}</version>
			<scope>compile</scope>
		</dependency>
		
		<!-- JSF -->		
		<dependency>
			<groupId>org.glassfish</groupId>
			<artifactId>javax.faces</artifactId>
			<version>${javax.faces.version}</version>
			<scope>compile</scope>
		</dependency>
		
		<!-- Weld (Implementação do CDI) -->
		<dependency>
		    <groupId>org.jboss.weld.servlet</groupId>
		    <artifactId>weld-servlet</artifactId>
		    <version>${weld.version}</version>
		    <scope>compile</scope>
		</dependency>
		
		<!-- JSTL -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
			<scope>compile</scope>
		</dependency>
		
		<!-- PrimeFaces -->
		<dependency>  
		    <groupId>org.primefaces</groupId>  
		    <artifactId>primefaces</artifactId>  
		    <version>${primefaces.version}</version>  
		    <scope>compile</scope>
		</dependency>  
		
    	<!-- Commons Lang3 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>${commons.lang.version}</version>
			<scope>compile</scope>
		</dependency>
	
	</dependencies>
	
	<build>
	
		<finalName>${artifactId}</finalName>
		
		<plugins>
			
			<!-- Plugin de Compilação -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>${maven-compiler-version}</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
			
		</plugins>
	
	</build>
</project>
```

O post ficou meio longo devido ao conteúdo dos arquivos de configuração, mas está ai tudo que você precisa para acompanhar o vídeo sem ter muita dor de cabeça procurando por configurações.

## Links úties

[Repositório no Github]()

### Não deixe de acompanhar o video no canal do YouTube

[#04 - Carmais - Criando projeto]()

Até mais !

[]'s
