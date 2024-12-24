## Sobre
Tutorial sobre como habilitar múltiplos servidores de aplicação num projeto Spring Boot.

## Requisitos
- JDK 17
- Maven 3.8.x
- Spring Tool Suite

## Passo a passo

### Criando o projeto
- Abrir o Spring Tool Suite

- Criar um projeto **Spring Starter Project** utilizando os seguintes dados:
```
  Name: spring-boot-embeddedservers
  Type: Maven Project
  Packaging: jar
  Java Version: 17
  Language: Java
  Group: com.ctseducare
  Artifact: embeddedservers
  Version: 1.0.0
```

- Adicionar as seguintes dependencias:
```
  Spring Web
```

- Criar o pacote **com.ctseducare.embeddedservers.controller**

- Criar a classe **EmbeddedServerController** com o conteúdo abaixo
```
@RestController
public class EmbeddedServerController {
  @GetMapping("/")
  String status() {
    return "I am up!";
  }
}
```

- Testando a aplicação
```
http://localhost:8080
```

### Substituindo o Tomcat pelo Jetty
- Remover a dependência do Tomcat do starter Web
```
...
<dependency>
  <groupid>org.springframework.boot</groupid>
  <artifactid>spring-boot-starter-web</artifactid>
  <exclusions>
    <exclusion>
      <groupid>org.springframework.boot</groupid>
      <artifactid>spring-boot-starter-tomcat</artifactid>
    </exclusion>
  </exclusions>
</dependency>
...
```    

- Adicionar a dependência do Jetty
```
...
<dependency>
  <groupid>org.springframework.boot</groupid>
  <artifactid>spring-boot-starter-jetty</artifactid>
</dependency>
...
```

Testando a aplicação
```
http://localhost:8080
```

### Gerenciando mútiplos servidores
- Remover a dependência **spring-boot-starter-web**

- Criar um profile para o Tomcat
```
  ...
  <profiles>
    <!--TOMCAT-->
    <profile>
      <id>tomcat</id>
      <build>
        <finalname>SpringBootTomcat</finalname>
      </build>
      <activation>
        <activebydefault>true</activebydefault>
      </activation>
      <dependencies>
        <dependency>
          <groupid>org.springframework.boot</groupid>
          <artifactid>spring-boot-starter-web</artifactid>
        </dependency>
      </dependencies>
    </profile>
  </profiles>
  ...
```
  
- Criar um profile para o Jetty
```
    ...
    <!--JETTY-->
    <profile>
      <id>jetty</id>
      <build>
        <finalname>SpringBootJetty</finalname>
      </build>      
      <dependencies>
        <dependency>
          <groupid>org.springframework.boot</groupid>
          <artifactid>spring-boot-starter-web</artifactid>
          <exclusions>
            <exclusion>
              <groupid>org.springframework.boot</groupid>
              <artifactid>spring-boot-starter-tomcat</artifactid>
            </exclusion>
          </exclusions>
        </dependency>
        <dependency>
          <groupid>org.springframework.boot</groupid>
          <artifactid>spring-boot-starter-jetty</artifactid>
        </dependency>
      </dependencies>
    </profile>
  ...
```

- Criar um profile para o Undertown
```
  ...
    <!--UNDERTOW-->
    <profile>
      <id>undertow</id>
      <build>
        <finalname>SpringBootUndertow</finalname>
      </build>      
      <dependencies>
        <dependency>
          <groupid>org.springframework.boot</groupid>
          <artifactid>spring-boot-starter-web</artifactid>
          <exclusions>
            <exclusion>
              <groupid>org.springframework.boot</groupid>
              <artifactid>spring-boot-starter-tomcat</artifactid>
            </exclusion>
          </exclusions>
        </dependency>
        <dependency>
          <groupid>org.springframework.boot</groupid>
          <artifactid>spring-boot-starter-undertow</artifactid>
        </dependency>
      </dependencies>
    </profile>
  ...
```

### Criando launchers
- Selecionar o menu **Run** > **Run configurations...**

- Selecionar o item **Maven Build**, botão direito selecionar **New Configuration**

  - Definindo um launcher para empacotar a aplicação

    - Preencher os campos abaixo:

      Name: spring-boot-embeddedservers-tomcat-PACK
      Base directory: ${workspace_loc:/spring-boot-embeddedservers}
      Goals: package
      profile: tomcat

  - Definindo um launcher para executar a aplicação

    - Preencher os campos abaixo:

      Name: spring-boot-embeddedservers-tomcat-RUN
      Base directory: ${workspace_loc:/spring-boot-embeddedservers}
      Goals: spring-boot:run
      profile: tomcat
      
### Executando pelo Maven
```
mvn -P tomcat spring-boot:run
mvn -P jetty spring-boot:run
mvn -P undertow spring-boot:run
```

## Licença
Este projeto está sob licença do MIT. Para mais detalhes, ver o arquivo LICENSE.
