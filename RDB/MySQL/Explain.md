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
- 실행 결과에 나열된 순서대로 실행된 것임.
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


## Links
- [Mysql, explain - 쿼리정보 확인](http://www.dontorz.com/bbs/?mode=view&bbsid=study&ctg_cd=sql&bltn_seq=176)
- [MySQL Ver. 5.1 explain 사용법](http://mysqldba.tistory.com/162)