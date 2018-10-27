carmais
  |--  pom.xml

  |--  carmais-web
            |-- pom.xml

  |--  carmais-service
            |-- pom.xml

  |--  carmais-domain
            |-- pom.xml


<?xml version="1.0" encoding="UTF-8"?>
<beans
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_2_0.xsd"
	version="2.0" bean-discovery-mode="all">
</beans>


	<properties>

    <!-- Java Version -->
		<java.version>1.8</java.version>

		<!-- Encolding UTF-8 -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- Maven Compiler -->
		<maven-compiler-version>3.5.1</maven-compiler-version>

		<!-- Commons Lang3 -->
		<commons.lang.version>3.8</commons.lang.version>
		
		<!-- CDI -->
		<cdi.version>2.0</cdi.version>

		<!-- Java EE -->
		<javaee-api>7.0</javaee-api>

		<!-- JSTL -->
		<jstl.version>1.2</jstl.version>

		<!-- JSF -->
		<javax.faces.version>2.4.0</javax.faces.version>

		<!-- PrimeFaces -->
		<primefaces.version>6.0</primefaces.version>

	</properties>

  		<!-- Java EE -->
		<dependency>
			<groupId>javax</groupId>
			<artifactId>javaee-api</artifactId>
			<version>7.0</version>
			<scope>compile</scope>
		</dependency>
		
		<!-- JSF -->
		<dependency>
			<groupId>org.glassfish</groupId>
			<artifactId>javax.faces</artifactId>
			<version>2.3.3</version>
			<scope>provided</scope>
		</dependency>
		
		<!-- PrimeFaces -->
		<dependency>  
		    <groupId>org.primefaces</groupId>  
		    <artifactId>primefaces</artifactId>  
		    <version>${primefaces.version}</version>  
		    <scope>compile</scope>
		</dependency>  
		
		<!-- JSTL -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
			<scope>compile</scope>
		</dependency>

    !-- Commons Lang3 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>${commons.lang.version}</version>
		</dependency>
		
		<!-- CDI -->
		<dependency>
		    <groupId>javax.enterprise</groupId>
		    <artifactId>cdi-api</artifactId>
		    <version>${cdi.version}</version>
		    <scope>provided</scope>
		</dependency>

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
