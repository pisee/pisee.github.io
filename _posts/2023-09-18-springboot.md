---
title: springboot
permalink: /springboot/
---

# Hello-springboot 실습

---

## Spring Petclinic

<span style = "font-size: 20px">

- Spring 에서 제공하는 example 프로젝트인 petclinic의 일부기능을 직접 구현해 봅시다.
- petclinic 에 대한 자세한 내용은 아래 링크를 참고하세요
    - [Spring Petclinic Live Demo](https://spring-petclinic.github.io)
    - [Spring Petclinic Rest Github](https://github.com/spring-petclinic/spring-petclinic-rest)

---

## 프로젝트 소개

<span style = "font-size: 20px">

- petclinic의 기능중 애완동물 주인(owner) 의 정보를 관리하는 시스템입니다.
- 테이블의 정보는 아래와 같습니다. 
```sql
CREATE TABLE IF NOT EXISTS owners (
    id         BIGINT,
    first_name VARCHAR(30) NOT NULL,
    last_name  VARCHAR(30) NOT NULL,
    address    VARCHAR(255),
    city       VARCHAR(80),
    telephone  VARCHAR(20),
    PRIMARY KEY (id)
);
```  

---

## 실습 순서

<span style = "font-size: 20px">

아래 실습순서를 통해 어플리케이션을 제작해 봅니다.
1. 프로젝트 생성
1. 기본설정
1. RestAPI 작성  
1. Documentation  
1. JPA  
1. 서비스개발  
1. 단위테스트 및 Mock  

---

## 1. 프로젝트 생성

<span style = "font-size: 20px">

- http://start.spring.io 접속
- Project: Maven
- Language: Java
- Spring Boot: 3.0.5
- Project Metadata
    - Group: `org.myproject`
    - Artifact: `hello-springboot`
    - name: `hello-springboot`
    - Description: `Hello project for Spring Boot`
    - Package Name: `org.myproject.hello`
    - packaging: jar
    - java: 17
- Dependency: Spring Web, Spring Data JPA, Vaildation, H2 Database 추가
- Generate 후 다운로드된 zip파일 압축 해제 후 IDE 에서 프로젝트 Open
- `HelloSpringbootApplication` 테스트, `HelloSpringbootApplication` 실행 정상확인.

---

![bg 90%](https://code.sdsdev.co.kr/storage/user/393/files/ad3022d7-b83e-431d-9771-8d468a0a4189)

---

## 2. 기본설정

---

### 2.1 Hello Controller 생성

<span style = "font-size: 20px">

org.myproject.hello.controller 패키지에 HelloController를 생성.
```java
package org.myproject.hello.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/")
    public String sayHello(){
        return "Hello~ Developers!";
    }

}
```

---

### 2.2 로깅 - 로그 코드작성

<span style = "font-size: 20px">

Spring Boot 의 default Log Level 확인하기
- HelloController에 Logger 를 생성하고 코드를 추가.

```java
package org.myproject.hello.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    private static final Logger log = LoggerFactory.getLogger(HelloController.class);
    
    @GetMapping("/")
    public String sayHello() {
        log.debug("hi");
        log.info("hi");
        log.warn("hi");
        log.error("hi");
        return "Hello~ Developers!";
    }
}
```
- http://localhost:8080/ 으로 접속한 후 콘솔로그 확인. Default Log Level은 ? 

---

### 2.2 로깅 - 설정

<span style = "font-size: 20px">

Log Level을 debug 로 설정해보기
- `application.properties` 파일에 아래와 같이 로그 레벨을 설정.

```properties
logging.level.root=info
logging.level.org.myproject.hello=debug
```
- 어플리케이션을 재시작한 후 http://localhost:8080/ 접속하여 콘솔로그 확인

---

### 2.3 환경설정 파일 변경

<span style = "font-size: 20px">

`application.properties` 을 `application.yml`로 변경하고 아래와 같이 설정을 수정.

```yml 
logging:
  level:
    root: info
    org.myproject.hello: debug
```

- 어플리케이션을 재시작한 후 http://localhost:8080/ 접속하여 콘솔로그 확인

---

### 2.4 DB 설정

<span style = "font-size: 20px">

H2 데이터 소스 및 H2 web console 설정 추가
- `application.yml` 파일에 아래와 같이 설정 추가

```yml
spring:
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:hello
    username: sa
    password:
  h2:
    console:
      enabled: true
      path: /h2-console
```

- 어플리케이션을 재시작한 후 http://localhost:8080/h2-console/ 로 접속 확인.

---

### 2.4  DB 설정 - 콘솔 접속확인

<span style = "font-size: 20px">

- Driver Class: org.h2.Driver
- JDBC URL: jdbc:h2:mem:hello
- User Name: sa  

<div style="text-align: center; ">
  <img src="https://code.sdsdev.co.kr/storage/user/393/files/c61196e0-4bc2-4847-b0f8-76c798aaad62" style="width: 600px; height: 400px;" />
</div> 

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/dfb8a9cd-f03c-4121-80f2-efe5d3f06f26)

---

### 2.5 DB 테이블 생성

<span style = "font-size: 20px">

resources 폴더에 schema.sql 파일을 생성.

```sql
DROP TABLE owners IF EXISTS;

CREATE TABLE IF NOT EXISTS owners (
    id         BIGINT,
    first_name VARCHAR(30) NOT NULL,
    last_name  VARCHAR(30) NOT NULL,
    address    VARCHAR(255),
    city       VARCHAR(80),
    telephone  VARCHAR(20),
    PRIMARY KEY (id)
);
```

---

### 2.6 DB Data 생성

<span style = "font-size: 20px">

resources 폴더에 data.sql 파일을 생성.

```sql
INSERT INTO owners VALUES (1, 'George', 'Franklin', '110 W. Liberty St.', 'Madison', '6085551023');
INSERT INTO owners VALUES (2, 'Betty', 'Davis', '638 Cardinal Ave.', 'Sun Prairie', '6085551749');
INSERT INTO owners VALUES (3, 'Eduardo', 'Rodriquez', '2693 Commerce St.', 'McFarland', '6085558763');
INSERT INTO owners VALUES (4, 'Harold', 'Davis', '563 Friendly St.', 'Windsor', '6085553198');
INSERT INTO owners VALUES (5, 'Peter', 'McTavish', '2387 S. Fair Way', 'Madison', '6085552765');
INSERT INTO owners VALUES (6, 'Jean', 'Coleman', '105 N. Lake St.', 'Monona', '6085552654');
INSERT INTO owners VALUES (7, 'Jeff', 'Black', '1450 Oak Blvd.', 'Monona', '6085555387');
INSERT INTO owners VALUES (8, 'Maria', 'Escobito', '345 Maple St.', 'Madison', '6085557683');
INSERT INTO owners VALUES (9, 'David', 'Schroeder', '2749 Blackhawk Trail', 'Madison', '6085559435');
INSERT INTO owners VALUES (10, 'Carlos', 'Estaban', '2335 Independence La.', 'Waunakee', '6085555487');
```

---

### 2.7 스키마 생성 설정

<span style = "font-size: 20px">

`application.yml` 파일에 아래와 같이 설정 추가
```yml
  sql:
    init:
      mode: always  # never, embedded, always
      schema-locations:
        - "classpath:schema.sql"
      data-locations:
        - "classpath:data.sql"
```
- 어플리케이션을 재시작한 후 http://localhost:8080/h2-console 접속하여 테이블, 데이터 생성확인

---

### 2.8 application.yml

<span style = "font-size: 20px">

완성된 `application.yml` 은 아래와 같다.
```yml
logging:
  level:
    root: info
    org.myproject.hello: debug

spring:
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:hello
    username: sa
    password:
  h2:
    console:
      enabled: true
      path: /h2-console
  sql:
    init:
      mode: always  # never, embedded, always
      schema-locations:
        - "classpath:schema.sql"
      data-locations:
        - "classpath:data.sql"
```

[springboot application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html)

---

## 3. RestAPI 작성  

---

### 3.1 API 설계

<span style = "font-size: 20px">

- `Owners` 리소스 접근을 위한 API 설계  
	
|순서| 기능 | REST API | Http Method | 
|:---:|---|---|:---:|
| 1 | 'id' 로 단건조회 | /api/owners/{id} | GET | 
| 2 | 'lastName' 으로 다건조회 | /api/owners/lastName/{lastName} | GET | 
| 3 | 전체조회 | /api/owners | GET | 
| 4 | 단건 추가 | /api/owners | POST | 
| 5 | 'id' 로 수정 | /api/owners/{id} | PUT | 
| 6 | 'id' 로 삭제 | /api/owners/{id} | DELET | 

---

### 3.2 Domain 생성

<span style = "font-size: 20px">

- `org.myproject.hello.domain` 패키지에 `Owner` 클래스를 생성한다.
	
```java
package org.myproject.hello.domain;

public class Owner {

    private Long id;

    private String firstName;

    private String lastName;

    private String address;

    private String city;

    private String telephone;
    
    // getter, setter, toString, equals, hashCode 메소드 생성
}
```

---

### 3.3 Controller 생성 (1)

<span style = "font-size: 20px">

- `org.myproject.hello.controller` 패키지에 `OwnerController` 클래스를 생성한다.
- 서비스 로깅을 위해 log를 선언한다.
- Spring Boot는 기본적으로 logback와 slf4j를 사용한다.
	
```java
package org.myproject.hello.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/owners")
public class OwnerController {

    private static final Logger log = LoggerFactory.getLogger(OwnerController.class); 
}
```

---

### 3.3 Controller 생성 (2)

<span style = "font-size: 20px">

1. 'id' 로 단건조회 메소드생성
```java
    @GetMapping("/{id}")
    public ResponseEntity<Owner> getOwner(@PathVariable("id") long id) {
        log.debug("getOwner: {}", id);
        return new ResponseEntity<>(new Owner(), HttpStatus.OK);
    }
```
- 어플리케이션을 재시작한 후 브라우저에서 http://localhost:8080/api/owners/1 접속한 후 콘솔로그에서 `getOwner: 1` 로그확인

---

### 3.4 조회/등록/수정/삭제를 위한 RestAPI 추가 및 개발

<span style = "font-size: 20px">

```java
    @GetMapping("/lastname/{lastName}")
    public ResponseEntity<Collection<Owner>> getOwnersList(@PathVariable("lastName") String lastName) {
        log.debug("getOwnersList: {}", lastName);
        return new ResponseEntity<>(Collections.EMPTY_LIST, HttpStatus.OK);
    }

    @GetMapping
    public ResponseEntity<Collection<Owner>> getOwners() {
        log.debug("getOwners");
        return new ResponseEntity<>(Collections.EMPTY_LIST, HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<Owner> addOwner(@RequestBody Owner owner) {
        log.debug("addOwner: {}", owner);
        return new ResponseEntity<>(new Owner(), HttpStatus.OK);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Owner> updateOwner(@PathVariable("id") long id, @RequestBody Owner owner) {
        log.debug("updateOwner: {}, owner: {}", id, owner);
        return new ResponseEntity<>(new Owner(), HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteOwner(@PathVariable("id") long id) {
        log.debug("deleteOwner: {}", id);
        return new ResponseEntity<>(HttpStatus.OK);
    }
```

---

## 4. REST API Documentation

---

### 4.1 springdoc dependency 

<span style = "font-size: 20px">

Swagger 사용을 위한 `pom.xml에` Maven Dependency 추가  
```xml
    <dependencise>
        ...
		<dependency>
			<groupId>org.springdoc</groupId>
			<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
			<version>2.1.0</version>
		</dependency>
        ...
    </dependencise>
```
※ [Springdoc 문서](https://springdoc.org/v2/#Introduction)

---

### 4.2 springdoc 설정

<span style = "font-size: 20px">

swagger ui 사용을 위한 springdoc 설정을 `application.yml` 에 추가
```yml
springdoc:
  swagger-ui:
    enabled: true
    path: /swagger-ui.html
  api-docs:
    enabled: true
    path: /api-docs
  packages-to-scan:
    - org.myproject.hello.controller
```
- 어플리케이션을 재시작한 후 브라우저에서 http://localhost:8080/swagger-ui.html 접속

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/b579e64e-cac6-4191-b396-c5cea5049f16)

---

### 4.3 Swagger Annotation 활용 - @Tag

<span style = "font-size: 20px">

- `HelloController` Class 에 `@Tag` 를 이용하여 클래스에 대한 API 문서를 작성한다.

```java
import io.swagger.v3.oas.annotations.tags.Tag;

@RestController
@Tag(name = "HelloController Name", description = "HelloController Description")
public class HelloController {
    ...
}
```  
- `HelloController` 에 이름을 받아서 인사를 출력하는 새로운 컨트롤러 메소드(API)를 추가한다.  
```java
    @GetMapping("/{name}")
    public String sayHello(@PathVariable String name) {
        return "Hello~ " + name + "!";
    }
```

---

### 4.4 Swagger Annotation 활용 - @Operation

<span style = "font-size: 20px">

- Swagger Annotation (`@Operation`, ` @ApiResponses`, `@Parameter`)을 활용하여 새로운 API의 문서를 작성한다.  
```java
    @Operation(summary = "안녕이라고 말해요", description = "Hello~ your name! 출력합니다")
    @ApiResponses({
            @ApiResponse(responseCode = "200", description = "OK",
                    content = @Content(schema = @Schema(implementation = String.class))),
            @ApiResponse(responseCode = "500", description = "INTERNAL SERVER ERROR")
    })
    @GetMapping("/{name}")
    public String sayHello(@Parameter(description = "너의 이름", required = true, example = "doni") @PathVariable String name) {
        return "Hello~ " + name + "!";
    }
```  
- 어플리케이션을 재시작한 후 브라우저에서 http://localhost:8080/swagger-ui.html 접속하여 작성된 문서를 확인한다.

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/7ccb83f9-0f20-4616-9cc0-ab5bb228e277)    

---

### 4.5 Swagger Annotation 활용 - @OpenAPIDefinition

<span style = "font-size: 20px">

- `org.myproject.hello.config` 패키지를 만들고 `SwaggerConfig` 클래스를 생성한다.
```java
@Configuration
@OpenAPIDefinition(
    info = @io.swagger.v3.oas.annotations.info.Info(
        title = "Hello Springboot Open API",
        description = "Hello Springboot application.",
        version = "v0.0.1",
        license = @io.swagger.v3.oas.annotations.info.License(
            name = "Apache 2.0",
            url = "http://www.apache.org/licenses/LICENSE-2.0"
        )
    ),
    externalDocs = @io.swagger.v3.oas.annotations.ExternalDocumentation(
        description = "Hello Springboot Hands on",
        url = "https://code.sdsdev.co.kr/pages/doni/hello-springboot"
    )
)
public class SwaggerConfig {
}
```
- 어플리케이션을 재시작한 후 브라우저에서 http://localhost:8080/swagger-ui.html 접속하여 작성된 문서를 확인한다.

---

### 4.6 Swagger 활용 - OpenAPI

<span style = "font-size: 20px">

- `OpenAPI` 객체를 생성하여 `@OpenAPIDefinition` 어노테이션의 역할을 대체할 수 있다.
- `@OpenAPIDefinition` 어노테이션의 내용을 삭제하고 아래와 같이 수정한다.
```java
@Configuration
public class SwaggerConfig {
    @Bean
    public OpenAPI helloSpringbootOpenAPI() {
        return new OpenAPI()
                .info(new Info().title("Owners Open API")
                        .description("Hello Springboot application")
                        .version("v0.0.1")
                        .license(new License().name("Apache 2.0").url("http://www.apache.org/licenses/LICENSE-2.0")))
                .externalDocs(new ExternalDocumentation()
                        .description("Hello Springboot Hands on")
                        .url("https://code.sdsdev.co.kr/pages/doni/hello-springboot"));
    }    
}
```
- 어플리케이션을 재시작한 후 브라우저에서 http://localhost:8080/swagger-ui.html 접속하여 작성된 문서를 확인한다.

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/e2d24b1e-d61c-4737-b8e2-e712bc1dffd3)    

