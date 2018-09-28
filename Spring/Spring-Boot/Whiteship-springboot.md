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