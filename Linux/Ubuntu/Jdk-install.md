# Ubuntu jdk 설치 방법

## 1.Open jdk 설치
- $ sudo apt-get update
- java 설치유무 확인, $ java-version
- `$ sudo apt-get install openjdk-8-jdk`
- default-jdk 설치시 기본적으로 openjdk와 동일, $ sudo apt-get install default-jdk 

## 2.Oracle jdk 설치
- java 설치유무 확인, $ java-version
- $ sudo add-apt-repository ppa:webupd8team/java
- $ sudo apt-get update
- $ sudo apt-get install oracle-java8-installer

## 3. 수동설치(tar)

## 에러 발생 시
- oracle jdk가 설치되어 있는 상태에서 open jdk를 설치하려고 했을때 `note, selecting 'oracle-java8-installer' instead of 'openjdk-8-jdk'`,  `oracle-java8-installer is already the newest version` 메세지가 뜨며 설치가 안 되면 아래와 같이 repository를 등록 해주고 다시 설치하면 된다.

```shell
sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update   
sudo apt-get install openjdk-8-jdk  
```

## Oracle JDK와 Open JDK 차이
Oracle JDK는 OpenJDK의 JDK7 기반에 추가로 OpenJDK에 포함되지 않은 Component까지 모두 갖춘 프로젝트이다.

둘 간의 큰 차이라면 Oracle JDK는 OpenJDK에는 없는 재산권이 걸린 플러그인을 제공한다. 해당 플러그인은 Oracle이 재산권을 보유하고 있다.

Vendor에 의한 분리된 Version이 존재

- Oracle’s JDK (Commertial support from oracle) - 폐쇄적인 상업 코드 기반
- OpenJDK, the open source java - 오픈 소스 기반

JDK7 이전에는 두 Version간 큰 차이가 존재해 OpenJDK는 Oracle JDK에 비해 누락된 기능 및 성능이슈가 존재 했으나 현재는 java-web-plugin(<http://en.wikipedia.org/wiki/IcedTea> - 저작권이 있는 라이브러리의 대안으로 작성된)을 제외하고는 정확하게 같다고 볼 수 있다. 몇몇 사람들은 아직도 OpenJDK가 Oracle JDK에 비해 성능이 떨어진다고 하지만, 이것은 근거없는 말이다.

## Link
<https://www.holaxprogramming.com/2014/09/24/java-open-jdk/>
<http://jsonobject.tistory.com/395>
