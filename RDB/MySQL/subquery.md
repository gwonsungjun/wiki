# 서브쿼리(Sub Query)

- SELECT 명령에 의한 데이터 질의로, 상부가 아닌 하부의 부수적인 질의를 의미
- SQL 명령문 안에 지정하는 하부 SELECT 명령으로 괄호를 묶어 지정
- `(SELECT 명령)`

## DELETE의 WHERE 구에서 서브쿼리 사용하기
- `DELETE FROM sample WHERE a = (SELECT MIN(a) FROM sample);`
- 클라이언트 변수(Mysql 클라이언트에 한해 구현 가능)
  - @a : 변수, set : 변수에 대입하는 명령
  - `mysql > set @a = (SELECT MIN(a) FROM sample);`
  - `mysql > DELETE FROM sample WHERE a = @a;`

## 스칼라 값
- SELECT 명령이 하나의 값만 반환하는 것을 '스칼라 값을 반환한다'라고 한다.
- = 연산자를 사용하여 비교할 경우에는 스칼라 값끼리 비교할 필요가 있다.

## SELECT 구에서 서브쿼리 사용하기
- SELECT 구에서 서브쿼리를 지정할 때는 스칼라 서브쿼리가 필요
```SQL
SELECT  
  (SELECT COUNT(*) FROM sample1)AS sq1,
  (SELECT COUNT(*) FROM sample2)AS sq2;
```

## SET 구에서 서브쿼리 사용하기
- `UPDATE sample SET a=(SELECT MAX(a) FROM sample);`
- SET 구에서 서브쿼리를 사용할 경우에도 스칼라 값을 반환하도록 스칼라 서브쿼리를 지정할 필요가 있다.

## FROM 구에서 서브쿼리 사용하기
- FROM 구에 기술할 경우에는 스칼라 값을 반환하지 않아도 좋다.
- `SELECT * FROM (SELECT * FROM sample) sq;`
- SELECT 명령 안에 SELECT 명령
  - 이를 '네스티드(nested)구조 또는 중첩구조나 내포구조라 부른다.'

## INSERT 명령과 서브쿼리
- INSERT 명령에는 VALUES 구의 일부로 서브쿼리를 사용하는 경우와, VALUES 구 대신 SELECT 명령을 사용하는 두 가지 방법이 있다.
- VALUES 구에서 서브쿼리 사용하기.
  - 서브쿼리는 스칼라 서브쿼리 지정.
  - 자료형도 일치해야 함.
```SQL
INSERT INTO sample VALUES(
  (SELECT COUNT(*) FROM sample1),
  (SELECT COUNT(*) FROM sample2)
);
```
- INSERT SELECT (SELECT 결과를 INSERT)
  - VALUES 구 대신 SELECT 명령 사용.
  - SELECT가 반환하는 열 수와 자료형이 INSERT할 테이블과 일치하기만 하면됨. ( SELECT 반환 값이 꼭 스칼라 값일 필요x)
  - 데이터의 복사나 이동을 할 때 자주 사용하는 명령
    - 열 구성이 똑같은 테이블 사이에
```SQL
INSERT INTO sample SELECT 1, 2;
= INSERT INTO sample VALUES(1,2);
```

# 상관 서브쿼리
## EXISTS
- `EXISTS(SELECT 명령)`
- 서브쿼리가 반환하는 결과값이 있는지 조사할 수 있다.
- 단지 반환된 행이 있는지 확인만 하므로 스칼라 값일 필요 없다.
- 반환하는 행이 있으면 참, 없을 경우에는 거짓
```SQL
UPDATE sample1 SET a = '있음' WHERE
  EXISTS (SLECT * FROM sample2 WHERE no2 = no);
```

## NOT EXISTS
- 행이 존재하지 않는 상태가 참.
```SQL
UPDATE sample1 SET a = '없음' WHERE
  NOT EXISTS (SLECT * FROM sample2 WHERE no2 = no);
```
- SELECT, DELETE 명령으로도 사용 가능

## 상관 서브쿼리
- 위의 예처럼 부모(UPDATE 명령)와 자식(서브쿼리)이 특정 관계를 맺는 것을 상관 서브쿼리라고 부른다.
- 상관 서브쿼리를 사용함으로써 두 테이블에 걸쳐 조작할 수 있다.
- 부모 명령과 연관되어 처리되기 때문에 (위의 WHERE no2 = no) 서브쿼리 부분만을 따로 떼어내어 실행 시킬 수 없다.

## IN
- 집합 안의 값이 존재하는지 조사할 수 있다.
- `열명 IN(집합)`
```SQL
SELECT * FROM sample WHERE no IN (3, 5);
```
```SQL
SELECT * FROM sample1 WHERE no IN
  (SELECT no2 FROM sample2);
```
- 집합 안에 값이 포함되어 있으면 참.
- 반면 NOT IN으로 지정하면 집합에 값이 포함되어 있지 않을 경우 참이 된다.
- IN에서는 집합안에 NULL 값이 있어도 무시하지 안흔ㄴ다.
- 다만 NULL=NULL을 제대로 계산할 수 없으므로 NULL 값은 비교할 수 없다.
- 즉 NULL을 비교할 때는 `IS NULL`을 사용해야 한다.

## Links
- [SQL 첫걸음](https://book.naver.com/bookdb/book_detail.nhn?bid=9738902)
