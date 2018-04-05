**nohup 사용법**
===

**1.nohup 개요**
- nohup은 HUP(hangup) 신호를 무시하도록 만드는 명령어이다. HUP 신호는 전통적으로 터미널이 의존 프로세스들에게 로그아웃을 알리는 방식이다.
- 즉, 프로세스 중단(hangup)을 무시하고 명령어를 실행하는 명령어
- 터미널 세션이 끊겨도 실행을 멈추지 않고 동작하도록 함
- 일반적으로 터미널로 향하는 출력은 별도로 넘겨주기 처리를 하지 않았을 경우 nohup.out이라는 이름의 파일로 출력된다.

**2.& 란?**
- 프로세스를   백그라운드에서 동작하도록 만드는 명령어

**3.nohup 사용법**
- 실행 : Ex) $ nohup java -jar programName.jar & (사내에서 배치 프로그램을 jar 파일로 배포하는데 이때 nohup 명령어를 사용한다.)
- 실행시 java, python, .sh(쉘 스크립트), echo 등 사용가능.
- 확인 : ps -ef | grep programName
- 종료 : kill -(9, 15 ...) 해당 프로세스 pid

**4. 주의 사항**
- nohup으로 실행할 쉘스크립트파일은 현재 퍼미션이 755 이상 상태여야 한다.

**5.Links**
- [위키백과](https://ko.wikipedia.org/wiki/Nohup)
- [제타위키](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_nohup_%EC%82%AC%EC%9A%A9%EB%B2%95)
