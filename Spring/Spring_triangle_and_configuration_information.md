# 스프링 삼각형과 설정 정보
- [스프링 입문을 위한 자바 객체 지향의 원리와 이해](https://book.naver.com/bookdb/book_detail.nhn?bid=8920762)를 읽고 정리.


## 개요
- 스프링을 이해하는 데는 POJO(Plain Old Java Object)를 기반으로 스프링 삼각형이라는 애칭을 가진 IoC/DI, AOP, PAS라고 하는 스프링의 3대 프로그래밍 모델에 대한 이해가 필수다.
- 스프링은 거대함 속에 단순함을 가지고 있는데 바로 그 단순함이 스프링 삼각형인 것이다. 결국 스프링 프레임워크는 스프링 삼각형의 조합으로 이해할 수 있다.

## IoC/DI - 제어의 역전 / 의존성 주입
- IoC(Inversion of Control) : 제어의 역전
- DI(Dependency Injection) : 의존성 주입

### 의존성이란?
- 의사코드
    - 운전자가 자동차를 생산한다.
    - 자동차는 내부적으로 타이어를 생산한다.
- 자바로 표현

```java
new Car();
Car 객체 생성자에서 new Tire();
```

- 그리고 의존성을 단순하게 정의하면 다음과 같다.
    - 의존성은 new다.
    - new를 실행하는 Car와 Tire 사이에서 Car가 Tire에 의존한다.
- `결론적으로 전체가 부분에 의존한다고 표현할 수 있다.`
- 더 깊게 들어가면 의존하는 객체(전체)와 의존되는 객체(부분) 사이에 집합 관계(Aggregation)와 구성 관계(Composition)로 구분할 수도 있지만 지금은 그저 전체와 부분이라고 받아들이면 된다.
- `프로그래밍에서 의존관계는 new로 표현된다`를 기억하자.

### 기존 방식의(자바코드) 의존 관계

```java
public class Car{
    Tire tire;

    public Car(){
        tire = new KoreaTire();
        // tire = new AmericaTire();

        public String getTireBrand(){
            return ~~;
        }
    }
}
```

- 자동차가 타이어를 생산(new)하는 부분, 즉 의존 관계가 일어나고 있는 부분
- new KoreaTire() - 타이어 생산

### 스프링 없이 의존성 주입하기 1 - 생성자를 통한 의존성 주입
- 의사코드
    - 운전자가 타이어를 생산한다.
    - 운전자가 자동차를 생산하면서 타이어를 장착한다.
- 자바로 표현 - 생성자 인자 이용

```java
Tire tire = new KoreaTire();
Car car = new Car(tire);
```

### 주입이란?
- 주입이란 말은 외부에서라는 뜻을 내포하고 있는 단어다.
- 결국 자동차 내부에서 타이어를 생산하는 것이 아니라 외부에서 생상된 타이어를 자동차에 장착하는 작업이 주입이다.

### 결론
- 기존 방식에서라면 Car는 KoreaTire, AmericaTire에 대해 정확히 알고 있어야만 그에 해당하는 객체를 생성할 수 있었다.
- 의존성 주입을 적용할 경우 Car는 그저 Tire 인터페이스를 구현한 어떤 객체가 들어오기만 하면 정상적으로 작동하게 된다.
- 의존성 주입을 하면 확장성도 좋아지는데, 나중에 어떤 새로운 타이어 브랜드가 생겨도 각 타이어 브랜드들이 Tire 인터페이스를 구현한다면 Car.java 코드를 변경할 필요 없이 사용할 수 있기 때문이다.

### 스프링 없이 의존성 주입하기 2 - 속성을 통한 의존성 주입 (get/set)
- 의사코드
    - 운전자가 타이어를 생산한다.
    - 운전자가 자동차를 생산한다.
    - 운전자가 자동차에 타이어를 장착한다.
- 자바로 표현 - 속성 접근자 메서드 사용

```java
Tire tire = new KoreaTire();
Car car = new Car();
car.setTire(tire);
```

- 생성자를 통해 주입하는 것은 자동차를 생산(구입)할 때 한번 타이어를 장착하면 더 이상 교체할 수 없다. 현실적인 방법은 운전자가 원할 때 Car의 Tire를 교체하는 것.


### 스프링을 통한 의존성 주입 - XML 파일 사용
- 의사코드
    - 운전자가 종합 쇼핑몰에서 타이어를 구매한다.
    - 운전자가 종합 쇼핑몰에서 자동차를 구매한다.
    - 운전자가 자동차에 타이어를 장착한다.
- 자바로 표현 - `속성 메서드 사용`

```java
ApplicationContext context = new ClassPathXmlApplicationContext("expert002.xml"); //종합 쇼핑몰

Tire tire = (Tire)context.getBean("tire", Tire.class); //종합 쇼핑몰에서 tire 구매

Car car = (Car)context.getBean("car", Car.class); //종합 쇼핑몰에서 car 구매

car.setTire(tire);
```

```xml
- expert002.xml

<bean id="tire" class="expert002.KoreaTire"/>

<bean id="americaTire" class="expert002.AmericaTire"/>

<bean id="car" class="expert002.Car"/>
```

- 각 상품을 구분하기 위한 id 속성과 그 상품을 어떤 클래스를 통해 생산(인스턴스화)해야 할지 나타내는 class 속성을 함께 지정
- 스프링을 도입해서 얻는 이득? 가장 큰 이득은 자동차 타이어 브랜드를 변경할 때 그 무엇도 재컴파일/재배포하지 않아도 XML 파일만 수정하면 프로그램의 실행 결과를 바꿀 수 있다는 것이다.

### 스프링을 통한 의존성 주입 - 스프링 설정 파일(XML)에서 속성 주입
- 의사 코드 - 점점 더 현실 세계를 닮아가고 있다.
    - 운전자가 종합 쇼핑몰에서 자동차를 구매 요청한다.
    - 종합 쇼핑몰은 자동차를 생산한다.
    - 종합 쇼핑몰은 타이어를 생산한다.
    - 종합 쇼핑몰은 자동차에 타이어를 장착한다.
    - 종합 쇼핑몰은 운전자에게 자동차를 전달한다.
- 자바로 표현

```java
ApplicationContext context = new ClassPathXmlApplicationContext("expert003.xml");
Car car = context.getBean("car", Car.class);
```

```xml
- expert003.xml
<bean id="koreaTire" class="expert003.KoreaTire"/>

<bean id="americaTire" class="expert003.AmericaTire"/>

<bean id="car" class="expert003.Car">
    <property name="tire" ref="koreaTire"></property>
</bean>
```

### 스프링을 통한 의존성 주입 - @Autowired를 통한 속성 주입
- 의사 코드
    - 운전자가 종합 쇼핑몰에서 자동차를 구매 요청한다.
    - 종합 쇼핑몰은 자동차를 생산한다.
    - 종합 쇼핑몰은 타이어를 생산한다.
    - 종합 쇼핑몰은 자동차에 타이어를 장착한다.
    - 종합 쇼핑몰은 운전자에게 자동차를 전달한다.

```java
@Autowired
Tire tire;
```

- 설정자 메서드를 이용하지 않고도 종합쇼핑몰인 스프링 프레임워크가 설정 파일을 통해 설정자 메서드 대신 속성을 주입해 준다.

```xml
- expert004.xml

<context:annotation-config/>

<bean id="tire" class="expert004.KoreaTire"/>

<bean id="americaTire" class="expert004.AmericaTire"/>

<bean id="car" class="expert004.Car"/>
```

- property 태그가 왜 사라졌을까? @Autowired를 통해 car의 property를 자동으로 엮어 줄 수 있으므로(자동 의존성 주입) 생략이 가능해진 것이다.
 - @Autowired id와 type 중 type 구현에 우선순위가 있음

 ### 스프링을 통한 의존성 주입 - @Resource를 통한 속성 주입
 - 의사 코드
    - 운전자가 종합 쇼핑몰에서 자동차를 구매 요청한다.
    - 종합 쇼핑몰은 자동차를 생산한다.
    - 종합 쇼핑몰은 타이어를 생산한다.
    - 종합 쇼핑몰은 자동차에 타이어를 장착한다.
    - 종합 쇼핑몰은 운전자에게 자동차를 전달한다.

- 달라진점은 @Autowired -> @Resource

```java
@Resource
Tire tire
```

- @Autowired : 스프링의 어노테이션
- @Resource : 자바 표준 어노테이션
- @Resource의 경우 @Autowired과 반대로 type보다 id의 매칭 우선순위가 높다.

### 결론
- 의존 관계가 new라고 단순화 했지만 사실 변수에 값을 할당하는 모든 곳에 의존 관계가 생긴다.
- 즉, 대입 연산자(=)에 의해 변수에 값이 할당되는 순간에 의존이 생긴다.
- 변수가 지역 변수이건 속성이건, 할당되는 값이 리터럴이건 객체이건 의존은 발생한다.
- 의존 대상이 내부에 있을 수도 있고, 외부에 있을 수도 있다.
- DI는 외부에 있는 의존 대상을 주입하는 것을 말한다.
- 의존 대상을 구현하고 배치할 때 SOLID와 응집도는 높이고 결합도는 낮추라는 기본 원칙에 충실해야 한다. (구현과 유지보수가 수월해지기 위함)

## AOP - Aspect? 관점? 핵심 관심사? 횡단 관심사?
- AOP - Aspect-Oriented Programming의 약자 : 관점 지향 프로그래밍
- 스프링 DI가 의존성(new)에 대한 주입이라면 스프링 AOP는 로직(code) 주입이라고 할 수 있다.
- 프로그램을 작성하다 보면 이처럼 다수의 모듈에 공통적으로 나타나는 부분이 존재하는데, 바로 이것을 `횡단 관심사(cross-cutting concern)`라고 한다. ( 입출금, 이체 모듈에서 로깅, 보안, 트랜잭션 기능이 반복적으로 나타나는 것)
- `코드 = 핵심 관심사 + 횡단 관심사`
- 로직을 주입한다면 어디에 주입할 수 있을까?
    - 객체 지향에서 로직(코드)이 있는 곳은 당연히 메서드 안쪽이다.
    - 5군데 : Around(메서드 전 구역), Before(메서드 시작 전(직전)), After(메서드 종료 후(직전)), AfterRunning(메서드 정상 종료 후(직후)), AfterThrowing(메서드에서 예외가 발생하면서 종료된 후(직후))

```xml
<aop:aspectj-autoproxy/>

<bean id="myAspect" class="aop002.MyAspect"/>
<bean id="boy" class="aop002.Boy"/>
```

```java
package aop002;

@Aspect
public class MyAspect{
    @Before("execution(public void aop002.Boy.runSomething())")
    public void before(JoinPoint joinPoint){
        System.out.println("얼굴 인식 확인 : 문을 개방하라")
    }
}
```

- @Aspect : 이 클래스를 이제 AOP에서 사용하겠다는 의미.
- @Before : 대상 메서드 실행 전에 이 메서드를실행 하겠다는 의미.
- JoinPoint : @Before에서 선언된 메서드인 aop002.Boy.runSomething()을 의미 
- `@Before("execution(* runSomething())")
- <aop:aspectj-autoproxy/>는 뭘까?
    - 프록시 패턴을 이용해 횡단 관심사를 핵심 관심사에 주입하는 것이다.
    - 스프링 AOP는 프록시를사용한다.
    - 호출하는 쪽이나 호출당하는 쪽, 그 누구도 프록시의 존재를 모른다. 오직 스프링 프레임워크만 프록시의 존재를 안다.
    - <aop:aspectj-autoproxy/>는 스프링 프레임워크에게 AOP 프록시를 사용하라고 알려주는 지시자인 것이다. 게다가 auto가 붙었으니 자동으로

### 용어 정리
#### Pointcut - 자르는 지점? Aspect 적용 위치 지정자!
- @Before("execution(* runSomething())")에서 * runSomething()이 바로 Pointcut이다.
- 결국 Pointcut이라고 하는 것은 횡단 관심사를 적용할 타깃 메서드를 선택하는 지시자(메서드 선택 필터)인 것이다.
- 줄여서 `타깃 클래스의 타킷 메서드 지정자`라고 할 수 있다.
- 타킷 메서드 지정자에는 익히 잘 알려진 정규식과 AspectJ 표현식 등을 사용할 수 있다.
- [접근제한자패턴] 리턴타입패턴 [패키지&클래스패턴] 메서드이름패턴(파라미터패턴) [thorws 예외패턴]

#### JoinPoint - 연결점? 연결 가능한 지점!
- Pointcut은 JoinPoint의 부분 집합이다.
- 스프링 AOP는 메서드에만 적용 가능하다.
- Pointcut은의 후보가 되는 모든 메서드들이 JoinPoint, 즉 Aspect 적용이 가능한 지점이 된다.
- `Aspect 적용이 가능한 모든 지점`
- 광의의 JoinPoint란 Aspect 적용이 가능한 모든 지점이다.
- 협의의 JoinPoint란 호출된 객체의 메서드다.

#### Advice - 조언? 언제, 무엇을!
- Advice는 Pointcut에 적용할 로직, 즉 메서드를 의미하는데 여기에 더해 언제라는 개념까지 포함하고 있다.
- 결국 Advice란 Pointcut에 언제, 무엇을 적용할지 정의한 메서드다.

```java
@Before("execution(public void aop002.Boy.runSomething())")
public void before(JoinPoint joinPoint){
    System.out.println("얼굴 인식 확인 : 문을 개방하라")
}
```
- 에서 @Before, public void before(JoinPoint joinPoint){
    System.out.println("얼굴 인식 확인 : 문을 개방하라")
}에 해당.

#### Aspect - 관점? 측면? Advisor의 집합체!
- 여러개의 Adivce와 여러 개의 Pointcut의 결합체를 의미하는 용어
- `Aspect = Adivce들 + Pointcut들`
- 결국 Aspect는 When + Where + Whar (언제, 어디에, 무엇을)이 된다.

#### Advisor - 조언자? 어디서, 언제, 무엇을!
- `Advisor = 한 개의 Advice + 한 개의 Pointcut`
- 스프링에서만 사용하는 용어이고 버전이 올라가면서 이제는 쓰지말라고 권고하는 기능이기도 하다.
- Aspect로 인해 사용할 필요가 없어졌다.

### POJO와 XML 기반의 AOP
- 어노테이션 없이 XML 파일에 설정한다. (어노테이션이 없어지면 해당 java 파일은 스프링 프레임워크에 의존하지 않는 POJO가 된다.)

```xml
<aop:config>
    <aop:aspect ref="myAspect">
        <aop:before method="before" pointcut="execution(* runSomething())" />
    </aop:aspect>
</aop:config>
```

### 결론
- `스프링 AOP는 인터페이스(interface) 기반이다.`
- `스프링 AOP는 프록시(proxy) 기반이다.`
- `스프링 AOP는 런타임(runtime) 기반이다.` 
 
## PSA - 일관성 있는 서비스 추상화
- PSA(Portable Service Abstraction), 즉 일관성 있는 서비스 추상화다.
- 예로 JDBC.
    - `어댑터 패턴을 적용해 같은 일을 하는 다수의 기술을 공통의 인터페이스로 제어할 수 있는 한 것을 서비스 추상화라고 한다.`
- 어떤 OXM(객체와 xml 매핑) 기술을 쓰든 일관된 방식으로 코드를 작성할 수 있게 지원한다. 또한 하나의 OXM 기술에서 다른 OXM 기술로 변경할 때 큰 변화 없이 세부 기술을 교체해서 사용할 수 있게 해준다. 
- 이처럼 서비스 추상화를 해주면서 그것도 일관성 있는 방식을 제공한다고 해서 이를 PSA(일관성 있는 서비스 추상화)라고 한다. 