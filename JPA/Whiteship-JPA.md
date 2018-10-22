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

### 1. 관계형 데이터베이스와 자바
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

### 2. ORM : Object-Relation Mapping
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


### 3. ORM : 패러다임 불일치
- 객체를 릴레이션에 맵핑하려니 발생하는 문제들을 아래 나열
- 패러다임 불일치의 문제(아래 나열한 문제, 즉 둘 사이 갭이 큼)들을 줄이고 호환이 가능하도록 노력하고 있는 것이 ORM. 따라서 조금 어렵고 학습 비용이 비싸다.

#### 밀도(Graularity) 문제

객체|릴레이션
---|---
다양한 크기의 객체를 만들 수 있음. 커스텀한 타입 만들기 쉬움| 테이블, 기본 데이터 타입(UDT(User Defined Type)은 비추)

#### 서브타입(Subtype) 문제

객체 | 릴레이션
---|---
상속 구조 만들기 쉬움. 다형성 | 테이블 상속이라는게 없음. 상속 기능을 구현했다 하더라도 표준 기술이 아님. 다형적인 관게를 표현할 방법이 없음.

#### 식별성(Identity) 문제

객체 | 릴레이션
---|---
레퍼런스 동일성 (==), 인스턴스 동일성(equals() 메소드) | 주키(primary key)

#### 관계(Association) 문제

객체 | 릴레이션
---|---
객체 레퍼런스로 관계 표현. 근복적으로 '방향'이 존재한다. 다대다 관계를 가질 수 있음 | 외래키(foreign key)로 관계 표현. '방향'이라는 의미가 없음. 그냥 Join으로 아무거나 묶을 수 있음. 태생적으로 다대다 관계를 못만들고, 조인 테이블 또는 링크 테이블을 사용해서 두개의 1대다 관계로 만들어야 함.

#### 데이터 네비게이션(Nevigation) 문제

객체 | 릴레이션
---|---
레퍼런스를 이용해서 다른 객체로 이동 가능. 콜렉션을 순회할 수도 있음. | 하지만 그런 방식은 릴레이션에서 데이터를 조회하는데 있어서 매우 비효율적이다. 데이터베이스에 요청을 적게 할 수록 성능이 좋다. 따라서 Join을 쓴다. 하지만 너무 한번에 많이 가져오려고 해도 문제다. 그렇다고 lazy loading을 하자니 그것도 문제다(n+1 select)

### 4. JPA 프로그래밍 프로젝트 세팅

#### (1)프로젝트 생성 : 의존성 따로 추가 없이 Spring boot 프로젝트 생성
- 스프링 부트 v2.*
- 스프링 프레임워크 v5.*

#### (2) 데이터 베이스 실행
-  PostgreSQL 도커 컨테이너 사용 

```bash
docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=sungjun -e POSTGRES_DB=springdata --name postgres_boot -d postgres

docker exec -i -t postgres_boot bash

su - postgres

psql springdata
( Windows : psql --username sungjun --dbname springdata)

데이터베이스 조회
\list

테이블 조회
\dt

쿼리
SELECT * FROM account;

```

#### (3) 스프링 부트 스타터 JPA 설정
- JPA 프로그래밍에 필요한 의존성 추가
    - JPA v2. *, Hibernate v5. * 
- PostgreSQL driver 의존성 추가

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>

```

#### (4) JDBC 설정
- application.properties 
    - datasource 설정
- 자동 설정: HibernateJpaAutoConfiguration
    - 컨테이너가 관리하는 EntityManager (프록시) 빈 설정
    - PlatformTransactionManager 빈 설정

```property
spring.datasource.url=jdbc:postgresql://localhost:5432/springdata
spring.datasource.username=sungjun
spring.datasource.password=pass

spring.jpa.hibernate.ddl-auto=create (update, validate)
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
```

### JPA 프로그래밍 : 엔티티 맵핑
- 도메인 모델을 릴레이션(테이블)에 어떻게 매핑시킬지에 대한 정보를 하이버네이트에 줘야한다.
    - (1) **Annotation**
    - (2) xml
- getter, setter 없어도 맵핑 가능함.

#### @Entity
- "엔티티"는 객체 세상에서 부르는 이름.
- 보통 클래스와 같은 이름을 사용하기 때문에 값을 변경하지 않음.
    - ex) @Entity (name = "users") 가능.
- 엔티티의 이름은 JQL에서 쓰임(하이버네이트 즉, 객체 세상 안에서만)

#### @Table
- "릴레이션" 세상에서 부르는 이름.
- @Entity의 이름이 기본값. (따라서, @Table은 생략)
- 테이블의 이름은 SQL에서 쓰임.

#### @Id
- 엔티티의 주키를 맵핑할 때 사용.
- 자바의 모든 primitive 타입과 그 랩퍼 타입을 사용할 수 있음
    - Date랑 BigDecimal, BigInteger도 사용 가능.
    - 주로 랩퍼 타입을 사용
- 복합키를 만드는 맵핑 방법도 있지만 그건 논외로..

#### @GeneratedValue
- 주키의 생성 방법을 맵핑하는 애노테이션
- 생성 전략과 생성기를 설정할 수 있다.
    - 기본 전략은 AUTO : 사용하는 DB에 따라 적절한 전략 선택
    - TABLE, SEQUENCE, IDENTITY 중 하나.

#### @Column
- 사용하지않아도 자동으로 @Column이 붙어서 테이블에 맵핑됨.
- 추가적인 제약 사용
    - unique = true
    - nullable = false
    - length
    - columDefinition
    - ...

#### @Temporal
- @Temporal을 사용하면 DB 타입에 맞도록 매핑할 수 있다.
- 현재 JPA 2.1까지는 Date와 Calendar만 지원.
- 이후 LocalDateTime 등 추가될 예정.

```java
@Temporal(TemporalType.TIMESTAMP)
private Date created = new Date();
```

#### @Transient
- 컬럼으로 맵핑하고 싶지 않은 멤버 변수