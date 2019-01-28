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
    - 1단계 : @ComponentScan
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
    - @ConditionalOnMissingBean : 해당 Bean이 없을 때만 사용하라. : Componentscan할때 Bean이 있으니깐 AutoConfiguration할때는 해당 Bean을 등록하지 않는다.
    - @ConditionalOnMissingBean은 starter project configuration 클래스 Bean에 등록한다.
- 빈 재정의 수고 덜기 (굳이 Bean 설정하지 않고 application.properties 설정만)
    - @ConfigurationProperties(“holoman”)
    - @EnableConfigurationProperties(HolomanProperties)
    - 프로퍼티 키값 자동 완성
    
```xml
자동 완성을 위해
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
    - Discover the HTTP Port at Runtime : `ApplicationListener<ServletWebServerInitializedEvent>`
        - 웹 서버가 생성되면 이벤트 리스너(ApplicationListener)가 호출됨

- <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-web-servers.html>

### (8) 내장 웹 서버 응용 2부 : HTTPS와 HTTP2 적용하는 방법

- <https://opentutorials.org/course/228/4894>


#### HTTPS 설정하기
- 키스토어 만들기
    - <https://gist.github.com/keesun/f93f0b83d7232137283450e08a53c4fd>
    - keystore.p12 file 생성됨
- appplication.properties에 위 gist의 내용을 입력하고 start
- springboot는 기본적으로 HTTP connector 하나만 등록됨. 따라서 해당 connector에 ssl 적용됨.
- ` curl -I -k --http2 https://localhost:8080/hello` -> HTTP/1.1 200

#### HTTPS 설정하면 HTTP는 못쓰네?
- HTTP 커넥터는 (하나이기 때문에) 코딩으로 설정하기
- <https://github.com/spring-projects/spring-boot/tree/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors>

```java
public ServletWebServerFactory serverFactory() {
    TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory();
    tomcat.addAdditionalTomcatConnectors(createStandardConnector());
    return tomcat;
}

private Connector createStandardConnector() {
    Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
    connector.setPort(8080);
    return connector;
}
```

#### HTTP2 설정
- server.http2.enable
- 사용하는 서블릿 컨테이너 마다 다름.
    - 언더토우는 server.http2.enable=true 설정만 해주면 된다.
    - Tomcat 8.5는 시스템 설정을 해야되므로 사용하지말고 Tomcat 9.0.x, JDK9 사용시 따로 설정이 필요없다.
- HTTP2는 HTTPS가 기본이다. 따라서 SSL 적용이 필수다.


### (9) 톰캣 HTTP2
- JDK9와 Tomcat 9+ 추천
- 그 이하는 아래 링크 참고
    - <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-web-servers.html#howto-configure-http2-tomcat>
    
### (10) 독립적으로 실행 가능한 JAR

- <https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html>

#### “그러고 보니 JAR 파일 하나로 실행할 수 있네?”

- mvn package를 하면 실행 가능한 JAR 파일 “하나가” 생성 됨.
    - 생선된 JAR 파일 안에 의존 JAR 파일이 다 들어가있다.
- spring-maven-plugin이 해주는 일 (패키징)
- 과거 “uber” jar 를 사용했었음.(자바에는 JAR 안에 들어있는 JAR를 읽을 수 있는 표준적인 방법이 없음)
    - 모든 클래스 (의존성 및 애플리케이션)를 하나로 압축하는 방법
    - 뭐가 어디에서 온건지 알 수가 없음
        - 무슨 라이브러리를 쓰는건지.
    - 내용은 다르지만 이름이 같은 파일은 또 어떻게?
- 스프링 부트의 전략
    - 내장 JAR : 기본적으로 자바에는 내장 JAR를 로딩하는 표준적인 방법이 없음.
    - 애플리케이션 클래스와 라이브러리 위치 구분
    - org.springframework.boot.loader.jar.JarFile을 사용해서 내장 JAR를 읽는다.
    - org.springframework.boot.loader.Launcher를 사용해서 실행한다.
