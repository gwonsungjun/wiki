# Intellij : address localhost 1099 is already in use

- intellij 사용 시 가끔 코드 수정 후 프로젝트를 재시작하면 기존에 사용 중인 1099 포트가 정상적으로 종료되지 않아 재시작이 안 되는 경우가 발생한다.

## 해당 프로세스를 kill(강제종료)하고 재시작하면 해결된다.

### Windows
1. cmd 창에서 `$ netstat -ano` 실행 후 해당 포트(1099)를 사용 중인 프로세스 아이디(PID) 확인
2. `$ taskkill /pid 9876 /f` 명령을 통해 프로세스 강제 종료
3. intellij에서 톰캣 재시작 -> 성공

### Linux(Ubuntu)
1. `$ netstat -ntlp | grep 1099` 해당 포트를 사용 중인 프로세스 아이디(PID) 확인
    - `$ lsof -i :포트번호` 명령으로도 확인 가능.
2. `$ kill -9 9876` 명령을 통해 프로세스 강제 종료
3. intellij에서 톰캣 재시작 -> 성공