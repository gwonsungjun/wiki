# Nginx
- Nginx는 차세대 웹서버로 불린다.
- Nginx를 한 마디로 정의하면 `더 적은 자원으로 더 빠르게 데이터를 서비스 할 수 있다.`
- Apache는 웹의 산증인이라고 해도 과언이 아니지만 오래전에 만들어진 소프트웨어이고 아파치가 만들어진 시대의 요구사항이 이제는 유효하지 않은 것도 있다.
- 이에반해, Nginx는 새로운 시대의 요청에 부흥해서 만들어진 웹서버이다.
- 개발의 모든 목적이 높은 성능에 맞춰져 있고 잘 사용하지 않는 기능은 과감하게 제외 했다.
- NGINX는 웹서버다. 웹서버는 인터넷 네트워크 위에서 HTTP 프로토콜을 이용해 HTML, CSS, JavaScript, 이미지와 같은 정보를 웹브라우저에게 전송한다. 

## Ubuntu 18.04 Nginx Install 

```shell
$ sudo apt-get update
$ sudo apt-get install nginx
```

### Document Root
- `/usr/share/nginx/html/`

### 설정 파일 위치 (apt-get을 이용해 설치한 경우의 위치)
- `/etc/nginx/`
- `$ sudo find / -name nginx.conf` 명령을 통해 설정 파일의 위치를 찾을 수 있음.

### 로그 파일 위치
- `/var/log/nginx`

## 사용자
- NGINX는 마스터 프로세스(Master Process)와 작업자 프로세스(Worker Process)를 가지고 있다.
- `마스터 프로세스`는 루트 계정으로 실행되면서 80, 443 포트의 소켓과의 통신을 담당한다. 
- `작업자 프로세스`는 실제로 데이터를 처리하는 프로세스라고 할 수 있는데  일반적으로 웹서버의 워커 유저는 www-data를 사용한다. 