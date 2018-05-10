# curl 사용법

## 1. curl 이란?
- curl = Client URL
- 다양한 프로토콜로 데이터를 전송해볼 수 있는 명령어 기반의 컴퓨터 프로그램
- 클라이언트에서 커맨드 라인이나 소스코드로 손 쉽게 웹 브라우저 처럼 활동할 수 있도록 해주는 기술(커맨드라인 Tool 혹은 라이브러리)
- cURL은 아래와 같은 프로토콜 등에 의해 전송되는 파일들을 위한 command line tool이다. (다양한 프로토콜 지원)
- HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, TELNET, DICT, LDAP, LDAPS, FILE, SSL, HTTP POST, HTTP PUT, FTP uploading, HTTP form기반의 upload, proxies, cookies, user+password 인증도 제공한다.
- cURL은 무료이며, 많은 운영체제에서 컴파일되고 동작하는 open software이다.
- libcurl은 일반 개발자가 많이 쓰는 HTTP, FTP를 시작해 매우 많은 프로토콜을 지원하는 무료 라이브러리다.
- **url을 가지고 할 수 있는 것들은 다할 수 있다.** 예를 들면, http 프로토콜을 이용해 웹 페이지의 소스를 가져온다거나 파일을 다운받을 수 있다. ftp 프로토콜을 이용해서는 파일을 받을 수 있을 뿐 아니라 올릴 수도 있다. 심지어 SMTP 프로토콜을 이용하면 메일도 보낼 수 있다

## 2. 사용법
- curl [-option] http://gwonsungju.github.io 하게되면 소스가 화면으로 출력된다.
- 이외에 옵션을 이용하여 사용할 수 있다.

## 3. 주요 옵션
- -O : 파일 다운로드 > curl -O http://gwonsungju.github.io/file.zip
- -i : 헤더값 확인 > curl -i http://gwonsungjun.github.io
- -I : 사이트의 Header와 바디 정보를 함께 가져오기
- -v : request, response 어떻게 오가는지 확인.(헤더, 바디)
- -k : https 사이트를 SSL certificate 검증없이 연결.
- -s : 정숙 모드. 진행 내역이나 메시지등을 출력하지 않음.
- -H : 헤더 설정. 헤더 정보 전달
- -d : HTTP Post data (FORM 을 POST 하는 HTTP나 JSON 으로 데이타를 주고받는 REST 기반의 웹서비스 디버깅시 유용한 옵션)

## 4. 같이 읽어보면 좋은 자료
- [커맨드라인 환경에서 REST API (HTTP) 요청 보내기 (cURL, resty, httpie, Vim REST Control)](https://bakyeono.net/post/2016-05-02-rest-api-client-for-cli.html)
- [cURL!](http://khanrc.tistory.com/entry/cURL)


## 5. Links
- [Curl이란?](http://jokergt.tistory.com/83)  
- [cURL](http://sunphiz.me/wp/archives/491)
- [curl 설치 및 사용법 - HTTP GET/POST, REST API 연계등](https://www.lesstif.com/pages/viewpage.action?pageId=14745703)
