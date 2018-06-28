# MySQL 스토리지 엔진(Storage Engine)

- 스토리지 엔진 : 물리적 저장장치(DB)에서 데이터를 어떠한 방식으로 저장하고 읽어올지에 대한 기능(역할)
- 엔진 종류 마다 동작원리가 다름 따라서, 트랜잭션, 성능(안정적, 속도)과 같은 주요 이슈에도 밀접하게 연관
- MySQL의 스토리 엔진은 'Plug in' 방식
- 기본적으로 8가지의 스토리지 엔진이 탑재 되어 있다.
-  3rd Party 스토리지 엔진도 간단하게 플러그인 형식으로 설치를 할 수 있다. (INSTALL, UNINSTALL)
- 아래의 명령으로 탑재된 스토리지 엔진 확인 가능

```SQL
mysql > SHOW ENGINES;
```

![showEngines](/images/showEngines.PNG)

-  CREATE TABLE시, 가장 마지막 구문에 스토리지 엔진이 이름을 추가하여 설정할 수 있다.
- ENGINE=MyISAM, ENGINE=INNODB, ...
```SQL
CREATE TABLE test (  
    `test_id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
    `test_name` varchar(64) NOT NULL DEFAULT '') ENGINE = INNODB;
```
## Mysql 엔진 확인 명령

- 전체 테이블 조회
```SQL
SELECT * FROM information_schema.TABLES;
```
- 특정 테이블 조회
```SQL
SELECT ENGINE FROM information_schema.TABLES WHERE table_name='[tablename]' AND table_schema='[dbname]';
```
## 대표적인 엔진

### InnoDB Storage Engine
- default 로 설정되는 스토리지 엔진
- 트랜잭션 지원 O
- 커밋 & 롤백 (비정상 종료시 복구 기능)
- 동시 데이터 처리 시, 행 단위 잠금 (row-level locking) -> INSERT, UPDATE, DELETE에 대한 속도 빠름
- In-Memory : 메모리에 인덱스/데이터 모두 올려서 데이터를 처리하기 때문에 데이터 접근 속도가 상당히 빠름.

### MyISAM Storage Engine
-  트랜잭션 지원 X
-  table-level locking 제공 때문에 쓰기 작업(INSERT, UPDATE) 속도가 느리다.
- Multi-thread 환경에서는 성능 저하
- 인덱스만 메모리에 올려서 테이블 잠금으로 데이터를 처리하는 스토리지 엔진으로 단순 백그라운드에서 로그 수집에 적합함 (SELECT 작업)

## Links
- [MySQL, 주요 스토리지 엔진(Storage Engine) 간단 비교](http://asuraiv.blogspot.kr/2017/07/mysql-storage-engine.html)