- MANIFEST.MF : jar 파일 unzip, META-INF 밑에 존재. 모든 JAR 파일의 시작점은 MANIFEST 밑의 Main-class (자바 스펙)
    - 스프링부트는 Main-Class를 JarLauncher를 사용하도록 패키징했음
  
## 스프링 부트 활용

### (1) 스프링 부트 활용 소개

| 스프링 부트 핵심 기능 | 각종 기술 연동 |
| --- | --- |
| - SpringApplication | - 스프링 웹 MVC |
| - 외부 설정 | - 스프링 데이터 |
| - 프로파일 | - 스프링 시큐리티 |
| - 로깅 | - REST API 클라이언트 |
| - 테스트 | - 다루지 않은 내용들 |
| - Spring-Dev-Tools | |

### (2) SpringApplication 1부
- <https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-spring-application.html#boot-features-spring-application>
- `SpringApplication.run(Application.class, args)`를 사용하면 커스터마이징 하기 어려우므로 인스턴스를 만들어서 run을 하는 방법으로 사용한다.
    - `SpringApplication app = new SpringApplication(Application.class); app.run(args);`
- 기본 로그 레벨 INFO
    - 뒤에 로깅 수업때 자세히 살펴볼 예정.
    - Run/Debug Configuration > VM options : -Ddebug 입력 또는 Program arguments : --debug 입력하면 로그 레벨 디버그로 설정됨.
- FailureAnalyzer
    - 애플리케이션 에러 메시지를 조금 더 이쁘게 출력해주는 기능
- 배너
    - banner.txt | gif | jpg | png
    - banner.txt의 위치를 resources 밑이 아닌 다른 위치에 놓고 싶으면
        - (application.properties에) spring.banner.location classpath 설정.
    - ${spring-boot.version} 등의 변수를 사용할 수 있음. (레퍼런스 참조)
    - 배너 끄는 방법 `app.setBannerMode(Banner.Mode.OFF);` 
    - Banner 클래스 구현하고 SpringApplication.setBanner()로 설정 가능. (txt 파일과 동시에 존재한다면 txt 파일이 더 우선순위가 높음.)
    
    ```java
    app.setBanner(new Banner() {
        @Override
        public void printBanner(Environment environment, Class<?> sourceClass, PrintStream out) {
            out.println("===============");
            out.println("Sungjun");
            out.println("===============");
        }
    });
    ```
    
