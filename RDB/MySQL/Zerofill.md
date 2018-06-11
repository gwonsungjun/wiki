# MySQL 데이터 타입 선언  INT()에서 괄호의 의미

- INT의 괄호는 보여지는 숫자의 개수의 제약을 의미하는 것이 아니다.
- INT의 괄호 옵션은 ZEROFILL을 위한 거다.
- ZEROFILL (이하 ZF) 속성이 지정됐을 경우에만 영향을 받기때문에 ZF 속성을 사용하지 않는다면 의미가 없다.
    - ZF의 기능은 입력된 값의 자릿수를 일관되게 맞추려는 목적으로 사용된다.
- 만약 괄호 안에 5를 집어넣었다면(INT(5)), 5자리 내의 숫자는 0으로 채워지게 된다.(괄호안의 숫자만큼 빈칸을 0으로 채움)
- ZF은 보여지는 형식을 정의 하는것이지 실제 저장되는 방식과는 무관하다.

## 테스트

- 테스트 테이블 생성
```sql
CREATE TABLE TEST(
seq INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
size5_fillzero INT(5) ZEROFILL, 
size5_nozero INT(5) , 
size9_fillzero INT(9) ZEROFILL, 
size9_nozero INT(9) 
);
```
- insert
```sql
INSERT INTO TEST
(size5_fillzero, size5_nozero, size9_fillzero, size9_nozero)
VALUES (1,1,1,1);
```
- 결과

![zerofill](/images/zerofill.PNG)

##


## Links
- <http://blackbull.tistory.com/44>
- <http://great-artist.tistory.com/84>