---

### 4.7 REST API 문서 그룹핑하기

<span style = "font-size: 20px">

- `HelloController` 와 `OwnerController` 의 API를 각각 나눠서 문서화 하고 싶은경우
- `SwaggerConfig` 클래스에 `GroupedOpenApi` 객체를 생성하여 문서를 구룹핑할 수 있다.
```java
    @Bean
    public GroupedOpenApi helloApi() {
        return GroupedOpenApi.builder()
                .group("hello Api").pathsToMatch("/**")
                .pathsToExclude("/api/owners/**")
                .build();
    }

    @Bean
    public GroupedOpenApi ownersApi() {
        return GroupedOpenApi.builder()
                .group("owners Api").pathsToMatch("/api/owners/**")
                .addOpenApiCustomizer(openApi -> {
                    openApi.info(new Info().title("Owners Open API").version("v0.0.1")
                                    .description("Hello Springboot application")
                                    .license(new License().name("Apache 2.0").url("http://www.apache.org/licenses/LICENSE-2.0")))
                            .externalDocs(new ExternalDocumentation().description("Hello Springboot Hands on")
                                    .url("https://code.sdsdev.co.kr/pages/doni/hello-springboot"));
                })
                .build();
    }
```

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/14f2dff6-df96-4a5e-b222-2f68370ddffd)    

