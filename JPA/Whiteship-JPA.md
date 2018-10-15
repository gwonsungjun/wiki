# 백기선의 스프링 데이터 JPA
- 인프런 [백기선의 스프링 데이터 JPA](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/) 강의를 듣고 요약 정리

# 학습 주제
- 스프링 데이터 JPA에 대해 학습한다.
- 왜 JPA를 학습해야 하는가?
    - 도메인 주도 개발이 가능합니다.
        - 애플리케이션의 코드가 SQL 데이터베이스 관련 코드에 잠식 당하는 것을 방지하고 도메인 기반의 프로그래밍으로 비즈니스 로직을 구현하는데 집중할 수 있습니다.
    - 그리고 개발 생산성에 좋으며, 데이터베이스에 독립적인 프로그래밍이 가능하고, 타입 세이프한 쿼리 작성 그리고 Persistent Context가 제공하는 캐시 기능으로 성능 최적화까지 가능합니다.

## 학습 목차
- 핵심 개념 이해
    - 관계형 데이터베이스와 자바
    - ORM이 해결하려는 문제 (패러다임 불일치)
    - JPA의 특징 이해
    - 스프링 데이터 JPA 원리 이해
- 스프링 데이터 JPA 활용
    - 스프링 부트 연동
    - 리포지토리
    - 엔티티 저장
    - 쿼리
    - 스토어드 프로시져
    - 트랜잭션
    - Auditing

## 1부 : 핵심 개념 이해

### 관계형 데이터베이스와 자바
- 애플리케이션의 데이터(정보)를 영속화(어딘가에 저장해놓고 유지)하기 위해 DB 사용
- 데이터베이스 : 자바와 독립적 (자바 외에 다른 언어로도 DB 사용 가능)
- JDBC : (관계형) 데이터베이스와 자바의 연결 고리 (각각의 언어에 해당하는 중계 라이브러리가 존재, 자바는 JDBC)

#### JDBC
- DataSource / DriverManager
- Connection
- PreparedStatement

#### SQL
- DDL
- DML

```java
try(Connection connection = DriverManager.getConnection(url, username, password)) {
    System.out.println("Connection created: " + connection);
    String sql = "INSERT INTO ACCOUNT VALUES(1, 'keesun', 'pass');";
    try(PreparedStatement statement = connection.prepareStatement(sql)) {
        statement.execute();
    }
}

```

#### 무엇이 문제인가?
- Connection 객체를 만드는 비용이 비싸다.
- SQL을 실행하는 비용이 비싸다.
- SQL이 데이터베이스 마다 다르다.
- 스키마를 바꿨더니 코드가 너무 많이 바뀌네...
- 반복적인 코드가 너무 많아.
- 당장은 필요가 없는데 언제 쓸 줄 모르니까 미리 다 읽어와야 하나...

#### 이런 문제를 해결하고자 ORM을 사용.

### ORM : Object-Relation Mapping
- 도메인 모델 사용

```java
Account account = new Account(“keesun”, “pass”);
accountRepository.save(account);
```

#### JDBC 대신 도메인 모델 사용하려는 이유?
- 도메인 모델 기반으로 코딩을 하고 싶기 때문 즉,
- 객체 지향 프로그래밍의 장점을 활용하기 좋으니까.
- 각종 디자인 패턴
- 코드 재사용
- 비즈니스 로직 구현 및 테스트 편함. -> 유지보수 ↑

### ORM ?
- ORM은 애플리케이션의 클래스와 SQL 데이터베이스의 테이블 사이의 `맵핑 정보를 기술한 메타데이터`를 사용하여, 자바 애플리케이션의 객체를 SQL 데이터베이스의 테이블에 `자동으로 (또 깨끗하게 : 비지니스 로직과는 관계없는 JDBC때문에 써야하는 Connection과 같은 코드들이 없이) 영속화` 해주는 기술.


장점|단점
---|---
생산성|학습비용
유지보수성 |
성능 |
벤더 독립성 |


