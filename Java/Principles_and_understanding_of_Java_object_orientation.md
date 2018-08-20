# 스프링 입문을 위한 자바 객체 지향의 원리와 이해
- [스프링 입문을 위한 자바 객체 지향의 원리와 이해](https://book.naver.com/bookdb/book_detail.nhn?bid=8920762)를 읽고 정리.
- 앞뒤 구분 없이 책을 읽으며 기억하고 싶은 내용을 쭉 적어 보았다.

## 신기술은 이전 기술의 어깨를 딛고
- 스프링을 비롯한 모든 신기술은 갑자기 하늘에서 뚝 떨어진 것이 아니다. 이전 기술의 어깨를 디딤돌 삼아 그 위에 이전 기술이 제시한 철학과 기법을 정반합의 논리로 정제하고, 이전 기술을 거름 삼아 새로운 철학과 기법을 더해 나타나는 것이다.
- 기계어 -> 어셈블리어 -> C언어(One Source Multi Object Use Anywhere) -> C++ 언어(객체지향 개념의 도입) -> 자바(진정한 객체 지향 언어, Write Once Run Anywhere)
- 위에 나열한 기술들 모두 없던 기술을 새롭게 만든 것이 아니다. 이전 기술의 어깨를 딛고 조금 더 사람들이 편하도록 발전하고 있음을 보여준다.

## 자바와 절차적/구조적 프로그래밍
- JDK : Java Development Kit / 자바 개발 도구 / JVM용 소프트웨어 개발 도구 / javac.exe 포함
- JRE : Java Runtime Environment / 자바 실행 환경 / JVM용 OS / java.exe 포함
- JVM : Java Virtual Machine / 자바 가상 기계 / 가상의 컴퓨터
- 자바 개발 도구인 JDK를 이용해 개발된 프로그램은 JRE에 의해 가상의 컴퓨터인 JVM 상에서 구동된다.
- 프로그램이 메모리를 사용하는 방식
    - 크게 코드 실행 영역과 데이터 저장 영역으로 나뉜다.
    - 객체 지향 프로그램에서는 데이터 저장 영역을 다시 세 개의 영역으로 분할해서 사용한다.
    - 스태틱(static) 영역, 스택(stack) 영역, 힙(heap) 영역

![javamemory](/images/javamemory.png)[그림 출처](http://asfirstalways.tistory.com/329)

- 구조적 프로그래밍은 함수를 써라는 것.
    - 함수를 쓰면 좋은 이유는 중복 코드를 한 곳에 모아서 관리할 수 있고, 논리를 함수 단위로 분리해서 이해하기 쉬운 코드를 작성할 수 있기 때문.
- 그럼 자바 언어에서 이러한 절차적 / 구조적 프로그래밍 유산은 어디에 남아 있을까? `메서드 내부`
- JRE는 먼저 프로그램 안에 main() 메서드가 있는지 확인. main() 메서드의 존재가 확인되면 JRE는 프로그램 실행을 위한 사전 준비에 착수한다.
    - 가상의 기계인 JVM에 전원을 넣어 부팅하는 것이다. 부팅된 JVM은 목적 파일을 받아 그 목적 파일을 실행한다. 
    - JVM이 맨 먼저 하는 일은 `전처리`라고 하는 과정이다. 
        - 모든 자바 프로그램이 반드시 포함하게 되는 패키지가 있다. `java.lang`패키지다.
        - JVM은 가장 먼저 java.lang 패키지를 메모리의 스태틱 영역에 가져다 놓는다.
        - 그 다음 import된 패키지를 메모리의 스태틱 영역에 배치
        - 프로그램 상의 모든 클래스를 메모리의 스태틱 영역에 배치
        - main() 메서드 스택 프레임 배치
        - 변수 공간 배치 등...
- main() 메서드 스택 프레임을 소멸시키는 블록 마침 기호인 닫는 중괄호가 보이면 메모리 소멸, JVM 기동 중지, JRE가 사용했던 시스템 자원을 운영체제에 반납하게 된다.
- 지역변수 : 스택 영역 (스택 프레임이 사라지면 함께 사라짐)
- 클래스 멤버 변수 : 스태틱 영역 (JVM이 종료될 때까지 고정)
- 객체 멤버 변수 : 힙 영역 (가비지 컬렉터가 회수할 때 까지)
- `외부 스택 프레임에서 내부 스택 프레임의 변수에 접근하는 것은 불가능하나 그 역은 가능하다.`
- __멀티 스레드의 메모리 모델은 스택 영역을 스레드 개수만큼 분해서 쓰는 것__
- 객체 지향 프로그래밍은 연산자, 제어문, 메모리 관리 체계 등 구조적 프로그래밍의 많은 부분을 차용하고 있다. 그리고 구조적 프로그래밍은 함수로 그 특징을 대변한다고 볼 수 있는데, 객체 지향 프로그래머들도 메서드 작성에 대한 지혜를 구조적 프로그래밍에서 배워와야 한다. 그러한 뜻에서 객체지향은 절차적 / 구조적 프로그래밍의 어깨를 딛고 만들어 졌다고 할 수 있다.
```
- 스태틱 : 클래스의 놀이터
- 스택 : 메서드의 놀이터
- 힙 : 객체의 놀이터
```

## 아래 링크를 참고

## [객체 지향 4대 특성](https://github.com/gwonsungjun/TIL/blob/master/Java/The_four_principles_of_object-oriented_Java.md)

## [객체 지향 설계 5원칙](https://github.com/gwonsungjun/TIL/blob/master/Java/SOLID.md)

## [디자인 패턴](https://github.com/gwonsungjun/TIL/blob/master/Java/designPattern.md)

## [스프링 삼각형과 설정 정보](https://github.com/gwonsungjun/TIL/blob/master/Spring/Spring_triangle_and_configuration_information.md)

## 자바 8 람다와 인터페이스 스펙 변화

### 람다가 도입된 이유
- 빅데이터 지원 -> 병렬화 강화 -> 컬렉션 강화 -> 스트림 강화 -> 람다 도입 -> 인터페이스 명세 변경 -> 함수형 인터페이스 도입

### 람다란 무엇인가?
- 람다란 한 마디로 코드 블록이다. 
- 기존 코드 블록은 반드시 메서드 내에 존재해야 했다. 그래서 코드 블록만 갖고 싶어도 기존에는 코드 블록을 위해 메서드를, 다시 메서드를 사용하기 위해 익명 객체를 만들거나 하는 식이었다.
- 하지만 자바 8부터는 코드 블록인 람다를 메서드의 인자나 반환값으로 사용할 수 있게 됐다. 

```java
public static void main(String[] args){
    Runnable r = new Runnable(){
        public void run(){
            System.out.println("Hello Lambda");
        }
    }
}
```

```java
public static void main(String[] args){
    Runnable r = () -> {
        System.out.println("Hello Lambda2");
    }
```

- new Runnable() 사라짐
- public void run() => ()으로 변경됨
- -> 추가됨
- System.out.println("Hello Lambda2"); 동일함

### 함수형 인터페이스
- 추상 메서드 하나만 갖는 인터페이스를 자바 8부터는 함수형 인터페이스라고 한다. 이런 함수형 인터페이스만을 람다식으로 변경할 수 있다.

```java
public static void main(String[] args){
    MyFunctionalInterface mfi = (int a) -> { return a * a };

    int b = mfi.runSomething(5);

    System.out.println(b);
}

@FunctionalInterface
interface MyFunctionalInterface{
    public abstract int runSomething(int count);
}
```

- (int a) -> {return a * a}; => a -> a*a로 간소화할 수 있음(타입 추정 기능을 통하여)

### 자바 8 API에서 제공하는 함수형 인터페이스
- 앞서 사용자 정의 함수형 인터페이스를 만들어 사용했지만 자바 8 API 설계자들은 개발자들이 많이 쓸 것이라고 예상되는 함수형 인터페이스를 이미 여러 패키지에서 제공하고 있다.
- Bi는 Binary(이항)의 약자.
- 아래 표에서 설명하는 함수형 인터페이스를 비롯해 java.util.function 패키지에서는 총 43개의 함수형 인터페이스를 제공
- 만약 43개 중에서 필요로 하는 함수형 인터페이스가 없다면 사용자 정의 함수형 인터페이스를 정의해서 사용하면 된다.

함수형 인터페이스 | 추상메서드 | 용도
--------------------- | -------------- | --------------
 Runnable | void run() | 실행할 수 있는 인터페이스
 Supplier<T> | T get() | 제공할 수 있는 인터페이스
 Consumer<T> | void accept(T t) | 소비할 수 있는 인터페이스
 Function<T, R> | R apply(T t) | 입력을 받아서 출력할 수 있는 인터페이스
 Predicate<T> | Boolean test(T t) | 입력을 받아 참/거짓을 단정할 수 있는 인터페이스
 UnaryOperator<T> | T apply(T t) | 단항(Unary) 연산할 수 있는 인터페이스
 BiConsumer<T, U> | void accept(T t, U u) | 이항 소비자 인터페이스
 BiFunction<T, U, R> | R apply (T t, U u) | 이항 함수 인터페이스
 BiPredicate<T, U> | Boolean test(T t, U u) | 이항 단정 인터페이스
 BinaryOperator<T, T> | T apply <T t, T t> | 이항 연산 인터페이스

 ```java
public static void main(String[] args){
    Runnable run = () -> System.out.println("Hello");
    Supplier<Integer> sup = () -> 3 * 3;
    Consumer<Integer> con = num -> System.out.println(num);
    Function<Integer, String> fun = num -> "input :" + num;
    Predicate<Integer> pre  = num -> num > 10;
    UnaryOperator<Integer> uOp = num -> num * num;
    BiConsumer<String, Integer> bCon = (str, num) -> System.out.println(str + num);
    BiFunction<Integer, String, Integer> bFun = (num1, num2) -> "add result: " + (num1+num2);
    BiPredicate<Integer, Integer> bPre = (num1, num2) -> num1 > num2 ;
    BinaryOperator bOp = (num1, num2) -> num1 - num2;
}
 ```

### 컬렉션 스트림에서 람다 사용

```java
public static void main(String[] args){
    Integer[] ages = { 20, 25, 18, 27, 30, 21, 17, 19, 34, 28};

    for(int i =0; i < ages.length; i++){
        if (ages[i] < 20){
            System.out.println("Age %d!!! Can't enter \n", ages[i]);
        }
    }
}
```

```java
public static void main(String[] args){
    Integer[] ages = { 20, 25, 18, 27, 30, 21, 17, 19, 34, 28};

    Arrays.stream(ages)
    .filter(age -> age <20)
    .forEach(age ->  System.out.println("Age %d!!! Can't enter \n", ages[i]));
}
```

- 가장 좋아진 점은 How가 아닌 What만을 지정했다는 것.
- 함수형 프로그래밍의 장점인 선언적 프로그래밍을 활용하는 것. 이는 SQL 구문과 유사하다. SQL을 작성할 때 '어떻게 하라'를 명령하는 것이 아니고 '무엇을 원한다'라고 선언하는 것과 같다.
- 스트림은 컬렉션 스트림을 다른 컬렉션 스트림으로 변환하는 map, 집계 함수인 sum, count, average, min, max 등의 많은 메서드를 제공해준다.

### 메서드 레퍼런스와 생성자 레퍼런스
- `Arrays.stream(ages).sorted().forEach(System.out::println);`
- -> `Arrays.stream(ages).sorted().forEach(age -> System.out.println(age));`
- 인자를 아무런 가공 없이 그대로 출력. 이런 코드를 사용할 때 메서드 레퍼런스라고 하는 간략한 형식을 쓸 수 있다. 
- 메서드 레퍼런에는 세 가지 유형이 있다.
    - 인스턴스::인스턴스메서드
    - 클래스::정적메서드
    - 클래스::인스턴스메서드
- 결국 메서드 레퍼런스는 람다식으로 변형되고 최종적으로 함수형 인터페이스가 된다.
- 생성자 레퍼런스 => 클래스::new

### 인터페이스의 디폴트 메서드와 정적 메서드
- 자바 8에서 인터페이스가 가질 수 있는 멤버
    - 정적 상수
    - 추상 인스턴스 메서드
    - **구체 인스턴스 메서드 - 디폴트 메서드**
    - **(구체) 정적 메서드**

```java
public default void concreteInstanceMethod(){
    System.out.println("디폴트 메서드");
}

public static void concreteStaticMethod(){
    System.out.println("정적 메서드");
}
```

### 정리
- 람다 = 변수에 저장 가능한 로직.
- 변수는 지역 변수, 속성, 메서드의 인자, 메서드의 반환값으로 사용할 수 있다. 이것은 곧 람다도 지역 변수, 속성, 메서드의 인자, 메서드의 반환값으로 사용할 수 있다는 것을 의미한다.
- 기존의 자바에서 로직은 메서드를 통해서만 구현이 가능했다. 이제 로직을 람다로 표현할 수도 있다.
- 람다가 메서드의 역할도 할 수 있게 된 것이다.