- SpringApplicationBuilder로 빌더 패턴 사용 가능

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        new SpringApplicationBuilder()
                .sources(Application.class)
                .run(args);
    }
}
```

### (3) SpringApplication 2부
- <https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-spring-application.html#boot-features-application-events-and-listeners>
- ApplicationEvent 등록
    - ApplicationContext를 만들기 전에 사용하는 리스너는 @Bean으로 등록할 수 없다. (아래 ApplicationStartingEvent )
    
    ```java
    public class SampleListener implements ApplicationListener<ApplicationStartingEvent> {
        @Override
        public void onApplicationEvent(ApplicationStartingEvent applicationStartingEvent) {
            System.out.println("==============");
            System.out.println("Application is starting");
            System.out.println("==============");
        }
    }
    ```
    - `SpringApplication.addListeners(SampleListener());` 필요함.
- WebApplicationType 설정
    - `app.setWebapplicationType(webApplicationType.NONE)`
    - 기본적으로 Spring MVC가 존재하면 Servelt, 서블릿이 없고 WebFlux가 존재하면 REACTIVE로 돈다.
    - 서블릿 있냐 없냐를 체크하는게 첫번째 조건. 둘다 존재하면 서블릿으로 실행됨. 따라서 둘다 존재하는 상황에서 리액티브를 사용하고 싶으면 webApplicationType.REACTIVE로 설정해야 함.
- 애플리케이션 아규먼트 사용하기
    - ApplicationArguments를 빈으로 등록해 주니까 가져다 쓰면 됨.
    - Configuration의 Program arguments에 해당 : -- 옵션
    - VM options는(-D로 시작) JVM 옵션
        - JVM 옵션은 애플리케이션 아규먼트가 아니다.
        
    ```java
    @Component
    public class SampleListener {
    
        public SampleListener(ApplicationArguments arguments){
            System.out.println("foo: " +  arguments.containsOption("foo"));
            System.out.println("bar: " + arguments.containsOption("bar"));
        }
    }
    ```

- 애플리케이션 실행한 뒤 뭔가 실행하고 싶을  때
    - ApplicationRunner (추천) 또는 CommandLineRunner
    
    ```java
    @Component
    public class SampleListener implements ApplicationRunner{
    
        @Override
        public void run(ApplicationArguments args) throws Exception {
            System.out.println("foo: " + args.containsOption("foo"));
        }
    }
    ```
    
    - 순서 지정 가능 @Order (@Order(1), @Order(2), ... 1이 먼저)
    
### (4) 외부 설정 1부

- <https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config>

#### 사용할 수 있는 외부 설정
- properties
- YAML
- 환경 변수
- 커맨드 라인 아규먼트

#### 프로퍼티 우선 순위 (오버라이딩 가능)

1. 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
2. 테스트에 있는 @TestPropertySource
    - `@TestPropertySource(properties = "sungjun.name=sungjun2")`
3. @SpringBootTest 애노테이션의 properties 애트리뷰트
    - `@SpringBootTest(properties = "sungjun.name=sungjun2")`
    - 참고) test/resource application.properties는 main/resource의 application.properties를 오버라이딩 하기때문에 test properties에 없는 값을 쓰게되면 에러 발생함.
    - 따라서, 파일명이 다른 test.properties를 생성해서 테스트에서 필요한 값을 넣고 location 설정을 통해 읽어들이도록 한다. (`@TestPropertySource(locations = "classpath:/test.properties")`)
4. 커맨드 라인 아규먼트
    - `java -jar target/springinit-0.0.1-SNAPSHOT.jar --sungjun.name=sungjun`
5. SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로티) 에 들어있는 프로퍼티
6. ServletConfig 파라미터
7. ServletContext 파라미터
8. java:comp/env JNDI 애트리뷰트
9. System.getProperties() 자바 시스템 프로퍼티
10. OS 환경 변수
11. RandomValuePropertySource
12. JAR 밖에 있는 특정 프로파일용 application properties
13. JAR 안에 있는 특정 프로파일용 application properties
14. JAR 밖에 있는 application properties
15. JAR 안에 있는 application properties
    - `src/main/resources/application.properties`는 여기에 해당.
16. @PropertySource
17. 기본 프로퍼티 (SpringApplication.setDefaultProperties)


#### application.properties 자체의 우선 순위 (높은게 낮은걸 덮어 쓴다.)
- application.properties 동일한 이름의 파일을 여러군데 놓을 수 있다.

1. file:./config/
2. file:./ (프로젝트 루트 밑에 위치)
3. classpath:/config/
4. classpath:/ (src/main/resource 밑에 위치)

#### application.properties 값 설정하기
- application.properties 랜덤값 설정하기
    - ${random.*}
- application.properties 플레이스 홀더
    - name = keesun
    - fullName = ${name} baik
    
### (5) 외부 설정 2부

#### 타입-세이프 프로퍼티 @ConfigurationProperties
- 같은 Key로 시작하는 외부 설정이 있는 경우 묶어서 하나의 Bean으로 등록하는 방법. (여러 프로퍼티를 묶어서 읽어올 수 있음)
- 타입-세이프? @Value("${keesun.name}")와 같이 문자열로 사용하면 오타 등의 이유로 에러가 나는것으로 부터 안전하다. getName(), getAge()등을 사용할 수 있으므로.
- 빈으로 등록해서 다른 빈에 주입할 수 있음
    - @EnableConfigurationProperties
    - **@Component**
    - @Bean
        - 대부분 아래 예시처럼 @Component 방식을 사용. @Bean을 사용하는 경우는 거의 없다.
    - `@EnableConfigurationProperties는 자동으로 등록되어 있으므로 @ConfigurationProperties class에 @Component 어노테이션만 등록해주면 된다.`
- 아래와 같이 사용
    
```properties
- application.properties
sungjun.name = sungjun
sungjun.age=${random.int(0,100)}
sungjun.fullNmae = ${sungjun.name} gwon
```

```java
@Component
@ConfigurationProperties("sungjun")
public class SungjunProperties {

