# Spring Transaction

## Read Only, 읽기 전용 트랜잭션

- Read Only가 설정된 해당 메서드는 오로지 읽기에만 사용된다.
- boolean 값을 설정하며, 기본값은 false.
- INSERT나 UPDATE, DELETE문은 허용되지 않는다. 만약 쓰기나 삭제가 실행될 경우 에러를 발생시킨다.
- **@Transactional(readOnly=true)** 는 JDBC의 Connection.setReadOnly(true)를 호출한다. 이는 단지 “힌트”일 뿐이며 실제 데이터베이스 자체를 read-only 트랜잭션으로 생성할지 여부는 JDBC 드라이버에 따라 달라지고 행위도 데이터 계층 프레임워크의 구현여부에 따라 달라질 수 있다.
- Oracle JDBC 드라이버는 Read-Only 트랜잭션을 지원하며, MySQL의 경우 5.6.5 버전이 되어서야 이를 지원하게 되었다.
- **읽기 작업만 하더라도 트랜잭션을 걸어주는 것이 좋다. 트랜잭션을 걸지 않으면 모든 SELECT 쿼리마다 commit을 하기 때문에 성능이 떨어진다. 명시적으로 트랜잭션을 걸어주면 마지막에 명시적으로 commit을 해주면 되며, commit 횟수가 줄어서 성능이 좋아진다.**

## Links
- [권남:Spring Transaction](http://kwonnam.pe.kr/wiki/springframework/transaction)