---

![bg 70%](https://code.sdsdev.co.kr/storage/user/393/files/127557d1-744a-466a-9976-3f8f4fcfbc27)    

---

### 4.8 REST API 문서 그룹핑하기 - configuration

<span style = "font-size: 20px">

- `application.yml` 에 설정을 이용하여 문서를 그룹핑할 수 있다.  
	
```yml
springdoc:
  swagger-ui:
    enabled: true
    path: /swagger-ui.html
    groups-order: desc
  api-docs:
    enabled: true
    path: /api-docs
    groups:
      enabled: true
  group-configs:
    - group: Hello API
      paths-to-match:
        - /**
      paths-to-exclude:
        - /api/owners/**
    - group: Owner API
      paths-to-match:
        - /api/owners/**
```  

---

## 5. JPA 활용

---

### 5.1 JPA, H2 DB 사용을 위한 Maven Dependency 확인

<span style = "font-size: 20px">

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
```

---

### 5.2 application.yml파일에 JPA, H2 환경 설정 값 추가  

<span style = "font-size: 20px">

```yml
spring:
  jpa:
    database: H2
    generate-ddl: true  # schema.sql 파일을 사용하여 Table을 생성하도록 설정
    hibernate:
      ddl-auto: update  # create, create-drop, none, update, validate
```

---

### 5.3 domain class 수정

<span style = "font-size: 20px">

- `Owner` 클래스에 JPA 활용을 위한 Annotation 추가
- `@Entity` : JPA entity로 지정  
- `@Table` : JAP entity와 OWNERS table과 매핑  
- `@Id` : JPA에서 해당 Object의 ID로 인식  
```java 
@Entity
@Table(name = "owners")
public class Owner {
    @Id
    protected Long id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "address")
    private String address;

    @Column(name = "city")
    private String city;

    @Column(name = "telephone")
    private String telephone;
}
```

---

### 5.4 Repository 구현

<span style = "font-size: 20px">

- `org.myproject.hello.repository` 패키지 생성  
- `OwnerRepository` 인터페이스 생성 및 `@Repository` 선언  
	
```java
import org.myproject.hello.domain.Owner;
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

