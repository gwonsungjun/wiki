# MySQL Database 생성 & 사용자 추가

## DB 생성
```SQL
1. $ mysql -u root -p : mysql 접속
2. mysql > create database testdb; : testdb db 생성
3. mysql > show databases; : database 목록 확인
```

## 사용자 생성
```SQL
mysql > create user 'testuser'@'localhost' identified by 'password'; : localhost 에서만 접속 가능한 사용자 생성
mysql > create user 'testuser'@'%' identified by 'password'; : 외부에서 접속 가능한 사용자 생성
```

## 사용자에게 DB 접속권한 부여
```SQL
mysql> grant all privileges on testdb.* to 'testuser'@'%' identified by 'pass'; : IP 관계 없이 id가 testuser
mysql> grant all privileges on nextdb.* to 'testuser'@'localhost'; : 해당 IP(localhost)에서 접속한 id가 testuser
```
