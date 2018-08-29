# Java - try with resources

- 자바 7 이전에는 외부자원에 접근하는 경우 사용한 뒤 자원을 닫아줘야(close) 했다.
- `try with resources(자동 리소스 닫기)`는 Java SE 7 부터 지원하는 기능으로
- 하나 이상의 resource ( 프로그램이 사용후 close 해야 하는 object ) 를 선언하는 try statement가 끝나면, resource 는 자동으로 close 되는 이를 보장하는 기능이다.

## AutoCloseable
- JDK1.7에 추가된 인터페이스
- (finally 블록에서) 명시적으로 close()하지 않아도 자동으로 close()를 무조건 호출
- AutoCloseable 구현해서 사용 가능, java.lang.AutoCloseable 을 구현한 객체는 try-with-resources statement 의 resource 가 될수있다.

```java
/**
 * @author Josh Bloch
 * @since 1.7
 */
public interface AutoCloseable {
    public void close() throws IOException;
}
```

## try with resources 사용 예

```java 
// Java7 이전
BufferedReader reader = null;
try {
    reader = new BufferedReader(new FileReader("/home/sungjun/..."));
    String line = reader.readLine();
    ...
} finally {
    if (reader != null) {
        try {
            reader.close();
        } catch (Exception ex) {
            // ignored
        }
    }
}
```

```java
// Java7
try (BufferedReader reader = new BufferedReader(new FileReader("/home/sungjun/..."))) {
    String line = reader.readLine();
    ...
}
```

- finally 블록이 사라짐

### 특징
- `변수를 만들지 않은 상태로 사용할 경우 close() 메서드가 호출되지않아 자원이 수거되지않으니 꼭 유념`
- try () 안에서는 세미콜론으로 구분하여 여러개의 리소스를 생성해도 된다.
- try ()안에 들어갈 수 있는건 AutoCloseable 구현체뿐이다.
- catch or finally block 은, 선언된 resource 가 close 된후 실행된다.


## Links
- <http://kwonnam.pe.kr/wiki/java/7>
- <http://freeprog.tistory.com/202>
- <http://multifrontgarden.tistory.com/192>