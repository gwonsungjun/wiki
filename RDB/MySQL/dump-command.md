# MySQL Dump / Import

## Dump
```mysql
$ mysqldump -u [사용자 아이디] -p [데이터베이스명] > 저장된 파일명.sql : db 전체 dump
ex) mysqldump -u root -p test > test.sql
$ mysqldump -u [사용자 아이디] -p [데이터베이스명] [테이블명] > 저장될 파일명.sql : 특정 테이블만 dump
ex) mysqldump -u root -p test user > test_user.sql
```

## Import
```mysql
$ mysql -u [사용자 아이디] -p [데이터베이스명] < 덤프 파일명.sql
ex) mysql -u root -p test < test.sql
```