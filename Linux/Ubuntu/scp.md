# scp

## scp란?
- scp란 Secure Copy의 약자로 로컬서버에서 리모트(원격)서버로 파일을 복사해서 올리거나 내려받을 때 사용하는 unix계열 utility이다.

## 사용법

### 2대의 server 준비
- 서버 1 : 192.168.10.10, username : test1
- 서버 2 : 192.168.10.20, username : test2
- 서버 1 -> 서버 2 전송

### 파일 복사
- scp [복사하려는 파일]  [서버2 username]@[서버2 ip address]:[서버2 복사 될 위치]

```shell
$ scp  ./debug.log  test2@192.168.10.20:/home/test2
```

### 폴더 복사
- scp [복사하려는 폴더]  [서버2 username]@[서버2 ip address]:[서버2 복사 될 위치]

```shell
$ scp  ./logbackupFolder test2@192.168.10.20:/home/test2
```

## Links
- <http://ngee.tistory.com/264>