import java.util.Collection;

@Repository
public interface OwnerRepository extends CrudRepository<Owner, Long> {

}
```  

---

### 5.5 JPA Query Method 활용

<span style = "font-size: 20px">

```java
@Repository
public interface OwnerRepository extends CrudRepository<Owner, Long> {
    Collection<Owner> findByLastName(String lastName);
}
```

---

### 5.6 JPA native Query 활용

<span style = "font-size: 20px">

```java
@Repository
public interface OwnerRepository extends CrudRepository<Owner, Long> {

    Collection<Owner> findByLastName(String lastName);

    @Query(value= "SELECT * FROM OWNERS WHERE LAST_NAME = ?1", nativeQuery = true)
    Collection<Owner> getOwnersUsingSql(String lastName);

    @Query(value= "SELECT * FROM OWNERS WHERE LAST_NAME = :lastName", nativeQuery = true)
    Collection<Owner> getOwnersUsingSqlWithNamedParam(@Param("lastName") String lastName);

    @Query(value= "SELECT o FROM Owner o WHERE lastName = ?1", nativeQuery = false)
    Collection<Owner> getOwnersUsingJpql(String lastName);
}
```

---

### 5.7 Repository 테스트 케이스 작성

<span style = "font-size: 20px">

- `/src/test/java` 폴더에 `org.myproject.hello.repository.OwnerRepositoryTest` 클래스 생성

```java
package org.myproject.hello.repository;

