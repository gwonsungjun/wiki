# 백기선의 스프링 데이터 JPA
- 인프런 [백기선의 스프링 데이터 JPA](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/) 강의를 듣고 요약 정리
- [실습 GitHub Repository](https://github.com/gwonsungjun/springdata-jpa)

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
- `패러다임 불일치의 문제(아래 나열한 문제, 즉 둘 사이 갭이 큼)들을 줄이고 호환이 가능하도록 노력하고 있는 것이 ORM`. 따라서 조금 어렵고 학습 비용이 비싸다.

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

### 5. JPA 프로그래밍 : 엔티티 맵핑
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

### 6. JPA 프로그래밍 : Value 타입 맵핑

- 엔티티 타입과 Value 타입 구분
    - 식별자가 있어야 하는가 
    - 독립적으로 존재해야 하는가
    - 엔티티 타입은 고유 식별자가 존재하며 독립적으로 존재해야 한다. 반대는 종속적.

- Value 타입 종류
    - 기본 타입 (String, Date, Boolean, ...)
    - Composite Value 타입 (기본 타입보다 조금 더 큰 단위의 Value 타입)
    - Collection Value 타입
        - 기본 타입의 콜렉션
        - 컴포짓 타입의 콜렉션
    - Composite Value 타입 맵핑
        - @Embadable
        - @Embadded
        - @AttributeOverrides (매핑 정보 재정의)
        - @AttributeOverride

```java
// Composite Value 타입

@Embeddable
public class Address {

    private String street;

    private String city;

    private String state;

    private String zipCode;

}
```

```java
    // Account Class

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "street", column = @Column(name = "home_street"))
    })
    private Address address;

    // Address의 street을 home_street으로 바꿔서 쓰겠다.
```

### 7. JPA 프로그래밍 : 관계 맵핑 (1대다 맵핑)
- 관계에는 `항상 두 엔티티`가 존재 한다.
    - 둘 중 하나는 그 관계의 주인(owning) (주인 = 관계를 설정했을 때 그 값이 반영 됨)이고
    - 다른 쪽은 종속된(non-owning) 쪽이다.
    - 해당 관계의 반대쪽 레퍼런스를 가지고 있는 쪽이 주인.
- 단방향에서의 관계의 주인은 명확하다.
    - 관계를 정의한 쪽이 그 관계의 주인이다.
- 단방향 @ManyToOne
    - 기본값은 FK 생성
- 단방향 @OneToMany
    - 기본값은 조인 테이블 생성
- @ManyToOne, @OneToMany가 헷갈릴때는 뒤쪽 One - 하나, Many - 여러개(콜렉션)을 보고 설정하면 된다.
- 양방향
    - FK 가지고 있는 쪽이 오너 따라서 기본값은 @ManyToOne 가지고 있는 쪽이 주인.
    - 주인이 아닌쪽(@OneToMany쪽)에서 mappedBy 사용해서 관계를 맺고 있는 필드를 설정해야 한다.
        - `@OneToMany(mapped By = "owner")`
        - 그래야 필요없는 스키마와 중복데이터가 발생되지 않음.
    - @OneToMany에 mappedBy를 설정하지 않고 양쪽에 @ManyToOne, @OneToMany만 설정하면 그냥 서로 바라보는 단방향이 된다.
- 양방향
    - @ManyToOne (이쪽이 주인)
    - @OneToMany(mappedBy)
    - 주인한테 관계를 설정해야 DB에 반영이 된다.
        - 반대쪽에 관계를 설정하면 DB에 반영되지 않음.
        - 주인한테만 관계를 설정해도 되지만 객체지향적으로 생각했을때 (서로의 레퍼런스를 가지고 있어야하므로) 두개를 묶어서 설정
        - 두개의 묶음을 convenient method라고 부른다. (add, reomve 등 한쪽에서 더하고 지우면 다른쪽도 반영이 되야함.)

### 8. JPA 프로그래밍 : Cascade
- JPA, 하이버네이트에서 가장 중요한 개념 중 하나인 `엔티티의 상태`, `상태 전이(Cascade) 옵션`.
- Cascade : 엔티티의 `상태 변화를 전파` 시키는 옵션.
    - 보통 `cascade = {CascadeType.ALL}` 사용
- @OneToMany, @ManyToOne에서 Cascade 옵션을 줄 수 있다.
- Default 아무 것도 없음 -> 전이 시키지 않음.
- 엔티티의 상태 4가지 (성능상 장점!)
    - Transient : JPA가 모르는 상태 
        - new로 생성된 객체는 현재 DB에 들어갈지도 모르는 상태
    - Persistent : JPA가 관리중인 상태 (1차 캐시, Dirty Checkcing, Write Behind, ...)
        - 객체의 변경사항을 모니터링 하고 있다는 뜻
        - Save를 했을때 Persistent 상태가 된다
        - but, Save를 했다고 바로 DB에 들어가는 것이 아님, Persistent에서 관리하고 있는 객체가 이쯤 됬으면 데이터 베이스에 넣어야 겠다고 판단하면 저장함. 
        - 따라서, save 즉시 insert 쿼리가 발생되지 않음.
        - 1차 캐시
            - PersistenceContext : EntityManager, Session에 인스턴스를 넣은 상태(캐시가 된 상태)
        - Dirty Checkcing
            - 객체의 변경사항을 계속해서 감지
        - Write Behind
            - 객체 상태의 변화를 데이터베이스에 최대한 늦게(가장 필요한 시점에) 반영
    - Detached : JPA가 더이상 관리하지 않는 상태
        - trasaction이 끝나서 밖에서 사용이 될 때(return) 
        - reattached (다시 Persistent로 ): Session.update(), Session.merge(), Session.saveOrUpdate()
    - Removed : JPA가 관리하긴 하지만 삭제하기로 한 상태

    ![JpaCascade](/images/jpaCascade.PNG)


### 9. JPA 프로그래밍 : Fetch (mode)
- 연관 관계의 엔티티(정보)를 어떻게 가져올 것이냐... 지금(Eager)? 나중에(Lazy)?
    - @OneToMany의 기본값은 Lazy
        - 다음과 같이 설정 : `@OneToMany(mappedBy ="post", cascade = {CascadeType.ALL}, fetch = FetchType.EAGER)`
    - @ManyToOne의 기본값은 Eager
        - 데이터가 하나이기 때문에 Eager 합리적
- 실습 repository에 Post, Comment Class 참조

```java

(Post class의 comments : @OneToMany - Lazy) 

Session session2 = entityManager.unwrap(Session.class);
Post post = session2.get(Post.class, 1L);
System.out.println(post.getTitle());

결과 : coomment제외한 post 정보만 가져온다.

Hibernate: 
select
post0_.id as id1_2_0_,
post0_.title as title2_2_0_ 
from
post post0_ 
where
post0_.id=?

```


```java

(Post class의 comments :  @OneToMany - Eager)

Session session2 = entityManager.unwrap(Session.class);
Post post = session2.get(Post.class, 1L);
System.out.println(post.getTitle());

결과 : Left Outer Join을 통하여 Comment정보와 같이 가져온다.

Hibernate: 
    select
        post0_.id as id1_2_0_,
        post0_.title as title2_2_0_,
        comments1_.post_id as post_id3_1_1_,
        comments1_.id as id1_1_1_,
        comments1_.id as id1_1_2_,
        comments1_.comment as comment2_1_2_,
        comments1_.post_id as post_id3_1_2_ 
    from
        post post0_ 
    left outer join
        comment comments1_ 
            on post0_.id=comments1_.post_id 
    where
        post0_.id=?
```

```java

(Comment class의 post : @ManyToOne (default : Eager))

Session session2 = entityManager.unwrap(Session.class);
Comment comment = session2.get(Comment.class, 2L);
System.out.println(comment.getComment());
System.out.println(comment.getPost().getTitle());

결과 : (쿼리가 2개 날아가지 않음 즉, comment 조회, post 조회 X) 이미 코멘트를 조회할 때 fetch로 post에 대한 내용도 같이 가져왔기 때문에 쿼리는 하나 날아가고 둘다 출력 가능.

Hibernate: 
    select
        comment0_.id as id1_1_0_,
        comment0_.comment as comment2_1_0_,
        comment0_.post_id as post_id3_1_0_,
        post1_.id as id1_2_1_,
        post1_.title as title2_2_1_ 
    from
        comment comment0_ 
    left outer join
        post post1_ 
            on comment0_.post_id=post1_.id 
    where
        comment0_.id=?
```

- 어떻게 잘 조정하느냐에 따라 성능에 영향을 준다.


### 10. JPA 프로그래밍 : Query
- JPQL (HQL)
    - Java Persistence Query Language / Hibernate Query Language
    - `데이터베이스 테이블이 아닌, 엔티티 객체 모델 기반(기준)`으로 쿼리 작성
    - JPA 또는 하이버네이트가 해당 쿼리를 SQL로 변환해서 실행함.
    - DB에 독립적 JPQL(HQL) -> SQL 
    - 단점 : 타입 세이프하지 않다 (`SELECT p FROM Post As p` 문자열 이기 때문에 얼마든지 오타가 발생할 수 있다.)
    - [레퍼런스 참조](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#hql)

```java
TypedQuery<Post> query = entityManager.createQuery("SELECT p FROM Post As p", Post.class);
        List<Post> posts = query.getResultList();
        posts.forEach(System.out::println)

Post는 Entity 이름(테이블 이름 x)
```

- Criteria
    - 타입 세이프 쿼리 (문자열이 하나도 안들어감.)
    - [레퍼런스 참조](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#criteria)

```java
CriteriaBuilder builder = entityManager.getCriteriaBuilder();
CriteriaQuery<Post> criteria = builder.createQuery(Post.class);
Root<Post> root = criteria.from(Post.class);
criteria.select(root);
List<Post> posts = entityManager.createQuery(criteria).getResultList();
posts.forEach(System.out::println)
```

- Named Query
    - Mybatis와 유사.

```java
entityManager.createNamedQuery("all_post", Post.class);

NamedQuery를 Entitiy위에 정의 필요.
@NamedQueries ...
```

- Native Query
    - SQL 쿼리 실행하기.
    - [레퍼런스 참조](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#sql)

```java
List<Post> posts = entityManager
                .createNativeQuery("SELECT * FROM Post", Post.class)
                .getResultList();
```

### 11. 스프링 데이터 JPA 소개 및 원리

```java
@Repository
@Transactional
public class PostRepository {

    @PersistenceContext
    EntityManager entityManager;

    public Post add(Post post) {
        entityManager.persist(post);
        return post;
    }

    public void delete(Post post) {
        entityManager.remove(post);
    }

    public List<Post> findAll() {
        return entityManager.createQuery("SELECT p FROM Post AS p", Post.class)
                .getResultList();
    }
}

```

- 위의 Repository class와 같이 직접 만들어 쓸 수 있지만 매번 너무 뻔한 코드들을 작성해야 하므로 CRUD operation들을 제공해주는 JpaRepository를 상속받는 interface를 생성해서 사용한다.

```java
public interface PostRepository extends JpaRepository<Post, Long> {}
```

- JpaRepository<Entity, id> 인터페이스
    - 매직 인터페이스
    - @Repository가 없어도 빈으로 등록해 줌.
    - 코드 자체가 줄어드므로 개발 생산성, 유지 보수성 ↑
- @EnableJpaRepositories
    - 매직의 시작은 여기서 부터
    - 원래는 @configuration 클래스에 붙여야하나 스프링부트는 자동 설정을 해줌.
- 매직은 어떻게 이뤄지나?
    - @EnableJpaRepositories -> @import(JpaRepositoriesRegistrar.class)
        - 시작은 @import(JpaRepositoriesRegistrar.class)
    - 핵심은 ImportBeanDefinitionRegistrar 인터페이스
        - Bean을 프로그래밍을 통해 등록할 수 있게끔 해준다.
        - 프로그래밍을 통해서 JpaRepository를 상속받은 모든 interface를 찾아서 이 interface 타입의 빈을 만들어서 등록해줄 수 있다는 뜻.

#### ※ Hibernate 사용 시 무슨 쿼리를 발생시키는지, 내가 의도한건지 확인하는 습관 중요하다. 확인 후 꼭 튜닝 필요! 튜닝하지 않을 거면 하이버베이트를 쓰면 안된다.
- 팁
    - logging.level.org.hibernate.SQL=debug (=spring.jpa.show-sql=true)
    - logging.level.org.hibernate.type.descriptor.sql=trace (물음표(?)로 가려지는 값을 보여줌.)

## 2부 : 스프링 데이터 JPA 활용

### 1. 스프링 데이터 JPA 활용 파트 소개
- 스프링 데이터란 하나의 프로젝트가 아닌 여러개의 프로젝트 묶음을 지칭하는 말
- 그 중 스프링 데이터 JPA를 학습 할 예정

 ![springdatajpa](/images/springdatajpa.PNG)

project | description 
---|---
스프링 데이터 | SQL & NoSQL 저장소 지원 프로젝트의 묶음.
스프링 데이터 Common | 여러 저장소 지원 프로젝트의 공통 기능 제공.
스프링 데이터 REST | 저장소의 데이터를 하이퍼미디어 기반 HTTP 리소스로(REST API로) 제공하는 프로젝트.
스프링 데이터 JPA | 스프링 데이터 Common이 제공하는 기능에 JPA 관련 기능 추가.

- 스프링 데이터 Common : repository를 생성해서 Bean으로 등록하는 방법, 쿼리 메소드를 만드는 기본 메카니즘 등이 들어있다.

### 2. 스프링 데이터 Common : Repository

![jpa_repository](/images/jpa_repository.PNG)

- Repository : Marker interface, 기능을 가지진 않음.
- CrudRepository : 기본적으로 save, saveAll, findById, existById, findAll, findAllById, count, deletById ... , 기본적인 crud operation 제공
- PagingAndSortingRepository : findAll(Pageable pageable) 많이 사용
- JpaRepository : 쿼리메소드 생성 가능.

### 3. 스프링 데이터 Common : Repository 인터페이스 정의하기
- Repository 인터페이스로 공개할 메소드를 직접 일일히 정의하고 싶다면

- 특정 리포지토리 당
    - @RepositoryDefinition
        - 제공하고 싶은 기능들만 정의 (2개의 기능만 제공)
        - 각 엔티티마다 아래의 기능들이 필요하다면 Repository를 또 정의해야함. -> 공통으로 사용할 repository 정의 가능 : @NoRepositoryBean

```java
@RepositoryDefinition(domainClass = Comment.class, idClass = Long.class) // Bean 등록
public interface CommentRepository {

    Comment save(Comment comment); // 메소드에 해당하는 기능을 구현 해줌.

    List<Comment> findAll();
    
}
```

- 공통 인터페이스 정의
    - @NoRepositoryBean
    - 위의 CommentRepository에서 정의한 메소드는 지우고 아래 MyRepository를 상속받아서 사용하면 된다.

```java
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable> extends Repository<T, ID> {

    <E extends T> E save(E entity);

    List<T> findAll();

}
```

### 4. 스프링 데이터 Common : Null 처리하기

- 스프링 데이터 2.0 부터 자바 8의 Optional 지원.
    - `Optional<Post> findById(Long id);`
        - `<E extends  T> Optional<E> fidById(Id id);`
- 콜렉션은 Null을 리턴하지 않고, 비어있는 콜렉션을 리턴.
    - 단일값은 Null을 리턴할 수 있음.
    - 스프링 데이터 JPA의 특징이다.
        - 스프링 데이터 JPA가 지원하는 Repository의 콜렉션들은 Null 리턴 X, 비어있는 콜렉션 리턴.
        - 따라서 콜렉션은 Null 체크해봤자 필요가 없음! (무조건 Null이 아니기 때문에)
- 스프링 프레임워크 5.0부터 지원하는 Null 애노테이션 지원.
    - @NonNullApi(pakage-info.java를 통해 패키지 전역에 설정), @NonNull, @Nullable.
    - 런타임 체크 지원 함.(런타임시 null 여부를 자동으로 체크하는 코드를 심는다고 생각)
    - JSR 305 애노테이션을 메타 애노테이션으로 가지고 있음. 
        - 인텔리제이에서는 아직 스프링이 지원하는 Null 애노테이션을 잘 모르는 것 같음. 따라서 명시적으로 추가하면 코딩할 때 힌트를 받을 수 있다.
- 인텔리J 설정(을 통해 Tool 지원받기.)
    - (1) settings에서 runtime assertion 검색 & CONFIGURE ANNOTATIONS... 선택
        - ![springdataNull](/images/springdataNull.PNG)
    - (2) 위는 Nullable 아래는 NotNull
        - Nuallable에 spring Nullable 추가, Notnull에 spring NonNull 추가
        - ![springdataNull2](/images/springdataNull2.PNG)
    - (3) 적용을 위해 Restart

### 5. 스프링 데이터 Common : 쿼리 만들기 개요

#### 스프링 데이터 저장소의 메소드 이름으로 쿼리 만드는 방법

- 메소드 이름을 분석해서 쿼리 만들기 (CREATE)
     - 메소드 이름을 분석해서 스프링 데이터 JPA가 직접 쿼리를 만들어줌
        - `List<Comment> findByTitleContains(String keyword);`
- 미리 정의해 둔 쿼리 찾아 사용하기 (USE_DECLARED_QUERY)
    - 메소드에 붙어있는 부가적인 정보 (annotation)를 찾아서 실행해줌.
        - `@Query("SELECT c FROM Comment AS c")` : 기본 값 JPQL
        - natriveQuery = true 속성을 추가하면 SQL 
- 미리 정의한 쿼리 찾아보고 없으면 만들기 (CREATE_IF_NOT_FOUND)
    - 위의 2개를 합친 것으로 기본값. 미리 정의한 쿼리가 있으면 먼저 사용.
    - @EnableJpaRepositories(queryLookupStrategy = QueryLookUpStrategy.Key.CREATE_IF_NOT_FOUND)

#### 쿼리를 만드는 방법
- `리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)[(And|Or){프로퍼티 표현식}(조건식)]{정렬 조건} (매개변수)`
- 만든 쿼리가 맞는지 확인하려면 실행해보면 됨
    - 빈을 만들떄 쿼리까지 만들기 때문에 만든 쿼리가 맞지 않다면 에러

식|예
---|---
접두어|Find, Get, Query, Count, ...
도입부|Distinct, First(N), Top(N)
프로퍼티 표현식|Person.Address.ZipCode => find(Person)ByAddress_ZipCode(...)
조건식|IgnoreCase, Between, LessThan, GreaterThan, Like, Contains, ...
정렬 조건|OrderBy{프로퍼티}Asc|Desc
리턴 타입|E, Optional<E>, List<E>, Page<E>, Slice<E>, Stream<E>
매개변수|Pageable, Sort

#### 쿼리 찾는 방법
- 메소드 이름으로 쿼리를 표현하기 힘든 경우에 사용.
- 저장소 기술에 따라 다름.
- JPA: @Query @NamedQuery


### 6. 스프링 데이터 Common : 쿼리 만들기 실습
- 기본 예제
    - List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
    - distinct
        - List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
        - List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
    - ignoring case
        - List<Person> findByLastnameIgnoreCase(String lastname);
        - List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
- 정렬
    - List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
    - List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
- 페이징
    - Page<User> findByLastname(String lastname, Pageable pageable);
    - Slice<User> findByLastname(String lastname, Pageable pageable);
    - List<User> findByLastname(String lastname, Sort sort);
    - List<User> findByLastname(String lastname, Pageable pageable);

    ```java
    PageRequest pageRequest = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "LikeCount"));

    Page<Comment> comments = commentRepository.findByCommentContainsIgnoreCase("Spring", pageRequest);
    assertThat(comments.getNumberOfElements()).isEqualTo(2);
    ```

- 스트리밍
    - Stream<User> readAllByFirstnameNotNull();
        - `try-with-resource 사용할 것. (Stream을 다 쓴다음에 close() 해야 함)`

### 7. 스프링 데이터 Common : 비동기 쿼리
- 비동기 쿼리
    - @Async Future<User> findByFirstname(String firstname);
        - java5에 등장
        - `future.get();` : 결과가 나올때 까지 기다림
        - `futrue.isDone();` : 결과가 나왔는지 확인 
    - @Async CompletableFuture<User> findOneByFirstname(String firstname); 
        - java8에 등장
    - @Async ListenableFuture<User> findOneByLastname(String lastname); 
        - 스프링에서 만든 것으로 젤 깔끔한 API
        - 해당 메소드를 스프링 TaskExecutor에 전달해서 별도의 쓰레드에서 실행함. Reactive랑은 다른 것임
        - `future.addCallback();`
- @Async 사용하려면 @EnableAsync를 main class에 붙여야함. (권장 X)

#### 권장하지 않는 이유
- 테스트 코드 작성이 어려움.
- 코드 복잡도 증가.
- 성능상 이득이 없음. 
    - DB 부하는 결국 같고.
    - 메인 쓰레드 대신 백드라운드 쓰레드가 일하는 정도의 차이.
    - 단, 백그라운드로 실행하고 결과를 받을 필요가 없는 작업이라면 @Async를 사용해서 응답 속도를 향상 시킬 수는 있다.
- 성능 최적화는...
    - SQL 데이터베이스에 쿼리하는 쿼리 갯수를 줄이고 필요로 하는 데이터를 필요한 만큼만 가져오는 게 최고의 성능 튜닝.

### 8. 스프링 데이터 Common : 커스텀 리포지토리 만들기

- 쿼리 메소드(쿼리 생성과 쿼리 찾아쓰기)로 해결이 되지 않는 경우 직접 코딩으로 구현 가능.
    - 기능 추가하기
        - 커스텀 리포지토리 인터페이스 정의 (POJO, 독립 적인 interface)
        - 인터페이스 구현 클래스 만들기 (기본 접미어는 Impl, 네이밍 컨벤션)
        - 엔티티 리포지토리에 커스텀 리포지토리 인터페이스 추가 (extends)
    - 기본 기능 덮어쓰기 (스프링 데이터 리포지토리 기본 기능 덮어쓰기 가능.)
        - spring data jpa가 제공하는 기본 기능이 마음에 안들 때.
        - 기본 기능 제공하는 메소드를 가져와서 커스텀 리포지토리(내가 만든 interface)에 재정의, 즉 붙여 넣는다. 
            - JPA Repository와 내가 만든 interface간에 중복이 발생한다. 하지만 스프링 데이터 jpa는 항상 내가 custom하게 구현한 구현체를 우선순위 높게 준다.
        - 구현 클래스 생성 및 엔티티 레포지토리에 커스텀 인터페이스 추가
    - 접미어 설정하기
        - SpringbootApplication class에 @EnableJpaRepositories(repositoryImplementationPostfix = "default") 어노테이션 설정, 기본 값은 impl
        - 커스텀 리포지토리 ex) PostCustomRepositoryimpl -> PostCustomRepositorydafault로 사용 가능.

### 9. 스프링 데이터 Common : 기본 리포지토리 커스터마이징

-` 모든 리포지토리에 공통적으로 추가하고 싶은 기능이 있거나 덮어쓰고 싶은(오버라이딩) 기본 기능이 있다면`
- 구현 방법
    - (1) JpaRepository를 상속 받는 인터페이스 정의
        - @NoRepositoryBean (중간에 사용되는 Repository는 꼭 추가 해줘야함.)
    - (2) 기본 구현체를 상속 받는 커스텀 구현체 만들기
    - (3) @EnableJpaRepositories에 설정
        - repositoryBaseClass

```java
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable> extends JpaRepository<T, ID> {

    boolean contains(T entity);

}
```

- SimpleJpaRepository : 스프링 데이터 JPA가 제공해주는 가장 밑단의 많은 기능을 가지고 있는 클래스

```java
public class SimpleMyRepository<T, ID extends Serializable> extends SimpleJpaRepository<T, ID> implements MyRepository<T, ID> {

    private EntityManager entityManager;

    // 생성자 추가 꼭 필요 (SimpleMyRepository super 호출 할 때 2개의 인자 전달 해줘야 하므로)
    public SimpleMyRepository(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager) {
        super(entityInformation, entityManager);
        this.entityManager = entityManager;
    }

    @Override
    public boolean contains(T entity) {
        return entityManager.contains(entity); // PersistContext에 있는지 확인만
    }
}
```

```java
@EnableJpaRepositories(repositoryBaseClass = SimpleMyRepository.class)
public interface PostRepository extends MyRepository<Post, Long> {
}
```

```java
// 상속받아서 사용
public interface PostRepository extends MyRepository<Post, Long> {
}
```