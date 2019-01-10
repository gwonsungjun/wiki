# 백기선의 스프링 부트
- 인프런 [백기선의 스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/) 강의를 듣고 요약 정리

# 학습 주제
- 스프링 부트 원리
    - 의존성 관리
    - 자동 설정
    - 내장 웹 서버
    - 독립적으로 실행 가능한 JAR
- 스프링 부트 활용
    - 스프링 부트 핵심 기능
        - SpringApplication
        - 외부 설정
        - 로깅
        - 테스트
        - Spring-Boot-Devtools
    - 각종 기술 연동
        - 스프링 웹 MVC
        - 스프링 데이터
        - 스프링 시큐리티
        - REST 클라이언트
- 스프링 부트 운영
    - 엔드포인트
    - 메트릭스
    - 모니터링

## 스프링 부트 시작하기 (프로젝트 생성)

### 첫 번째 방법
1. new Project : Maven Project
2. <https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#getting-started-maven-installation> 링크 10.1.1 Maven Installation에서 parent, dependencies, build에서 해당하는 부분 입력
3. Application Class 하나 만들어서 아래 코드 입력 후 실행

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

- IntelliJ 상의 Treminal 에서 `mvn package` 입력
- 웹 프로젝트가 아니기 때문에 jar 파일로 생성되는 것을 확인할 수 있음. `java -jar 명령을 통해 실행 가능`

### 두 번째 방법
- <https://start.spring.io> 접속
- maven Project, java, 2.0.3, Group, Artifact Dependencies 입력 후 Generate Project
- IDE에서 프로젝트 Open

### 세 번째 방법
- IntelliJ -> new Project -> Spring Initializr -> Default -> ... -> Finish

## 스프링 부트 프로젝트 구조
- 메이븐 기본 프로젝트 구조와 동일
    - 소스 코드 (src\main\java)
    - 소스 리소스 (src\main\resource)
    - 테스트 코드 (src\test\java)
    - 테스트 리소스 (src\test\resource)
- 메인 애플리케이션 위치 (@SpringBootApplication)
    - 기본(default) 패키지에 만드는 것을 추천
    - Why ? ComponentScan 때문
    - 만약, main/java 밑에 만들게 되면 java의 전체 패키지를 Sacn! (X)
    - 아래 그림을 예로 들면 me.sungjun(기본 패키지) 밑에 생성
    - ![springbootStructrue](/images/springbootStructrue.PNG)

## 스프링 부트 원리

### (1) 의존성 관리 이해
- `spring-boot-starter-parent`의 parent는 `spring-boot-dependencies`
- spring-boot-dependencies는 `최상위` pom : `버전` 들이 나열되어 있음
-  따라서  spring-boot-dependencies pom안에 정의되있는 것을 하나라도 쓰게 되면 직접 버전을 명시하지 않아도 사용 가능. (부모로부터 상속받은 버전을 사용하고 싶지 않으면 직접 명시 가능. 즉, 오버라이딩 가능)
- 실질적인 dependency는 spring-boot-dependencies에 있고 spring-boot-starter-parent에는 스프링부트에 최적화된 설정들이 나열되어 있다.
- 또한 인텔리제이의 가장 오른쪽 Maven Project 탭의 Dependencies 부분에서 계층 구조를 확인할 수 있다.
- dependency management 기능이 왜 좋을까?
    - 우리가 직접 관리해야할 의존성이 줄어든다 = 해야 할 일이 줄어든다.

### (2) 의존성 관리 응용

#### 버전 관리 해주는 의존성 추가
- 아래 spring-boot-starter-data-jpa 추가

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

#### 버전 관리 안해주는 의존성 추가
- <https://mvnrepository.com/>에서 검색한 modelmapper Dependency 추가

```xml
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>2.3.0</version>
</dependency>
```

- version을 명시하지 않으면 해당 라이브러리를 가져오지 못하는 것을 확인할 수 있다. (modelmapper는 spring boot에서 버전 관리를 해주지 않으므로)