    private String name;

    private int age;

    private String fullName;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getFullName() {
        return fullName;
    }

    public void setFullName(String fullName) {
        this.fullName = fullName;
    }
}
```

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
	<optional>true</optional>
</dependency>
```

```java
@Component
public class SampleRunner implements ApplicationRunner {

    @Autowired
    SungjunProperties sungjunProperties;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("==========");
        System.out.println(sungjunProperties.getName());
        System.out.println(sungjunProperties.getAge());
        System.out.println("==========");
    }
}
```

- 융통성 있는 바인딩
    - context-path (케밥)
    - context_path (언드스코어)
    - contextPath (캐멀)
    - CONTEXTPATH
    - `sungjun.fullName = ${sungjun.name} gwon`, `sungjun.full_name = ${sungjun.name} gwon` 등 케밥, 언드, 캐멀 등 융통성있게 사용 가능.
- 프로퍼티 타입 컨버전
    - properties에 입력된 값은 문자열이지만(sungjun.age = 100) @ConfigurationProperties 클래스에서 int로 자동 변환됨. 
    - @DurationUnit (스프링부트가 제공하는 시간 정보와 관련된 특수한 컨버전 타입)
- 프로퍼티 값 검증
    - @Validated
    - JSR-303 구현체(hibernate)(@NotNull, …)
    
    ```java
    @Component
    @ConfigurationProperties("sungjun")
    @Validated
    public class SungjunProperties {
    
        @NotEmpty
        private String name;
        
        ...
    ```
    
- 메타 정보 생성
- @Value
    - SpEL 을 사용할 수 있지만…
    - 위에 있는 기능들은 전부 사용 못한다.
    
### (6) 프로파일
- @Profile 애노테이션은 어디에?
    - @Configuration
    - @Component
    
    ```java
    @Profile("prod")
    @Configuration
    public class BaseConfiguration {
    
        @Bean
        public String hello() {
            return "hello";
        }
    }    
    ```
    
- 어떤 프로파일을 활성화 할 것인가?
    - spring.profiles.active
    - `spring.profiles.active=prod`
- 어떤 프로파일을 추가할 것인가?
    - spring.profiles.include
    - 추가할 프로파일 설정.
- 프로파일용 프로퍼티
    - application-{profile}.properties
        - application-test.properties, application-prod.properties 생성 가능.
        - 프로파일용 프로퍼티가 기본 application.properties 보다 우선순위가 높기때문에 오버라이딩됨. 
        - 실행 시 `java -jar target/springboot2-0.0.1-SNAPSHOT.jar --spring.profiles.active=test`
        
### (7) 로깅 1부: 스프링 부트 기본 로거 설정

- 로깅 퍼사드 VS 로거
    - Commons Logging, SLF4j : 인터페이스
    - JUL, Log4J2, Logback(SLF4j 구현체)
- 스프링 5에 로거 관련 변경 사항
    - <https://docs.spring.io/spring/docs/5.0.0.RC3/spring-framework-reference/overview.html#overview-logging>
    - Spring-JCL (Jakarta Commons Logging)
        - Commons Logging -> SLF4j or Log4j2
    - pom.xml에 exclusion 안해도 됨.
    - 결국 스프링부트 실행시 찍히는 로그는 Logback을 사용한 것임. (Commons Logging -> SLF4j -> Logback 호출)
- 스프링 부트 로깅
    - 기본 포맷
    - –debug (일부 핵심 라이브러리(코어 : 컨테이너, 하이버네이트, 스프링부트 등)만 디버깅 모드로)
    - –trace (전부 다 디버깅 모드로)
    - 컬러 출력: spring.output.ansi.enabled
    - 파일 출력: logging.file 또는 logging.path
    - 로그 레벨 조정: logging.level.패지키 = 로그 레벨

### (8) 로깅 2부: 커스터마이징

- <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html>

