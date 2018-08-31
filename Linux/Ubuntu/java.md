# Java 명령어, 옵션 정리

## 1. Java 버전

- java 버전 확인 : `$ java -version`
- 설치된 java list 확인 : `$ update-java-alternatives --list`
- java 버전 변경 : `$ sudo update-alternatives --config java` 입력 후 해당 java version의 번호 입력.

## 2. JAVA_HOME, JRE_HOME 설정

- `$ sudo vi /etc/profile`
- 최하단에 아래 export 문 삽입
- SSH 재접속 후 확인 = `$ echo $JAVA_HOME`

```shell
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
```