# MySQL Explain, 쿼리 정보 확인

## 1. Explain 이란?
- sql 의 select 구문을 어떻게 실행할지에 대한 계획을 보여주는 역할.
-  쿼리가 어떤 인덱스를 타는지, 몇개의 row를 대상으로 실행되는지, 어떤순서로 join 되는지등
다양한 정보를 제공.

## 2. Explain 사용법
```SQL
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
- 표의 각 라인(레코드)은 쿼리 문장에서 사용된 테이블의 개수만큼 출력된다(임시 테이블까지 포함). 실행 순서는 위에서 아래로 순서대로 표시된다.
- 출력된 실행 계획에서 위쪽에 출력된 결과일수록(id 칼럼의 값이 작을수록) 쿼리의 바깥(Outer)부분이거나 먼저 접근한 테이블이고, 아래쪽의 결과일수록 쿼리 안쪽 부분 또는 나중에 접근한 테이블에 해당된다.


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
- Table 컬럼에 `<derived>` 또는 `<union>`과 같이 "<>"로 둘러싸인 이름이 명시되는 경우가 많은데, 이 테이블은 임시 테이블을 의미. "<>"안에 항상 표시되는 숫자는 단위 SELECT 쿼리의 id를 지칭한다.

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
- 테이블에서 열을 선택하기 위해 key 컬럼 안에 명명되어 있는 인덱스를 어떤 컬럼 또는 상수(constant)와 비교하는지를 보여준다.

### (10) rows
- 쿼리를 수행하기 위해 검색해야 할 row의 개수.
- 인덱스 조건을 최적화 해서 row의 개수를 줄이면 줄일수록 퍼포먼스가 향상됨.

### (11) filtered
- MySQL 5.1.12에서 추가됨.
- 테이블 정의문이 필터링하는 테이블 열을 추정한 비율.
- 즉, rows는 조사된 열의 추정 숫자이며, rows × filtered / 100는 이전 테이블과 조인이 될 예정인 열의 숫자가 된다.

### (12) extra
- 옵티마이저가 쿼리를 해석한 추가적인 정보를 출력함.
- 성능 개선에 필요한 주요 정보 컬럼.

| 속성값 | 내용 |
|--------|------|
| const row not found | 대상 테이블의 row가 0건인 경우. |
| distinct	| distinct 쿼리를 수행하는 경우, 이미 처리한 값과 동일한 값을 가지는 row는 처리하지 않음. |
| Full scan on NULL key	| index lookup function이 사용되는 서브쿼리에서 외부에서 제공되는 값이 null인 경우. 이때 index lookup function 이 실패하여 full table scan이 발생할 수 있다. |
| impossible HAVING	| having 절이 항상 false인 경우. select 처리 하지 않는다.|
| impossible WHERE	| WHERE 절이 항상 false인 경우. select 처리 하지 않는다. |
| Impossible WHERE noticedafter reading const tables | const/system 타입의 테이블을 읽은 후 where 절이 항상 false인 경우.|
| No tables used | from 절에 테이블이 명시되지 않음. 혹은 from dual 구문을 사용함 |
| Not exists | left join 형태의 anti join 쿼리를 not exists 형태로 최적화 하는 경우. 조건에 맞는 결과를 찾으면 추가 join 하지 않음. |
| range checked for eachrecord	| 조인 처리시 적절한 인덱스가 없는 상황에서, 선행 테이블에서 공급되는 값에 따라 인덱스 사용을 검토할수 있는 경우. 공급되는 각각의 row에 range/index merge를 검토함. |
| Select tables optimizedaway | 쿼리가 Aggregate 함수만 포함하고 있는 경우.(MAX, MIN, SUM...), 옵티마이저는 인덱스 확인후 1개의 결과만을 리턴한다.|
| unique row not found	| 쿼리결과가 만족하는 row가 없는 경우.|
|Using filesort	| 옵티마이저가 정렬을 위해 추가적인 과정을 필요로 할 경우. (물리적인 정렬작업 수행)|
|Using index | 전체 데이터 block을 읽지 않고 인덱스 block만으로 결과를 생성할수 있는 경우. 컬럼정보가 실제 테이블이 아닌 인덱스트리에서 추출, 쿼리에서 단일 인덱스된 컬럼들만을 사용하는 경우|
|Using index for group-by	| 전체 데이터 block을 읽지 않고 인덱스 block만으로 group by/distinct를 처리.|
|Using join buffer	| join 처리시 join buffer가 사용되었음을 의미. join buffer는 join 처리를 위한 index가 없을때 사용된다. 즉 인덱스를 사용하지 않은 join을 의미함.|
| Using sort_union, Using union, Using intersect |index merge를 수행함. 두 개 이상의 인덱스를 동시에 사용.|
|Using temporary | 쿼리를 처리하기 위해 임시 테이블을 생성함. 만일 쿼리가 컬럼을 서로 다르게 목록화 하는 GROUP BY 및 ORDER BY 구문을 가지고 있는 경우에 이런 것이 일어나게 된다.|
|Using where | where절이 다음 조인에 사용될 row나 출력될 row의 개수를 제한하는 경우.|

## 추가
### (1) sql_no_cache
- 한번 select하면 메모리에 올려져 있으므로 같은 퀴리로 계속 호출하면 메모리에 올라와 있는것을 읽으므로 속도가 개선된것처럼 착각을 할수 있다.
 - 이럴땐 퀴리에 sql_no_cache 를 붙여서 캐쉬를 사용하지 않으면 된다.
 - ex) SELECT SQL_NO_CACHE A.* FORM A;

### (2) Derived table
 - Derived table= 쿼리 실행 시 내부 작업으로 인해 임시적으로 생성되는 서브쿼리 (즉, 쿼리의 from 절에 서브쿼리로 만들어진 임시 테이블)

 ### (3) 인덱스 사용 팁
- 인덱스의 갯수는 3~4개 정도가 적당하다.
- 너무 많은 인덱스는 새로운 Row를 등록할때마다 인덱스를 추가해야하고, 수정/삭제시마다 인덱스 수정이 필요하여 성능상 이슈가 있다.
- 인덱스 역시 공간을 차지한다. 많은 인덱스들은 그만큼 많은 공간을 차지한다.
- 특히 많은 인덱스들로 인해 옵티마이저가 잘못된 인덱스를 선택할 확률이 높다.
- 인덱스의 키는 길면 길수록 성능상 이슈가 있다.
- 1개의 컬럼만 인덱스를 걸어야 한다면, 해당 컬럼은 카디널리티(Cardinality)가 가장 높은 것을 잡아야 한다는 점.
- 즉, 여러 컬럼으로 인덱스를 잡는다면 카디널리티가 높은순에서 낮은순으로 (group_no, from_date, is_bonus) 구성하는게 더 성능이 뛰어나다.
- 조회 쿼리 사용시 인덱스를 태우려면 최소한 첫번째 인덱스 조건은 조회조건에 포함되어야만 한다.
- 첫번째 인덱스 컬럼이 조회 쿼리에 없으면 인덱스를 타지 않는다는 점을 기억.

## Links
- [Mysql, explain - 쿼리정보 확인](http://www.dontorz.com/bbs/?mode=view&bbsid=study&ctg_cd=sql&bltn_seq=176)
- [MySQL Ver. 5.1 explain 사용법](http://mysqldba.tistory.com/162)
- [mysql explain 정보 보는방법](http://egloos.zum.com/laydios/v/1542611)
- [MySQL Query Plan 보는 법](http://blog.naver.com/PostView.nhn?blogId=realuv&logNo=10184319823)
- [Real MySQL [6-2] 실행계획 - table, type 칼럼](http://weicomes.tistory.com/153)
- [MySQL 인덱스 정리 및 팁](http://jojoldu.tistory.com/243)