import jakarta.annotation.Resource;
import org.junit.jupiter.api.Test;
import org.myproject.hello.domain.Owner;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.*;

import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest
class OwnerRepositoryTest {

    @Resource
    OwnerRepository ownerRepository;

    @Test
    void findById() {
        Optional<Owner> owner = ownerRepository.findById(1l);
        assertTrue(owner.isPresent());
        assertEquals("George", owner.get().getFirstName());
        assertEquals("Franklin", owner.get().getLastName());
    }
}
```

---

<span style = "font-size: 20px">

- `OwnerRepository`에 직접 생성한 메소드들에 대한 테스트를 추가한다.
	
```java
    @Test
    void findByLastName() {
        Collection<Owner> owners = ownerRepository.findByLastName("Franklin");
        List<Owner> list = owners.stream().toList();
        assertEquals(1, owners.size());
        assertEquals(1l, list.get(0).getId());
        assertEquals("George", list.get(0).getFirstName());
    }
    @Test
    void getOwnersUsingSql() {
        Collection<Owner> owners = ownerRepository.getOwnersUsingSql("Franklin");
        List<Owner> list = owners.stream().toList();
        assertEquals(1, owners.size());
        assertEquals(1l, list.get(0).getId());
        assertEquals("George", list.get(0).getFirstName());
    }
    @Test
    void getOwnersUsingSqlWithNamedParam() {
        Collection<Owner> owners = ownerRepository.getOwnersUsingSqlWithNamedParam("Franklin");
        List<Owner> list = owners.stream().toList();
        assertEquals(1, owners.size());
        assertEquals(1l, list.get(0).getId());
        assertEquals("George", list.get(0).getFirstName());
    }
    @Test
    void getOwnersUsingJpql() {
        Collection<Owner> owners = ownerRepository.getOwnersUsingJpql("Franklin");
        List<Owner> list = owners.stream().toList();
        assertEquals(1, owners.size());
        assertEquals(1l, list.get(0).getId());
        assertEquals("George", list.get(0).getFirstName());
    }
