# Ubuntu Desktop

## Ubuntu Install

- 16.04 LTS 설치 진행. (18.04 설치 하려 했으나 보류)
- 부팅 USB 생성
    - 기존 우분투 데스크탑에서 unetbootin 사용
    - `$ sudo apt-get install unetbootin` , 사용법 [참조](http://pinkwink.kr/871)

## Program Install
1. sshd 
    - `$ sudo apt-get install openssh-server`
1. JDK
    - `$ sudo apt-get install openjdk-8-jdk`
2. Git
    - `$ sudo apt-get install git-core`
3. Chrome

```shell
$ sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb 
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```
4. VSCode
    - [설치 및 환경 설정](https://gwonsungjun.github.io/vscode/2018/06/13/vscodeSetting/)


## Links
- [Ubuntu 크롬 설치](http://snowdeer.github.io/linux/2018/02/02/ubuntu-16p04-install-chrome/)