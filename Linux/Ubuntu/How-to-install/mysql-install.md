# Ubuntu Mysql Server Install

## 1.Mysql 서버 설치
- &#35; apt-get update
- &#35; apt-get install mysql-server
- root 계정 패스워드 설정 화면이 나오면 암호 입력 후 계속해서 설치

## 2.Mysql 서버 시작과 종료
- &#35; service mysql start
- &#35; service mysql stop

## 3.확인
- &#35; /etc/init.d/mysql status
- &#35; dpkg --list | grep mysql
- &#35; netstat -ntlp | grep mysqld

## 4.Mysql 서버 접속
- &#35; mysql -u root -p

## Link
- [MySQL Ubuntu 16.04 에서 MySQL 서버 설치](https://www.fun25.co.kr/blog/ubuntu-16-04-mysql-server-install/)
- [우분투 MySQL 설치](https://zetawiki.com/wiki/%EC%9A%B0%EB%B6%84%ED%88%AC_MySQL_%EC%84%A4%EC%B9%98)