#### 커스텀 로그 설정 파일 사용하기
- Logback: logback-spring.xml
    - logback.xml은 너무 일찍 로딩이 되기 때문에 Logback 추가기능(extension)을 사용할 수 없음. 
- Log4J2: log4j2-spring.xml
- JUL (비추): logging.properties
- Logback extension
    - 프로파일 <springProfile name=”프로파일”>
    - Environment 프로퍼티 <springProperty>

#### 로거를 Log4j2로 변경하기
- <https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html#howto-configure-log4j-for-logging>

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
	<exclusions>
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

### (9) 테스트

#### 시작은 일단 spring-boot-starter-test를 추가하는 것 부터
- test 스콥으로 추가.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

#### @SpringBootTest (통합테스트용)

- @RunWith(SpringRunner.class)랑 같이 써야 함.
- 빈 설정 파일은 설정을 안해주나? 알아서 찾는다. (@SpringBootApplication)
- webEnvironment
    - MOCK: mock servlet environment. 내장 톰캣 구동 안 함.
        - Default : `@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)`
        - 서블릿 컨테이너를 띄우지 않음. 서블릿을 Mocking 한 것이 뜬다.
        - DispatcherServlet에 요청을 보내는 것과 비슷하게 실험은 할 수 있는데 목업이된 서블릿에 인터렉션을 하려면 MockMvc 사용 필수.
    
    ```java
    @RunWith(SpringRunner.class)
    @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
    @AutoConfigureMockMvc
     public class SampleControllerTest {
    
        @Autowired
        MockMvc mockMvc;
    
        @Test
        public void hello() throws Exception {
            mockMvc.perform(get("/hello"))
                    .andExpect(status().isOk())
                    .andExpect(content().string("hello sungjun"))
                    .andDo(print());
        }
    
    }
    ```    
    
    - RANDON_PORT, DEFINED_PORT: 내장 톰캣 사용 함.
    
    ```java
    @RunWith(SpringRunner.class)
    @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
    public class SampleControllerTest {
    
        @Autowired
        TestRestTemplate testRestTemplate;
    
        @MockBean
        SampleService mockSampleService;
    
        @Test
        public void hello() throws Exception {
            when(mockSampleService.getName()).thenReturn("gwon");
    
            String result = testRestTemplate.getForObject("/hello", String.class);
            assertThat(result).isEqualTo("hello gwon");
        }
    
    }
    ```
    
    - NONE: 서블릿 환경 제공 안 함.

#### @MockBean
- ApplicationContext에 들어있는 빈을 Mock으로 만든 객체로 교체 함.
- 모든 @Test 마다 자동으로 리셋.

#### @WebTestClient
- Spring 5 부터 추가. async
- TestRestTemplate은 sync
    - TestRestTemplate 보다 유용하므로 사용 권장.
