# 백기선의 스프링 프레임워크 입문
- 인프런 [백기선의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/) 강의를 듣고 요약 정리

# 수업 목표
- 실제 코드를 보며 스프링 프레임워크에 대해 소개.
- 스프링 프레임워크가 개발자에게 주는 가치를 이해.
- 짧은 시간 안에 간략하게 이해하는 것을 목표.


# 학습 주제
- IoC, AOP, PSA

## 프로젝트 세팅
- JDK 1.8 사용
- [Petclinic](https://github.com/spring-projects/spring-petclinic) 프로젝트 clone -> intellij project import
- 실행 방법
    - 1.mvn spirng-boot:run
    - 2.IDE에서 메인 애플리케이션 실행
        - 서버사이드 쪽 코드를 수정하면 적용됨
        - But. 프론트엔드 쪽 코드를 수정하면 위의 1번 방식으로 실행하던지 아니면 wro4j:run을 실행한 뒤 main 애플레이케이션을 실행하면 반영됨
## IoC(Inversion of Control)

### 1. Inversion of Control
- 제어가 뒤 바꼇다고? 뭔 소리야?
- 여기서 말하는 제어의 대상은 보통 `의존성`

#### "내가 쓸 놈은 내가 만들어 쓸께..." (일반적인 의존성에 대한 제어권 - 자기 자신)

```java
class OwnerController {
    private OwnerRepository repository = new OwnerRepository(); 
}
```

#### “내가 쓸 놈은 이 놈인데... 누군가 알아서 주겠지...” (IoC)

```java
class OwnerController {
    private OwnerRepository repo;
    public OwnerController(OwnerRepository repo) {
        this.repo = repo; 
    }
}
```

```java
class OwnerControllerTest {
    @Test public void create() {
        OwnerRepository repo = new OwnerRepository();
        OwnerController controller = new OwnerController(repo); // 의존성을 주입. 
    } 
}
```

### 2. IoC (Inversion of Control) 컨테이너
- 스프링 : IoC 컨테이너 제공
- 컨테이너의 가장 핵심적인 인터페이스는 ApplicationContext (BeanFactory)
    - ApplicationContext : IoC 컨테이너라고 불림.

#### 빈(bean)을 만들고 (의존성)엮어주며 제공해준다.
- IoC 컨테이너 내부에서 OwnerController(위의 예제)의 객체를 생성해준다.
- 아이러니하게도 컨테이너를 직접 쓸 일은 많지 않다.

### 3. 빈(Bean)
- 스프링 IoC 컨테이너가 관리하는 객체
- 어떻게 등록하지?
    - Component Scanning
    - @Component
        - @Repository
        - @Service
        - @Controller
    - 또는 직접 일일히 XML이나 자바 설정 파일에 등록
- 어떻게 꺼내쓰지?
    - @Autowired 또는 @Inject
    - 또는 ApplicationContext에서 getBean()으로 직접 꺼내거나
- 특징
- 오로지 “빈"들만 의존성 주입을 해준다.

### 4. 의존성 주입(Dependency Injection)
- 필요한 의존성을 어떻게 받아올 것인가?
    - @Autowired / @Inject 
    - 생성자
- @Autowired / @Inject를 어디에 붙일까?
    - 생성자
        - 단일 생성자의 경우 @Autowired 붙이지 않아도됨.(스프링 4.3 이후 부터)
        - 어떤 빈이 되는 클래스에 생성자가 오로지 하나만 있고 그 생성자의 매개변수 타입이 빈으로 등록되어 있다면 그 빈을 주입해준다. Autowired/Inject가 없더라도.
    - 필드
    - Setter
        - Setter가 있다면 Setter에 붙이고 없다면 필드에 붙인다.
- 장점 : 테스트 하기 쉬워진다.
    - MockBean을 통하여 주입받아서 사용 가능해진다.

## AOP(Aspect Oriented Programming)

## 1. AOP 소개
- 흩어진 코드를 한 곳으로 모아
- 흩어진 AAAA 와 BBBB

```java
class A {
    method a () {
        AAAA 
        오늘은 7월 4일 미국 독립 기념일이래요. 
        BBBB 
    }

    method b () {
        AAAA 
        저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다. 
        BBBB 
    } 
}

class B {
    method c() {
         AAAA 
         점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요. BBBB 
    } 
}
```

- 모아 놓은 AAAA 와 BBBB

```java
class A {
    method a () {
        오늘은 7월 4일 미국 독립 기념일이래요.
    }

    method b () {
        저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다. 
    } 
}

class B {
    method c() {
        점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요. 
    } 
}

class AAAABBBB {
    method aaaabbb(JoinPoint point) {
        AAAA 
        point.execute()
        BBBB
    } 
}
```

### 2. AOP 적용 예제
- @LogExecutionTime 으로 메소드 처리 시간 로깅하기

#### @LogExecutionTime 애노테이션 (어디에 적용할지 표시 해두는 용도)

```java
@Target(ElementType.METHOD) 
@Retention(RetentionPolicy.RUNTIME) 
public @interface LogExecutionTime {
}
```

#### 실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)

```java
@Component
@Aspect 
public class LogAspect {

    Logger logger = LoggerFactory.getLogger(LogAspect.class);

    @Around("@annotation(LogExecutionTime)") 
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        StopWatch stopWatch = new StopWatch(); 
        stopWatch.start();
        
        Object proceed = joinPoint.proceed();
        
        stopWatch.stop(); 
        logger.info(stopWatch.prettyPrint());
        return proceed; 
    }
}
```

## PSA(Portable Service Abstraction)
- 잘 만든 인터페이스(이식 가능한(바꿔 끼기 쉬운) 서비스 추상화)
- JDBC를 쓰다가 Hibernate로 바꿔도 코드의 변화가 없어야 한다.
- 스프링 프레임웍이 제공하는 API의 90%는 추상화

### 1. 예 : 스프링 트랜잭션
- AOP의 예제가 되기도 하지만 PSA의 예제도 되는 스프링 트랜잭션
- @Transactional
- PlatformTransactionManager(interface)
    - JpaTransacionManager
    - DatasourceTransactionManager
    - HibernateTransactionManager (구현체)
- PlatformTransactionManager 인터페이스를 이용해서 코딩을 하기 때문에 Bean(Jpa->Datasource)이 바껴도 트랜잭션을 처리하는 코드는 바뀌지 않는다.

### 2. 예 : 캐시
- @Cacheable, @CacheEvict, ...
- CacheManager (interface)
    - JCacheManager
    - ConcurrentMapCacheManager 
    - EhCacheCacheManager (구현체)

### 3. 예 : 스프링 웹 MVC
- @Controller, @RequestMapping
    - @Controller, @RequestMapping만 쓰면 Servlet을 사용하는지 Reactive를 사용하는지 알 수 없다.
        - 따라서, 추상화. 즉, 둘 간의 전환이 가능함. -> PSA 가장 큰 장점이자 목표.