```

---

### 5.8 JPA 로깅

<span style = "font-size: 20px">

Case 1. `spring.jpa.show-sql` 를 활용한 SQL 로깅

```yml
spring:
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true
```

Case 2. `logging.level` 를 활용한 SQL 로깅

```yml
logging:
  level:
    root: info
    org.myproject.hello: debug
    org.hibernate.SQL: debug
    org.hibernate.orm.jdbc.bind: trace

spring:
  jpa:
    properties:
      hibernate:
        format_sql: true
```


---

## 6. Service 개발

---

### 6.1 Service interface 생성

<span style = "font-size: 20px">

- `org.myproject.hello.service` 패키지 생성
- `OwnerService` 인터페이스 생성  
	
```java
package org.myproject.hello.service;

import org.myproject.hello.domain.Owner;

import java.util.Collection;

public interface OwnerService {
    Owner findOwnerById(Long id);

    Collection<Owner> findOwnerByLastName(String lastName);

    Collection<Owner> findAllOwners();

    Owner saveOwner(Owner owner);

    void deleteOwner(Owner owner);
}
```

---

### 6.2 ServiceImpl 클래스 구현

<span style = "font-size: 20px">

- `org.myproject.hello.service` 패키지에 `OwnerServiceimpl` 클래스 생성  

```java
@Service
public class OwnerServiceImpl implements OwnerService{
    @Resource
    private OwnerRepository ownerRepository;

    @Override
    public Owner findOwnerById(Long id) {
        return ownerRepository.findById(id).orElse(null);
    }
    @Override
    public Collection<Owner> findOwnerByLastName(String lastName) {
        return ownerRepository.findByLastName(lastName);
    }
    @Override
    public Collection<Owner> findAllOwners() {
        return (Collection<Owner>) ownerRepository.findAll();
    }
    @Override
    public Owner saveOwner(Owner owner) {
        return ownerRepository.save(owner);
    }
    @Override
    public void deleteOwner(Owner owner) {
        ownerRepository.delete(owner);
    }
}
```

---

### 6.3 Service 테스트 클래스 생성

<span style = "font-size: 20px">

- `/src/test/java` 폴더에 `org.myproject.hello.service.OwnerServiceTest` 클래스 생성

```java
@SpringBootTest
class OwnerServiceTest {
    @Resource
    OwnerService ownerService;

    @Test
    void findOwnerById() {
        Owner owner = ownerService.findOwnerById(1l);

        assertEquals("George", owner.getFirstName());
        assertEquals("Franklin", owner.getLastName());
    }
}
```

---

### 6.4 Service Mock 테스트 클래스 생성

<span style = "font-size: 20px">

- `/src/test/java` 폴더에 `org.myproject.hello.service.OwnerServiceMockTest` 클래스 생성

```java
package org.myproject.hello.service;

import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.*;

@SpringBootTest
public class OwnerServiceMockTest {
}
```

---

### 6.5 Service Mock 테스트 코드 작성

<span style = "font-size: 20px">

- ```OwnerServiceImpl``` 클래스에 DI된 `OwnerRepository` 를 모킹(Mocking) 한다.

```java
@SpringBootTest
public class OwnerServiceMockTest {
    @Resource
    OwnerService ownerService;

