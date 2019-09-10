# MySQL Dump / Import

## Dump

### 특정 데이터베이스 dump

```sql
$ mysqldump -u [사용자 아이디] -p [데이터베이스명] > 파일명.sql
ex) mysqldump -u root -p test > test.sql
```

### 데이터베이스의 특정 테이블 dump

```sql
$ mysqldump -u [사용자 아이디] -p [데이터베이스명] [테이블명] > 파일명.sql
ex) mysqldump -u root -p test user > test_user.sql
```

### 전체 데이터베이스 dump

```sql
$ mysqldump -u [사용자 아이디] -p --all-databases > 파일명.sql
또는 $ mysqldump -u [사용자 아이디] -p -A > 파일명.sql
```

### Error
-  `Unkown table ‘COLUMN_STATISTICS’ in information_schema`
    - mysql 8 부터 추가된 옵션때문에 발생하는 에러.
    - `--column-statistics=0` 옵션을 추가해줘야 한다.

```sql
ex) $ mysqldump --all-databases -h test.read.sungjun.com -u user -p --column-statistics=0 > backup.sql
```

## Import

### 특정 데이터베이스 import

```SQL
$ mysql -u [사용자 아이디] -p [데이터베이스명] < 덤프 파일명.sql
ex) mysql -u root -p test < test.sql
```

### 전체 데이터베이스 import
- 전체 데이터베이스를 dump 한 경우

```SQL
$ mysql -u [사용자 아이디] -p < 덤프 파일명.sql
ex) mysql -u root -p < all_db.sql
```