#### 기존 의존성 버전 변경하기
- pom.xml에 properties 태그 추가

```xml
<properties>
    <spring.version>5.0.6.RELEASE</spring.version>
</properties>
```

### (3) 자동설정의 이해 (@EnableAutoConfiguration)
- @EnableAutoConfiguration은 @SpringBootApplication안에 숨어있다.
- @SpringBootApplication은 크게 @SpringBootConfiguration, @EnableAutoConfiguration, @ComponentScan 세 가지로 구성되어 있다.
- 사실상 @Configuration, @ComponentSacn만 가지고도 Application을 실행시킬 수는 있다. But. missing ServletWebServerFactory bean.
    - WebApplication이 아닌 애플리케이션 타입을 설정하고 실행은 가능함.
- 빈은 사실 두 단계로 나눠서 읽힌다.
    - 1단계 : @ComponentScann
    - 2단계 : @EnableAutoConfiguration
    -  ComponentSacn으로 빈을 다 등록 한 뒤 EnableAutoConfiguration으로 추가적인 빈들을 읽어 들임.
- `@ComponentScan`
    - @Component라는 애노테이션을 가진 클래스들을 스캔해서 빈으로 등록
    - @Component이 붙은 자기 클래스부터 시작해서 하위 패키지 까지 해당 애너테이션 (@Configuration @Repository @Service @Controller @RestController)이 붙어있는 클래스들을 빈으로 등록 
- `@EnableAutoConfiguration`
    - spring-boot-autoconfigure 프로젝트안에 META-INF 아래 spring.factories라는 파일이 있다.
    - ![spring-autoconfigure](/images/spring-autoconfigure.png)
    - spring.factories 안의 목록들은 전부 @Configuration Annotation이 설정되어있음
    - spring.factories를 찾고 그 안의 키값에 해당하는 모든 클래스를 보고 condition(조건)에 맞으면(@ConditionalOn, @ConditionalOnMissing ...) bean 등록
- `정리하면 spring.factories안에 들어있는 수많은 자동 설정들이 조건에 따라 적용이돼서 수많은 빈들이 생성이되고 그랬기 때문에 WebApplication(내장 톰을 사용해서)이 구동 되는 것이다.`

### (4) 자동 설정 만들기 1부 : Starter와 AutoConfigure

- xxx-Spring-Boot-AutoConfigure 모듈 : 자동 설정
- xxx-Spring-Boot-Starter : 필요한 의존성 정의
- 서로 다른 프로젝트 이지만 그냥 하나로 만들고 싶을 때는?
    - xxx-Spring-Boot-Starter로 생성(자동 생성도 Starter에 넣음)

#### 구현 방법
1. 새 프로젝트 생성 (Maven Project)
2. 의존성 추가

```xml
<dependencies>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-autoconfigure</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-autoconfigure-processor</artifactId>
      <optional>true</optional>
  </dependency>
</dependencies>
 
<dependencyManagement>
  <dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-dependencies</artifactId>
          <version>2.0.3.RELEASE</version>
          <type>pom</type>
          <scope>import</scope>
      </dependency>
  </dependencies>
</dependencyManagement>
```

3. @Configuration 파일 작성
4. src/main/resource/META-INF에 spring.factories 파일 만들기
5. spring.factories 안에 자동 설정 파일 추가

```yml
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
 me.sungjun.HolomanConfiguration
```

6. mvn install 
- 빌드해서 생성된 jar 파일을 다른 메이븐 프로젝트에서도 가져다 쓸 수 있도록 로컬 메이븐 저장소에 설치한다.

7. 이전 프로젝트로 가서 dependency 추가 후 확인