    @MockBean
    OwnerRepository ownerRepository;
    
    @Test
    public void findOwnerById() {
        Owner expectedOwner = new Owner();
        expectedOwner.setFirstName("Doni");
        expectedOwner.setLastName("Lee");
        when(ownerRepository.findById(1l)).thenReturn(Optional.of(expectedOwner));

        Owner owner = ownerService.findOwnerById(1l);

        assertEquals("Doni", owner.getFirstName());
        assertEquals("Lee", owner.getLastName());
        verify(ownerRepository, times(1)).findById(1L);
    }
}
```

---

## 7. Controller 개발

---

### 7.1 Controller 클래스 구현

<span style = "font-size: 20px">

- `OwnerService`를 사용하여 `OwnerController` 클래스를 구현한다.

```java
    @Resource
    private OwnerService ownerService;

    @GetMapping("/{id}")
    public ResponseEntity<Owner> getOwner(@PathVariable("id") long id) {
        log.debug("getOwner: {}", id);
        Owner owner = this.ownerService.findOwnerById(id);
        if (owner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owner, HttpStatus.OK);
    }

    @GetMapping("/lastname/{lastName}")
    public ResponseEntity<Collection<Owner>> getOwnersList(@PathVariable("lastName") String lastName) {
        log.debug("getOwnersList: {}", lastName);
        Collection<Owner> owners = this.ownerService.findOwnerByLastName(lastName);
        if (owners.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owners, HttpStatus.OK);
    }
```

---

<span style = "font-size: 20px">

```java

    @GetMapping
    public ResponseEntity<Collection<Owner>> getOwners() {
        log.debug("getOwners");
        Collection<Owner> owners = this.ownerService.findAllOwners();
        if (owners.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owners, HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<Owner> addOwner(@RequestBody Owner owner) {
        log.debug("addOwner: {}", owner);
        if (owner.getId() == null) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }
        Owner currentOwner = ownerService.findOwnerById(owner.getId());
        if (currentOwner != null) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }
        Owner resultOwner = ownerService.saveOwner(owner);
        return new ResponseEntity<>(resultOwner, HttpStatus.CREATED);
    }
