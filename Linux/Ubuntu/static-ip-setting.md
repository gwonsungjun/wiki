# Ubuntu 고정 ip 설정법

## 1. 네트워크 설정 파일 실행
- sudo vi /etc/network/interfaces

## 2. 고정 ip로 수정
- 처음에 동적 ip로 설치했다면 아래와 같이 dhcp로 설정 되어 있음.
![intellij_keymap_problem](/images/ipDhcp.PNG)
- 네트환경에 맞게 아래 그림과 같이 수정을 하고 저장한다.
![intellij_keymap_problem](/images/ipStatic.PNG)

## 3. 네트워크 재시작 또는 재부팅
- systemctl restart networking.service
- 위의 명령이 가끔 제대로 동작하지 않는 경우가 있다.
- 그때는 if-down, if-up 명령을 이용하면 적용되는 것을 볼 수 있음

## 4. Links
-[Ubuntu 16.04 | 고정 IP 설정하는 방법]( https://www.manualfactory.net/10108)