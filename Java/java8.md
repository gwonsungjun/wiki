# 자바 8 정리
- [Java 8 in Action](http://book.naver.com/bookdb/book_detail.nhn?bid=8883567)을 읽으면서 주요 요점들을 정리.

## 자바 8 기본 정보
- 함수(function) : 프로그래밍 언어에서 메서드 특히 정적 메서드와 같은 의미로 사용된다. **자바의 함수는** 이에 더해 수학적인 함수처럼 사용되며 부작용을 일으키지 않는 함수를 의미한다.
- 함수형 프로그래밍 패러다임의 핵심 사항은 공유되지 않은 가변 데이터, 메서드와 함수코드를 다른 메서드로 전달하는 기능이다.
- **메서드 레퍼런스(method reference)** : `::` : 이 메서드를 값으로 사용하라는 의미
  - 이로써 자바 8에서는 더 이상 메서드가 이급값이 아닌 일급값
  - 기존의 객체 레퍼런스(new로 객체 레퍼런스 생성)를 이용해서 객체를 이리저리 주고 받았던 것처럼 (예)File::isHidden을 이용해서 메서드 레퍼런스를 만들어 전달할 수 있게 되었다.
- **람다(lamda)** : 익명 함수 `(int x) -> x +1`
  - 자바 8에서는 named 메소드를 일급값으로 취급할 뿐 아니라 람다(또는 익명함수)를 포함하여 함수도 값으로 취급할 수 있다.
- Predicate : 프레디케이트란 수학에서 인수로 값을 받아 true, false를 반환하는 함수를 의미한다.
- **스트림(Stream)** : 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임
  - 스트림 API를 이용하면 루프를 신경 쓸 필요가 없어진다.
  - 라이브러리 내부에서 모든 데이터가 처리된다.
  - 이러한 처리를 내부 반복이라고 한다. <-> 외부반복(for-each)
  - 스트림과 람다 표현식을 이용하면 병렬성을 공짜로 얻을 수 있다.
- 스트림 API의 핵심은, 기존에는 한 번에 한 항목을 처리했지만 이제 자바8에서는 우리가 하려는 작업을 (데이터베이스 질의처럼) 고수준으로 추상화해서 일련의 스트림으로 만들어 처리할 수 있다는 것이다.
- **디폴트 메서드(default method)** : 구현 클래스에서 구현하지 않아도 되는 메서드를 인터페이스가 포함할 수 있는 기능을 제공, 메서드 바디는 클래스 구현이 아니라 인터페이스의 일부로 포함된다.
  - 기존의 코드를 건드리지 않고도 원래의 인터페이스 설계를 자유롭게 확장할 수 있다.

## 동작 파라미터화 코드 전달하기
- 동작 파라미터화 (behavior parameterization)을 이용화면 자주 바뀌는 요구사항에 효과적으로 대응할 수 있다.
- **즉, 메서드 내부적으로 다양한 동작을 수행할 수 있도록 코드를 메서드 인수로 전달한다.**
- 결과적으로 코드 블록에 따라 메서드의 동작이 파라미터화 된다.
- 변화하는 요구사항에 효과적으로 대응하는 코드를 구현할 수 있음(유연하고 재사용할 수 있는 코드를 만들 수 있다.)

- Comparator로 정렬하기 예제
```java
public interface Comparator<T> {
  public int compare(T o1, T o2);
}
```
- Comparator를 구현해서 sort 메서드의 동작을 다양화할 수 있다. 예를 들어 익명 클래스를 이용해서 무게가 적은 순으로 목록에서 사과를 정렬할 수 있다.
```java
inventory.sort(new Comparator<Apple>(){
  public int compare(Apple a1, Apple a2){
    return a1.getWeight().compareTo(a2.getWeight());
  }
});
```
- 람다표현식을 이용해 간단하게 코드 구현
```java
inventory.sort( (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```
  - `전략 디자인 패턴(strategy design pattern) : 각 알고리즘(전략이라 불리는)을 캡슐화하는 알고리즘 패밀리를 정의해둔 다음에 런타임에 알고리즘을 선택하는 기법`

## 람다 표현식
### 람다란 무엇인가 ?
- `람다 표현식`은 메서드로 전달할 수 있는 익명 함수를 단순화한 것
- 이름은 없지만, 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트는 가질 수 있다.
  - `익명` : 보통의 메서드와 달리 이름이 없으므로 `익명`이라고 표현.
  - `함수` : 람다는 메서드처럼 특정 클래스에 종속되지 않으므로 `함수`라고 부름. 하지만 메서드처럼 파라미터 리스트, 바디, 반환 형식, 가능한 예외 리스트를 포함
  - `전달` : 람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
  - `간결성` : 익명 클래스처럼 많은 자질구레한 코드를 구현할 필요가 없다. 간결한 방식으로 코드 전달 가능.
- 람다라는 용어는 람다 미적분학 학계에서 개발한 시스템에서 유래.
- 람다는 세 부분으로 이루어진다.
  - `파라미터 리스트` : Comparator의 compare 메서드의 파라미터 (두 개의 사과). `(Apple a1, Apple a2)`
  - `화살표` : 화살표(->)는 람다의 파라미터 리스트와 바디를 구분
  - `람다의 바디` : 람다의 반환값에 해당하는 표현식(두 사과의 무게를 비교) `a1.getWeight().compareTo(a2.getWeight());`
  ```java
  ```
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
- 자바 8에서 지원하는 다섯 가지 람다 표현식
  - `(String s) -> s.length()` : return문 함축되어 있음. return 문을 명시적으로 사용하지 않아도 된다.
  - `(Apple a) -> a.getWeight() > 150` : boolean 반환
  - () -> 42 : 파라미터가 없으며 int 반환
  - `(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());` : Apple 형식의 파라미터 두 개, int를 반환
  ```java
  - (int x, int y) -> {
    System.out.println("Result :");
    System.out.println(x+y);
  }
  ```
  : 여러행의 문장을 중괄호를 이용하여 표현 가능.
- 람다 예제
  - 불린 표현식 : (List<String> list) -> list.isEmpty()
  - 객체 생성 : () -> new Apple(10)
  - 객체에서 소비 : (Apple a) -> {System.out.println(a.getWeight());}
  - 객체에서 선택/추출 : (String s) -> s.length()
  - 두 값을 조합 : (int a, int b) -> a*b
  - 두 객체 비교 : (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())

### 어디에, 어떻게 람다를 사용할까?
- 어디에서 람다를 사용할 수 있다는 건가? `함수형 인터페이스`라는 문맥에서 람다 표현식을 사용할 수 있다.

#### 함수형 인터페이스
- `함수형 인터페이스` : 정확히 하나의 추상 메서드를 지정하는 인터페이스
```java
public interface Predicate<T>{
  boolean test (T t);
}
```
- 인터페이스는 `디폴트 메서드`(인터페이스의 메서드를 구현하지 않은 클래스를 고려해서 기본 구현을 제공하는 바디를 포함하는 메서드)를 포함할 수 있다. 많은 디폴트 메서드가 있더라도 `추상 메서드가 오직 하나면` 함수형 인터페이스다.
- 함수형 인터페이스로 무엇을 할 수 있을까?
  - 람다 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있으므로 `전체 표현식을 함수형 인터페이스의 인스턴스로 취급`(기술적으로 따지면 함수형 인터페이스를 concrete 구현한 클래스의 인스턴스)할 수 있다.

#### 함수 디스크립터
- `함수형 인터페이스의 추상 메서드 시그니처(signature)`는 람다 표현식의 시그니처를 가리킨다.
- 람다 표현식의 시그니처를 서술하는 메서드를 함수 디스크립터(function descriptor)라고 부른다.
- 특수 표기법으로 람다와 함수형 인터페이스의 시그니처를 설명
  - () -> void 라는 표기는 파라미터 리스트가 없으며 void를 반환하는 함수를 의미
- 기억할 것
  - **람다 표현식은 변수에 할당하거나 함수형 인터페이스를 인수로 받는 메서드로 전달할 수 있으며, 함수형 인터페이스의 추상 메서드와 같은 시그니처를 갖는다는 사실을 기억**
- @FunctionalInterface는 무엇인가?
  - 함수형 인터페이스임을 가리키는 어노테이션
  - 이 어노테이션으로 인터페이스를 선언했지만 실제로 함수형 인터페이스가 아니면 컴파일러가 에러 발생 시킴.

### 람다 활용 : 실행 어라운드 패턴
- 초기화/준비 코드 - 작업 - 정리/마무리 코드
- 실행 어라운드 패턴을 적용하는 네 단계의 과정
- 1단계
```java- 반드시 WEB-INF 폴더가 존재 해야한다. [그림참조](https://www.edwith.org/boostcourse-web/lecture/16686/)
public static String processFile() throws IOException
 try (BufferedReader br =
        new BufferedReader(new FileReader("data.txt"))){
          return br.readLine();
        }
 }
```
- 2단계 : 함수형 인터페이스를 이용해서 동작 전달
```java
public interface BufferedReaderProcessor{
  String porcess (BufferedReader b) throws IOException;
}
public static String processFile(BufferedReaderProcessor p) throws IOException {
  ...
}
```
- 3단계 : 동작 실행
```java
public static String processFile(BufferedReaderProcessor p) throws IOException{
  try (BufferedReader br =
          new BufferedReader(new FileReader("data.txt"))) {
            return p.process(br);
          }
}
```
- 4단계 : 람다 전달
```java
String oneLine = processFile((BufferedReader br) -> br.readLine());

String twoLine = processFile((BufferedReader br) -> br.readLine() + br.readLine());
...
```

### 함수형 인터페이스 사용
- 함수형 인터페이스의 추상 메서드 시그니처를 함수 디스크립터라 한다.
- 다양한 람다 표현식을 사용하려면 공통의 함수 디스크립터를 기술하는 함수형 인터페이스 집합이 필요하다. (따로 정의할 필요 없이 바로 사용할 수 있도록)

#### Predicate
- java.util.function.Predicate<T> 인터페이스는 test라는 추상 메서드를 정의하면 test는 제네릭 형식의 T의 객체를 인수로 받아 불린을 반환
```java
@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
}
```

#### Consumer
- java.util.function.Consumer<T> 인터페이스는 제네릭 형식 T 객체를 받아서 void를 반환하는 accept라는 추상 메서드를 정의
```java
@FunctionalInterface
public interface Consumer<T> {
  void accept(T t);
}
```

#### Function
- java.util.function.Function<T, R> 인터페이스는 제네릭 형식 T를 인수로 받아서 제네릭 형식 R 객체를 반환하는 apply라는 추상 메서드를 정의. 입력을 출력으로 매핑하는 람다를 정의할 때 활용.
```java
@FunctionalInterface
public interface Function<T, R> {
  R apply (T t);
}
```

#### 기본형 특화
- 자바의 모든 형식은 참조형(Byte, Integer, Object ...) 아니면 기본형(int double, byte ....)에 해당한다. 하지만 제네릭 파라미터(예를 들어 Consumer<T> t)에는 참조형만 사용할 수 있다.
- 자바에서는 기본형을 참조형으로 변환할 수 있는 기능 제공 `박싱(boxing)`
- 참조형을 기본형으로 변환하는 반대 동작을 `언박싱(unboxing)`
- 자동으로 이루어지는 `오토박싱(autoboxing)` 기능도 제공
- 하지만 이런 변환 과정은 비용이 소모된다. 박싱한 값은 기본형을 감싸는 래퍼며 힙에 저장된다. 따라서 박싱한 값은 메모리를 더 소비하며 기본형을 가져올 때도 메모리를 탐색하는 과정이 필요하다.
- 자바 8에서는 기본형을 입출력으로 사용하는 상황에서 오토박싱 동작을 피할 수 있도록 특별한 버전의 함수형 인터페이스를 제공
```java
public interface IntPredicate {
  boolean test(int t);
}

IntPredicate evenNumbers = (int i) -> i % 2 == 0;
evenNumbers.test(1000); <- 참(박싱없음)

Predicate<Integer> oddNumbers = (Integer i) -> i % 2 == 1;
oddNumbers.test(1000); <- 거짓(박싱)

```
- 일반적으로 특정 형식을 입력으로 받는 함수형 인터페이스의 이름 앞에는 DoublePredicate, IntConsumer, IntFunction처럼 형식명이 붙는다.
- 자바 8의 대표적인 함수형 인터페이스 [그림참조](https://www.slideshare.net/heechanglee7/8-49928187)
![java8lamda](/images/java8lamda.jpg)

#### 예외, 람다, 함수형 인터페이스의 관계
함수형 인터페이스는 확인된 예외를 던지는 동작을 허용하지 않는다. 즉, 예외를 던지는 람다 표현식을 만들려면 `확인된 예외를 선언하는 함수형 인터페이스를 직접 정의`하거나 `람다를 try/catch 블록으로 감싸야` 한다.
```java
@FunctionalInterface
public interface BufferedReaderProcessor {
  String process(BufferedReader b) throws IOException;
}
BufferedReaderProcessor p = (BufferedReader br) -> br.readLine();
```
```java
Function<BufferedReader, String> f =
  (BufferedReader b) -> {
    try{
      return b.readLine();
    }
    catch(IOException e){
      throw new RuntimeException(e);
    }
  }
```