```

---

<span style = "font-size: 20px">

```java

    @PutMapping("/{id}")
    public ResponseEntity<Owner> updateOwner(@PathVariable("id") long id, @RequestBody Owner owner) {
        log.debug("updateOwner: {}, owner: {}", id, owner);
        Owner currentOwner = this.ownerService.findOwnerById(id);
        if (currentOwner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        currentOwner.setAddress(owner.getAddress());
        currentOwner.setCity(owner.getCity());
        currentOwner.setFirstName(owner.getFirstName());
        currentOwner.setLastName(owner.getLastName());
        currentOwner.setTelephone(owner.getTelephone());
        Owner resultOwner = ownerService.saveOwner(currentOwner);
        return new ResponseEntity<>(resultOwner, HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteOwner(@PathVariable("id") long id) {
        log.debug("deleteOwner: {}", id);
        Owner owner = this.ownerService.findOwnerById(id);
        if (owner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        this.ownerService.deleteOwner(owner);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }
```

---

<span style = "font-size: 20px">

- `OwnerController` 클래스 전체는 다음과 같다.

<span style = "font-size: 10px">

```java
@RestController
@RequestMapping("/api/owners")
public class OwnerController {

    private static final Logger log = LoggerFactory.getLogger(OwnerController.class);

    @Resource
    private OwnerService ownerService;

    @GetMapping("/{id}")
    public ResponseEntity<Owner> getOwner(@PathVariable("id") long id) {
        log.debug("getOwner: {}", id);
        Owner owner = this.ownerService.findOwnerById(id);
        if (owner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owner, HttpStatus.OK);
    }

    @GetMapping("/lastname/{lastName}")
    public ResponseEntity<Collection<Owner>> getOwnersList(@PathVariable("lastName") String lastName) {
        log.debug("getOwnersList: {}", lastName);
        Collection<Owner> owners = this.ownerService.findOwnerByLastName(lastName);
        if (owners.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owners, HttpStatus.OK);
    }

    @GetMapping
    public ResponseEntity<Collection<Owner>> getOwners() {
        log.debug("getOwners");
        Collection<Owner> owners = this.ownerService.findAllOwners();
        if (owners.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(owners, HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<Owner> addOwner(@RequestBody Owner owner) {
        log.debug("addOwner: {}", owner);
        if (owner.getId() == null) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }
        Owner currentOwner = ownerService.findOwnerById(owner.getId());
        if (currentOwner != null) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }
        Owner resultOwner = ownerService.saveOwner(owner);
        return new ResponseEntity<>(resultOwner, HttpStatus.CREATED);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Owner> updateOwner(@PathVariable("id") long id, @RequestBody Owner owner) {
        log.debug("updateOwner: {}, owner: {}", id, owner);
        Owner currentOwner = this.ownerService.findOwnerById(id);
        if (currentOwner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        currentOwner.setAddress(owner.getAddress());
        currentOwner.setCity(owner.getCity());
        currentOwner.setFirstName(owner.getFirstName());
        currentOwner.setLastName(owner.getLastName());
        currentOwner.setTelephone(owner.getTelephone());
        Owner resultOwner = ownerService.saveOwner(currentOwner);
        return new ResponseEntity<>(resultOwner, HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteOwner(@PathVariable("id") long id) {
        log.debug("deleteOwner: {}", id);
        Owner owner = this.ownerService.findOwnerById(id);
        if (owner == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
        this.ownerService.deleteOwner(owner);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }
}
```

---

### 7.2 Controller 테스트 케이스 작성

<span style = "font-size: 20px">

- `/src/test/java` 폴더에 `org.myproject.hello.controller.OwnerControllerTest` 클래스 생성

<span style = "font-size: 15px">

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
class OwnerControllerTest {
    @Resource
    private MockMvc mockMvc;

    @Test
    public void testGetOwnerSuccess() throws Exception {
        Owner owner = new Owner();
        owner.setId(1l);
        owner.setFirstName("George");
        owner.setLastName("Franklin");
        owner.setAddress("110 W. Liberty St.");
        owner.setCity("Madison");
        owner.setTelephone("6085551023");

        mockMvc.perform(get("/api/owners/1")
                .accept(MediaType.APPLICATION_JSON_VALUE))
                .andExpect(status().isOk())
                .andExpect(content().contentType("application/json"))
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.firstName").value("George"));
    }

    @Test
    public void testGetOwnerNotFound() throws Exception {
        mockMvc.perform(get("/api/owners/-1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }
}
```
---

### 7.3 Controller Mock 테스트 클래스 생성

<span style = "font-size: 20px">

- `/src/test/java` 폴더에 `org.myproject.hello.controller.OwnerControllerMockTest` 클래스 생성

```java
package org.myproject.hello.controller;

import jakarta.annotation.Resource;
import org.myproject.hello.service.OwnerService;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.BDDMockito.given;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
class OwnerControllerMockTest {

    @Resource
    private MockMvc mockMvc;

    @MockBean
    private OwnerService ownerService;

}
```

---

### 7.4 Controller Mock 테스트 코드 작성

<span style = "font-size: 20px">

- ```OwnerController``` 클래스에 DI된 `OwnerService` 를 모킹(Mocking) 한다.

<span style = "font-size: 15px">

```java
@SpringBootTest
@AutoConfigureMockMvc
class OwnerControllerMockTest {

    @Resource
    private MockMvc mockMvc;

    @MockBean
    private OwnerService ownerService;

    @Test
    public void testGetOwnerSuccess() throws Exception {

        Owner owner = new Owner();
        owner.setId(1l);
        owner.setFirstName("Doni");
        owner.setLastName("Lee");

        when(this.ownerService.findOwnerById(1l)).thenReturn(owner);
        this.mockMvc.perform(get("/api/owners/1")
                .accept(MediaType.APPLICATION_JSON_VALUE))
                .andExpect(status().isOk())
                .andExpect(content().contentType("application/json"))
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.firstName").value("Doni"))
                .andExpect(jsonPath("$.lastName").value("Lee"));
    }

    @Test
    public void testGetOwnerNotFound() throws Exception {
        given(this.ownerService.findOwnerById(-1l)).willReturn(null);
        this.mockMvc.perform(get("/api/owners/-1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }
}
```

---

## 8 전체 기능확인

- http://localhost:8080/swagger-ui.html 접속하여 REST API 가 정상동작하는지 확인한다.
- http://localhost:8080/h2-console 접속하여 DB 테이블에 CRDU 가 정상적으로 수행되는지 확인한다.

---

<section style="height: 100%; width: 100%; display: flex;">
<section style="margin: auto;">

<h1>END</h1>

</section>
</section>
