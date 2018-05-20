# Linux 명령어 정리

## sudo 임시로 관리자 권한 얻기
- sudo : 관리자 권한 행사
- sudo -i, sudo su : root로 로그인

## grep 다양한 문자열을 한 번에 검색
- 파일의 내용을 빠짐없이 확인해서 찾는 문자열이 포함되어있는지 조사
- global regular expression print - 파일 전체에서 정규표현식과 일치하는 위치를 출력하라
```
$ grep -r "검색하고 싶은 문자열" /home/docs
```
- -r : 서브 폴더까지 검색하도록 지정
- 정규 표현식(regular expression)
```
- () : 그룹화
- | : 좌우 중 하나
- ? : 직전 표현이 0회 또는 1회 등장
- * : 직전 표현이 0회 이상 연속해서 등장
- + : 직전 표현이 1회 이상 연속해서 등장
- . : 임의이 한 문자
- ^ : 줄 머리
- $ : 줄 끝
```
- -E : 정규표현식 사용
- -i : ignore case, 알파벳 대소문자 차이 무시하고 검색
```
$ grep -r -i "yameno tarou" 디렉토리경로
$ grep -r -i -E "((야메노) *(타로) | yameno +tarou)" 디렉토리 경로
```

## vim
- 편집 : 시작 -> 노멀모드 -> i 입력 -> 끼워넣기 모드로 편집 -> esc -> :wq (저장 & 종료)
- 검색 : 노멀모드 -> / 입력
  - N : 검색된 곳을 순서대로
  - Shift N : 반대 방향
  - 정규표현식으로 검색 : / 뒤에 \v(백슬래쉬, 소문자v)
  - EX) /\v(CP949|EUC-KR)
- 복사 : 노멀모드 -> v - 선택모드 (화살표를 이용한 범위 지정)
- 양크 : (yank:끌어당기다) - 클립보드에 텍스트가 복사됨
- 붙여넣기 : Shift + p
  - 10회 반복해서 붙여넣기 : 1, 0, Shift, p
- 되돌리기 : u (undo)
- 되살리기 : ctrl + r (redo)
- ctrl + z : 실행중인 애플리케이션 일시 정지
  - fg : 다시 실행 (foreground)

## tmux 가상 단말
- $ sudo apt-get install tmux
- 실행 : $ tmux
- 네트웍이 끊긴경우 ssh 재접속해서 $ tmux attach 입력
- ctrl + b (tmux의 기능 사용) 입력한 다음
  - d 입력 : tmux 화면에서 빠져나옴 (detach)
  - c 입력 : create : 새로운 탭 열기
  - p 입력 : previous : 이전 탭
  - n 입력 : next : 다음 탭
  - " 입력 : 화면 가로로 분할
  - % 입력 : 화면 세로로 분할
    - ctrl + b + 방향키 : 분할된 화면 포커스 전환
    - exit : 분할 해제
    - 분할키를 입력하면 그때 포커스가 있는 화면을 분할함.
  - ctrl 누르면서 방향키 : 분할 경계선 (화면 비율) 변경

## 명령어 이력
- 방향키 위, 아래(↕)를 이용한 이전에 실행한 명령어 이력 표시
- $ vi ~/.bash_history
- 명령어 검색 기능
  - 후방 검색(현재 위치보다 오래된 방향으로 이동) : ctrl + R
  - ctrl + R을 사용하면 검색 위치가 오래된 방향으로 이동되서 그보다 새로운 명령어 이력은 검색할 수 없게 된다.
    - 따라서, 아래와 같이 역방향(전방검색)이 가능하도록 수정
    - 1. vi ~/.bash_history
    - 2. shift + G 로 마지막으로 이동해서 stty stop undef 입력
    - 3. :wq
    - 4. 재로그인 (bash 재실행, 새로운 설정 읽어 들임)
  - 전방 검색(현재 위치보다 새로운 방향으로 이동) : ctrl + S
- 이력 저장 건수 설정
  - 1. $ vi ~/.bashrc
  - 2. shift + G 로 마지막으로 이동
  - 3. export HISTSIZE=10000
  - 4. export HISTFILESIZE=10000
  - 5. c, d는 같은 값으로 지정하고 :wq 저장.
  - 6. 재로그인
- 가상 단말 여러개의 bash는 각자의 명령어 이력 복사본을 가짐
  - 따라서, 다른 화면에서 실행한 명령어는 또 다른 화면에서 실행한 명령어를 검색할 수 없다.
  - $ vi ~/.bashrc에 아래를 추가
  ```
  function share_history{
    history -a
    history -c
    history -r
  }
  PROMPT_COMMAND='share_history'
  shopt -u histappend
  ```
  - bash_history와 메모리 복사본을 자주 동기화 하라는 의미

## Links
- [만화로 배우는 리눅스 시스템 관리 1](http://book.naver.com/bookdb/book_detail.nhn?bid=10995037)
