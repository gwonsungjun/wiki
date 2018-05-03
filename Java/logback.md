# Logback

## Logback log 패턴
- **%-80(%d{HH:mm:ss.SSS} [%thread] %highlight(%-5level) %logger{36}) - %msg%n**

Conversion Pattern | Description
--------------------- | --------------
%d{yyyy-MM-dd-HH:mm:ss:sss} | 로깅하고 있는 현재 시간, 분, 초, 밀리초
%thread | 실행 스레드 명
%highlight | 로그 레벨에 따른 색 지정
%-5level | 로깅레벨(trace, debug, info, warn, error), 출력 고정폭 5자리, 로깅 레벨이 info일 경우 빈칸 하나 추가
%logger | 패키지 포함 클래스 정보 (Logger name)
%logger{0} | 패키지 제외한 클래스 이름만 출력
%logger{length} | length 자리수
%msg | 로깅 내용
%n | 개행 문자(new line 출력)
%method | 로깅하고 있는 클래스의 메소드
%line | 로깅하고 있는 클래스 소스의 line

- %highlight
    - 윈도우의 경우 org.fusesource.jansi:jansi:1.8 가 필요하며 (Linux ,Mac OS X는 기본적으로 지원)
    - 아래 withJansi 태그를 logback.xml에 설정한 후 사용해야 한다.
    ```xml
    <withJansi>true</withJansi>
    ``` 
    

## 고정폭
- %숫자 는 출력 고정폭 값
- -의 유무는 좌우를 결정지음
    - -가 있으면 좌로 정렬
    - ex) 맨 위의 예시에서 %-80, (%-5level)

## Links
- <http://souning.tistory.com/7>
- <https://hue9010.github.io/etc/logback-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/>