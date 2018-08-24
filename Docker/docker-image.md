# 도커 이미지 만들기
- 컨테이너를 생성하고 해당 컨테이너에 접속해서 필요한 패키지들을 설치하는 이런 복잡한 작업을 사용한다면 도커를 사용하지 않을 것이다.
- 도커를 사용하면 애플리케이션에 필요한 개발 환경과 애플리케이션을 저장한 채로 이미지를 빌드할 수 있고, 빌드된 이미지는 플랫폼에 구애받지 않고 도커 환경이라면 배포할 수 있다. 따라서 다들 도커를 사용하는 것이다.
- `도커는 이미지를 생성하기 위해 컨테이너 상태를 그대로 이미지로 저장한다.`

## 도커 이미지 빌드(build)
1. 완성한 컨테이너를 $ docker commit 명령으로 이미지로 만든다.
    - 이 방법은 소스 배포 등으로 컨테이너 내부 파일이 변경돼 이미지를 새로 빌드해야 할 때 사용한다.
2. dockerfile을 작성 후 이미지로 빌드한다.
    - 이 방법은 처음 컨테이너를 만들어 운영 혹은 개발 환경을 정의할 때 주로 이용한다.

### docker commit을 이용한 이미지 빌드
- 컨테이너 내 변경사항을 이미지 파일로 만드는 명령
- `$ docker commit -a "작성자 명" -m "메시지 내용" container_name inmage_nmae:TAG`

```shell
$ docker commit -a "sjkwon" -m "demo" web-demo web-demo:0.1
```

- docker commit 명령 옵션

옵션 | 설명
---- | ----
-a | --author="", 작업자 정보를 기입
-m | --message="", 이미지의 메시지
-p | --pause=ture/false 이미지를 생성할 때 컨테이너를 중지(stop)한 뒤 커밋할 것인지에 대한 정의. 기본 값은 false, 즉 중지하지 않고 이미지 생성

- `$ docker images`- 생성된 이미지 확인

### Dockerfile을 이용한 이미지 빌드
- 호스트 OS에서 `Dokerfile`(이미지 빌드용 DSL file)을 생성한다.

```shell
$ vi ~/Dockerfile
```

- Dockerfile 작성법은 [링크](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) 참조
    - 도커 빌드 중엔 키보드를 입력할 수 없기 때문에 `-y` 옵션 추가
- 작성된 Dockerfile build
    - `docker build [OPTIONS] {image_tag:version} {Dockerfile 위치}`

```shell
$ docker build -t web-demo:0.1 .
```

- `$ docker images`- 생성된 이미지 확인

#### Dockerfile 기본 명령어

##### FROM

```shell
FROM <image>:<tag>
FROM ubuntu:16.04
```

- 베이지 이미지 지정. (반드시)
- tag는 lastest보다 구체적인 버전 지정이 좋다.
- 이미지가 없을 경우 자동으로 이미지 pull

##### MAINTAINER

```shell
MAINTAINER <name>
MAINTAINER sungjun@sungjun.com
```

- Dockerfile을 관리하는 사람의 이름 또는 이메일 정보 적음

##### COPY

```shell
COPY <복사할 파일 경로> <이미지에 저장될 파일 위치>
COPY . /usr/src/app
```

- 파일이나 디렉토리를 이미지로 복사
- 일반적으로 소스를 복사하는데 사용
- target 디렉토리가 없드면 자동 생성
- 주의할 점은 복사할 파일 경로는 Dockerfile 경로를 벗어날 수 없다는 점


##### ADD

```shell
ADD <복사할 파일 경로> <이미지에 저장될 파일 위치>
ADD . /usr/src/app
```

- 파일을 이미지에 추가.
- COPY와 유사하나 차이점은 복사할 파일 경로에 파일 대신 URL을 입력할 수 있고 복사할 파일 경로에 압축 파일을 입력하는 경우 자동으로 압축을 해제하면서 복사된다.

##### RUN

```shell
RUN <command>
RUN  apt-get update && apt-get install -y default-jdk
```

- 명령어 실행
- 내부적으로 /bin/sh -c 뒤에 명령어를 실행하는 방식
- 주의할 점은 Y/N 등 대화 형태로 동작하게 되면 에러가 발생
- 따라서 명령에 -y 옵션 넣어줘야함.

##### CMD

```shell
CMD ["executable","param1","param2"]
CMD command param1 param2
CMD bundle exec ruby app.rb
```

- 컨테이너가 시작될 때마다 실행할 명령어를 정의
- Dockerfile에서 한 번만 사용할 수 있다.
- 빌드할 때는 실행되지 않으며 여러 개의 CMD 가 존재할 경우 가장 마지막 CM만 실행.

##### WORKDIR

```shell
WORKDIR /path/to/workdir
```

- bash sell의 `cd`명령과 같다.
- 작업할 위치를 정의.
- 여러번 사용 가능
- `RUN cd /path`명령은 다음 명령어에서 위치가 초기화 되지만 WORKDIR의 경우 초기화 되지 않음

##### EXPOSE

```shell
EXPOSE <port>
EXPOSE 8080
```

- 이미지에서 노출할 포트 설정
- 이 옵션은 단지 컨테이너의 해당 포트를 사용하겠다고 선언하는 것이지 호스트와 바인딩 되는 것은 아니다.
- 호스트와 바인드 하기 위해서는 `$docker run -p 80:80` 등의 명령으로 run할때 -p 옵션으로 정의해야 한다.

##### ENV

```shell
ENV <key> <value>
ENV <key>=<value> ...
ENV DB_URL mysql
```

- 컨테이너에서 사용할 환경변수 지정

## 도커 이미지 PUSH

- 이미지 저장소
    - [도커 레지스트리(Docker Registry)](https://docs.docker.com/registry/) : 설치형
    - [도커 허브](https://hub.docker.com/)
- 도커 허브 로그인
    - `$ docker login`
    - `~/.docker/config.json`에 인증정보 저장됨
- 도커 허브에서 리포지토리(repository) 생성
    - create repository (알맞은 name으로)
- 도커 tag 명령을 통해 기존에 만든 이미지에 추가로 이름 지어줌
    - `docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`
    - 이미지 이름을 `DockerHubID/DockerHubReponame:TAG`로 정의해야 도커 허브에 PUSH가 된다.
```shell
$ docker tag web-demo:0.1 sungjun/sungjun-app:1
```

- PUSH 명령을 이용해 도커 허브에 이미지 전송

```shell
$ docker push sungjun/sungjun-app:1
```

- 내려 받을 경우 `$ docker pull sungjun/sungjun-app:1`

## Links
- [초보를 위한 도커 안내서](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)
- [모두의 Docker](https://realhanbit.co.kr/books/226/pages/2201/read)