```xml
<dependency>
    <groupId>me.sungjun</groupId>
    <artifactId>jun-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

8. 덮어쓰기 문제가 발생한다. AutoConfiguration으로 설정한 클래스 파일을 @Bean으로 재정의 하지 못 한다.

### (5) 자동 설정 만들기 2부 : Starter와 AutoConfigure

- 덮어쓰기 방지하기
    - @ConditionalOnMissingBean : 해당 Bean이 없을 때만 사용하라. : componentsacn할때 Bean이 있으니깐 AutoConfiguration할때는 해당 Bean을 등록하지 않는다.
    - starter project configuration 클래스 Bean에 등록한다.
- 빈 재정의 수고 덜기 (굳이 Bean 설정하지 않고 application.properties 설정만)
    - @ConfigurationProperties(“holoman”)
    - @EnableConfigurationProperties(HolomanProperties)
    - 프로퍼티 키값 자동 완성
    
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

### (6) 내장 웹 서버 이해

- 스프링 부트는 서버가 아니다.
    - 톰캣 객체 생성
    - 포트 설정
    - 톰캣에 컨텍스트 추가
    - 서블릿 만들기
    - 톰캣에 서블릿 추가
    - 컨텍스트에 서블릿 맵핑
    - 톰캣 실행 및 대기
- 이 모든 과정을 보다 상세히 또 유연하고 설정하고 실행해주는게 바로 스프링 부트의 자동 설정.
    - ServletWebServerFactoryAutoConfiguration (서블릿 웹 서버 생성) : 서블릿 컨테이너 만듬.
        - TomcatServletWebServerFactoryCustomizer (서버 커스터마이징)
    - DispatcherServletAutoConfiguration : Dispatcher 서블릿을 만들어서 등록. (둘로 나눠져 있는 이유는 서블릿 컨테이너는 pom.xml 설정에 따라 달라질 수 있지만 서블릿은 변하지 않으므로)
        - 서블릿 만들고 등록
        
### (7) 내장 웹 서버 응용 1부 : 컨테이너와 포트

- 기본적으로 서블릿 기반 web mvc 개발 시 톰캣을 사용하게 된다.(spring-boot-starter-web 자동 설정)

- 다른 서블릿 컨테이너로 변경
    - 의존성 설정
    
    ```xml
    <dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-web</artifactId>
    	<exclusions>
    		<!-- Exclude the Tomcat dependency -->
    		<exclusion>
    			<groupId>org.springframework.boot</groupId>
    			<artifactId>spring-boot-starter-tomcat</artifactId>
    		</exclusion>
    	</exclusions>
    </dependency>
    <!-- Use Jetty instead -->
    <dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
    ```
    
    - 웹 서버 사용 하지 않기 (의존성이 있다 하더라도 무시하고 none web application으로 실행하고 끝난다.)
        - application.properties 추가 : `spring.main.web-application-type=none`
    - 포트
        - application.properties 추가 : `server.port=7070`
    - 랜덤 포트
        - application.properties 추가 : `server.port=0`
    - Discover the HTTP Port at Runtime : ApplicationListener<ServletWebServerInitializedEvent>
        - 웹 서버가 생성되면 이벤트 리스너(ApplicationListener가 호출됨

- <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-web-servers.html>

### (8) 내장 웹 서버 응용 2부 : HTTPS와 HTTP2 적용하는 방법

- https://opentutorials.org/course/228/4894


#### HTTPS 설정하기
- 키스토어 만들기
    - <https://gist.github.com/keesun/f93f0b83d7232137283450e08a53c4fd\>
    - keystore.p12 file 생성됨
- appplication.properties에 위 gist의 내용을 입력하고 start
- springboot는 기본적으로 HTTP connector 하나만 등록됨. 따라서 해당 connector에 ssl 적용됨.
- ` curl -I -k --http2 https://localhost:8080/hello` -> HTTP/1.1 200

#### HTTPS 설정하면 HTTP는 못쓰네?
- HTTP 커넥터는 코딩으로 설정하기
- <https://github.com/spring-projects/spring-boot/tree/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors>

#### HTTP2 설정
- server.http2.enable
- 사용하는 서블릿 컨테이너 마다 다름.