- dependency 추가

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class SampleControllerTest {

    @Autowired
    WebTestClient webTestClient;

    @MockBean
    SampleService mockSampleService;

    @Test
    public void hello() throws Exception {
        when(mockSampleService.getName()).thenReturn("gwon");

        webTestClient.get().uri("/hello").exchange()
                .expectStatus().isOk()
                .expectBody(String.class).isEqualTo("hello gwon");
    }
}
```

#### 슬라이스 테스트
- 레이어 별로 잘라서 테스트하고 싶을 때
    - @SpringBootTest는 모든 Bean을 등록
- @JsonTest
- @WebMvcTest
    - Controller와 같은 Web과 관련된 Bean만 등록됨. @Service, @Repository X
    
    ```java
    @RunWith(SpringRunner.class)
    @WebMvcTest(SampleController.class)
    public class MockMvcTest {
    
        @MockBean
        SampleService mockSampleService;
    
        // @WebMvcTest 사용시 MockMvc 필수
        @Autowired
        MockMvc mockMvc;
    
        @Test
        public void hello() throws Exception {
            when(mockSampleService.getName()).thenReturn("gwon");
    
            mockMvc.perform(get("/hello"))
                    .andExpect(content().string("hello gwon"));
        }
    
    }
    ```
    
- @WebFluxTest
- @DataJpaTest
    - Repository만 등록
- …

### (10) 테스트 유틸
- **OutputCapture**
    - 로그를 비롯해 콘솔에 찍히는 모든걸 캡처.
    - 중간 중간에 로그를 찍고 해당 로그가 찍혔는지 테스크 할수있음.
    
    ```java
    @RunWith(SpringRunner.class)
    @WebMvcTest(SampleController.class)
    public class MockMvcTest {
    
        @Rule
        public OutputCapture outputCapture = new OutputCapture();
    
        @MockBean
        SampleService mockSampleService;
    
        @Autowired
        MockMvc mockMvc;
    
        @Test
        public void hello() throws Exception {
            when(mockSampleService.getName()).thenReturn("gwon");
    
            mockMvc.perform(get("/hello"))
                    .andExpect(content().string("hello gwon"));
    
            assertThat(outputCapture.toString()).contains("holoman")
                    .contains("skip");
        }
    
    }

    ```
    
- TestPropertyValues
- TestRestTemplate
- ConfigFileApplicationContextInitializer

### (11) Spring-Boot Devtools

- spring-boot-devtools 의존성 추가
- 캐시 설정을 개발 환경에 맞게 변경.
- 클래스패스에 있는 파일이 변경 될 때마다 자동으로 재시작.
    - 직접 껐다 켜는거 (cold starts)보다 빠른다. 왜?
    - 릴로딩 보다는 느리다. (JRebel 같은건 아님)
    - 리스타트 하고 싶지 않은 리소스는? spring.devtools.restart.exclude
    - 리스타트 기능 끄려면? spring.devtools.restart.enabled = false
- 라이브 릴로드? 리스타트 했을 때 브라우저 자동 리프레시 하는 기능
    - 브라우저 플러그인 설치해야 함.
    - 라이브 릴로드 서버 끄려면? spring.devtools.liveload.enabled = false
- 글로벌 설정
    - ~/.spring-boot-devtools.properties (application.properties 보다 높은 우선순위 1순위)
- 리모트 애플리케이션

### (12) 스프링 웹 MVC 1부: 소개

- 스프링 웹 MVC
    - <https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#spring-web>
- 스프링 부트 MVC
    - 자동 설정으로 제공하는 여러 기본 기능 (앞으로 살펴볼 예정)
    - 자동 설정 file : spring-boot-autoconfigure > spring.factories > WebMvcAutoConfiguration class
        - 스프링 웹 MVC를 아무 설정 없이 바로 사용할 수 있었던 이유 : 자동 설정 파일이 적용되었기 때문
- 스프링 MVC 확장
    - @Configuration + WebMvcConfigurer
    
    ```java
    @Configuration
    public class WebConfig implements WebMvcConfigurer {
    }
    ```
    
- 스프링 MVC 재정의
    - @Configuration + @EnableWebMvc
    
### (13) 스프링 웹 MVC 2부: HttpMessageConverters

- <https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-config-message-converters>
- HttpMessageConverters: 스프링 프레임웍에서 제공하는 인터페이스, 스프링 MVC의 일부분
- HTTP 요청 본문을 객체로 변경하거나, 객체를 HTTP 응답 본문으로 변경할 때 사용 {“username”:”keesun”, “password”:”123”} <-> User
    - @ReuqestBody
    - @ResponseBody
- 어떤 요청을 받았는지, 어떤 응답을 보내야하는지에 따라 사용하는 HttpMessageConverter가 달라짐.
- @RestController : @ResponseBody 생략 가능


### (14) 스프링 웹 MVC 3부: ViewResolve

- ContentNegotiatiogViewResolver : viewResolver 중 하나, 들어오는 요청의 accept 헤더에 따라 응답이 달라진다.
- 스프링 부트
    - 뷰 리졸버 설정 제공
    - HttpMessageConvertersAutoConfiguration
- XML 메시지 컨버터 추가하기 (XML 컨버팅 필요시 추가해서 사용)

```xml
<dependency>
   <groupId>com.fasterxml.jackson.dataformat</groupId>
   <artifactId>jackson-dataformat-xml</artifactId>
   <version>2.9.6</version>
</dependency>
```

