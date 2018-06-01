# MySQL Explain, 쿼리 정보 확인

## 1. Explain 이란?
- sql 의 select 구문을 어떻게 실행할지에 대한 계획을 보여주는 역할.
-  쿼리가 어떤 인덱스를 타는지, 몇개의 row를 대상으로 실행되는지, 어떤순서로 join 되는지등
다양한 정보를 제공.

## 2. Explain 사용법
```mysql
EXPLAIN
SELECT *
FROM stock_manage;
```
- SELECT 키워드 앞에 EXPLAIN을 입력.
- 아래는 위의 쿼리 실행 결과
![mysqlExplain](/images/mysqlExplain.PNG)
- 조인, 서브쿼리, union 등을 하게 되면 추가한 갯수 만큼 출력된다.
- 실행 결과(위의 사진)에 나열된 순서대로 실행된 것임.
- select 구문만 가능하다.

## 3. 각 컬럼별 세부 설명

### (1) id
- 쿼리 내의 select 구분 번호
- 쿼리 실행 순서라고 봐도 무방함.
- 다만 join 의 경우엔 하나의 구문에서 두개 이상의 테이블을 참조하기때문에 모든 테이블에 같은 순번이 부여됨.

### (2) select_type
- select 구문의 실행 타입.
- 총 10가지 존재.(version에 따라 다름)

| 속성값 | 내용 |
| ------- | ----- |
| SIMPLE | 단순 select 구문, 조인이나 서브쿼리가 없음.|
| PRIMARY | 서브쿼리를 이용할 경우 서브쿼리의 부모가 되는 select 쿼리, union을 사용할 경우 union 의 첫번째 select 쿼리, 즉 가장 외곽의 select |
| UNION	| union을 사용한 쿼리에서 첫번째를 제외한 나머지 select 쿼리(두번째 혹은 나중에 따라오는 select 문) | 
| DEPENDENT UNION | 바깥 쿼리에 의존성을 가진 union문의 두 번째 select 문 의미 | 
| UNION RESULT	| UNION 쿼리의 결과 | 
| UNCACHEABLE UNION	| UNION과 기본적으로 동일하나 공급되는 모든 값에 대해 UNION 쿼리를 재처리 | 
| SUBQUERY	| 서브쿼리의 첫 번째 select문 | 
| DEPENDENT SUBQUERY | SUBQUERY와 기본적으로 동일하나 가장 바깥의 select문에 '의존성'을 가진 서브쿼리의 select문 | 
| UNCACHEABLE SUBQUERY |  SUBQUERY와 기본적으로 동일하나 공급되는 모든 값에 대해 SUBQURTY를 재처리함, 외부쿼리에서 공급되는 값이 동일하더라도 캐쉬된 결과를 사용할수 없음.| 
| DERIVED | select로 추출된 테이블 즉, from절 내부의 쿼리 |

### (3) table
- 테이블 명, Alias를 사용할 경우 약칭

### (4) partitions
- 테이블의 파티션 중 어떤 파티션을 사용했는지 등의 정보를 조회

### (5) type
- 데이터를 호출한 타입. 

| 속성값 | 내용 |
|--------|------|
| system	| 테이블에 row가 1건이라 매칭되는 row도 1건인 경우 |
| const	 | 옵티마이저가 unique/primary key를 사용하여 매칭되는 row가 1건인 경우. (하나의 행만 매치) |
| eq_ref | 1:1의 join 관계, unique/primary key를 사용하여 join을 처리함.  즉, 이전 테이블에서 공급받은 값으로 조인 처리시 단 하나의row만이 조인되는 테이블에 존재하는 경우 |
| ref	| 1:n의 join 관계, join 처리시 unique/primary key를 100% 활용하지 못하거나 non-unique 인덱스가 사용됨, 단일 쿼리인 경우 where 조건절에서 non-unique 인덱스가 사용됨. 즉,  이전 테이블에서 공급받은 값으로 조인 처리시 하나 이상의row가 조인되는 테이블에 존재하는 경우 |
| ref_or_null |	 ref와 동일하나 null 값(IS NULL)에 대한 최적화가 되어있음.|
| fulltext	| fulltext 인덱스를 사용하여 데이터 Access (Only MyISAM) |
| index_merge | 동일한 테이블에서 두개 이상의 인덱스가 동시에 사용됨.(fulltext 인덱스는 제외), 인덱스 병합이 최적화된 타입. 이경우, key 컬럼은 사용된 인덱스의 리스트를 나타낸다. |
| unique_subquery | 서브쿼리에서 unique한 값이 생성되는 경우, index lookup function이 사용됨.(서브쿼리 최적화) |
| index_subquery |  unique_subquery와 비슷하나 결과값이 unique하지 않은 경우. |
| range	| 주어진 범위내의 row를 스캔함. 당연한 이야기지만 범위내의 row가 많으면 많을수록 성능이 저하됨. 키컬럼이 상수와 =, <>, >, >=, <, <=, IS NULL,<=>, BETWEEN 또는 IN 연산에 사용될 때 적용됨 |
| index	| 인덱스를 사용하긴 하나 전체 인덱스 block을 스캔함. 즉 인덱스를 사용하긴 하나 all 타입과 흡사함, MYSQL은 쿼리에서 단일 인덱스의 일부분인 컬럼을 사용할때 이 조인타입을 적용함. 일반적인 경우 인덱스가 테이블보다 사이즈가 작기 때문에, ALL보다는 빠를 가능성이 높음|
| all	|  전체 데이터 block을 스캔.(full table scan), 이전테이블과의 조인을 위해 풀스캔함.|

### (6) possible_keys
- 옵티마이저가 쿼리 처리를 위해 고려한 인덱스 후보. 즉 사용가능한 인덱스들의 리스트
- possible_keys와 key 값은 항상 같지 않다.

### (7) key
- 옵티마이저가 실제로 사용한 인덱스 키
- type값이 index_merge 일때 key 값은 사용된 모든 인덱스 키를 출력

### (8) key_len
- 옵티마이저가 사용한 인덱스 키의 길이 값.
- key_len 값으로 옵티마이저가 실제 복수 컬림키 중 얼마나 많은 부분을 사용할 것인지 알 수 있다.

### (9) ref
- 행을 추출하는데 키와 함께 사용된 컬럼이나 상수값.

### (10) rows
- 쿼리를 수행하기 위해 검색해야 할 row의 개수.
- 인덱스 조건을 최적화 해서 row의 개수를 줄이면 줄일수록 퍼포먼스가 향상됨.

### (11) extra
계속...

## Links
- [Mysql, explain - 쿼리정보 확인](http://www.dontorz.com/bbs/?mode=view&bbsid=study&ctg_cd=sql&bltn_seq=176)
- [MySQL Ver. 5.1 explain 사용법](http://mysqldba.tistory.com/162)
- [mysql explain 정보 보는방법](http://egloos.zum.com/laydios/v/1542611)