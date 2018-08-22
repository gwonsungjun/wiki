# 도커 기본 명령어
- 사용 빈도수 높은 도커 기본 명령어 정리.

## 도커 서비스 시작 (ubntu)
```shell
$ sudo service docker start
```

## 도커 버전 확인
```shell
$ docker version
```

## 도커 허브 로그인
```shell
$ docker login 
(input : username, password)
```

## 이미지 검색
- ubuntu 검색

```shell
$ docker search ubuntu
```

## 검색된 이미지 다운로드 : pull (로컬에 저장)

```shell
$ docker pull <Image_name:TAG>
$ docker pull ubuntu:16.04
```

## 로컬에 저장된 이미지 확인
- 기본 이미지 저장 경로 `/var/lib/docker/`
- `$ docker info | grep 'Docker Root Dir'`명령으로 확인 가능

```shell
$ docker images
```

## 이미지 삭제하기
- 컨테이너가 실행중인 이미지는 삭제되지 않는다.
- `docker rmi [OPTIONS] IMAGE [IMAGE...]`

```shell
$ docker rmi ${image_id}
```

## 컨테이너 생성 
- run 명령을 사용하면 사용할 이미지가 있는지 확인하고 없다면 다운로드(pull)를 한 후 컨테이너를 생성(create)하고 시작(start)한다.
- `docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`
- 아래 명령은 우분투 컨테이너가 hello를 실행하고 종료하는 명령

```shell
$ docker run ubuntu:16.04
$ docker run -it ubuntu:16.04 /bin/echo hello
hello
$ docker run -d -p 1234:6379 redis
```

옵션 |	설명
---- | -----
-d	| detached mode 흔히 말하는 백그라운드 모드
-p	|호스트와 컨테이너의 포트를 연결 (포워딩)
-v	| 호스트와 컨테이너의 디렉토리를 연결 (마운트)
-e	| 컨테이너 내에서 사용할 환경변수 설정
–name	| 컨테이너 이름 설정
–rm	| 프로세스 종료시 컨테이너 자동 제거
-it	| -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
–link	|컨테이너 연결 [컨테이너명:별칭]

## 컨테이너 목록 확인
- 옵션이 없으면 실행중인 컨테이너 목록 확인
- `-a`옵션은 정지된 컨테이너도 함께 볼 수 있다.

```shell
$ docker ps
$ docker ps -a
```

## 컨테이너 중지
- `docker stop [OPTIONS] CONTAINER [CONTAINER...]`
- 중지하려면 컨테이너의 ID 또는 이름을 입력
- `도커의 ID 전체 길이는 62자리, 하지만 연명어의 인자로 전달할 때는 전부 입력하지 않아도 된다. 앞부분이 겹치지 않는 다면 1-2자만 입력해도 된다.`

```shell
$ docker stop ${CONTAINER_ID}
```

## 컨테이너 제거
- `docker rm [OPTIONS] CONTAINER [CONTAINER...]`
- 호스트 OS는 아무런 흔적도 남아있지 않고 컨테이너만 격리된 상태로 실행되었다가 삭제됨. 시스템이 꼬일 걱정이 없다.

```shell
$ docker rm ${CONTAINER_ID}
```

- 중지된 컨테이너를 일일이 삭제 하는 건 귀찮은 일이다. `docker rm -v $(docker ps -a -q -f status=exited)` 명령어를 입력하면 중지된 컨테이너 ID를 가져와서 한번에 삭제한다.


## 컨테이너 중지하지 않고 빠져 나오는 단축키
- `Ctrl + P + Q`

## 다시 컨테이너 내부로 들어가기

```shell
$ docker attach [컨테이너 이름]
= $ docker exec -it [컨테이너이름] /bin/bash
```

## 컨테이너 로그 보기
- `docker logs [OPTIONS] CONTAINER`
- 옵션 : -f, --tail
- 도커가 로그파일을 자동으로 알아채는 것이 아님. 컨테이너에서 실행되는 프로그램의 로그 설정을 파일이 아닌 표준 출력(stdout, stderr)으로 바꾸어야 한다.
- 또한, 컨테이너는 로그파일을 json 방식으로 어딘가에 저장한다. 로그가 많으면 파일이 차지하는 용량이 커지므로 주의!
- 앱의 규모가 커지면 특정 로그 서비스를 이용하는 것을 고려.

```shell
$ docker logs ${CONTAINER_ID}
$ docker logs --tail 10 ${CONTAINER_ID}
$ docker logs -f ${CONTAINER_ID}
```

## 컨테이너 명령어 실행하기
- 실행중인 컨테이너에 명령을 내린다.

```shell
$ docker exec -it mysql mysql -uroot

mysql > 
```

- 호스트 OS에 Mysql을 설치하지 않아도 mysql 클라이언트를 사용할 수 있게 되었다!
- 굳이 복잡한 작업이 필요 없는 경우 -it 옵션없이 단순하게 명령을 실행하고 종료할 수도 있다.


## Links
- [초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)
- [모두의 Docker](https://realhanbit.co.kr/books/226/pages/2201/read)