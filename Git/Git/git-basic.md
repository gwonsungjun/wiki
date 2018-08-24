# Git

## 1. Git 기초

### 커밋(commit)
- 게임의 `세이브`에 해당하는 행동
    - 커밋을 해야 세이브가 된다.
- 다시말해 언제든지 커밋한 시점으로 돌아 갈 수 있다.
- 커밋을 하려면 `저장을 원하는 파일들을 묶어서` 커밋 명령을 수행하면 된다.

#### 커밋 주의사항
1. 반드시 한 번에 하나의 논리적 작업만을 커밋한다.
2. 커밋 메시지는 잘 적어야 한다.

##### 커밋 메시지 작성법
1. 첫 줄에 간단하지만 명확한 내용을 쓴다.
2. 한 줄 비우고
3. 자세한 내용을 적는다.

### 스테이지에 올린다(add)
- commit에서 `저장을 원하는 파일을 묶는` 작업이 필요하다고 했다.
- 이 작업을 `스테이지에 파일을 올린다`라고 한다.

### github에 업로드(push)
- 커밋을 하면 현재 작업 내용의 데이터가 내 컴퓨터(로컬)에 저장된다.
- 이 것을 Github에 업로드하는 걸 push라고 한다.

### (가장 간단한) 일련의 절차
1. Github에서 repository 생성
2. 해당 repo URL 복사
3. Git client(sourcetree, GitKraken, terminal ...) setup
4. 설치한 Git client App에서 복사한 URL Clone
    - Clone : 원격 저장소의 내용을 내 컴퓨터(로컬)로 복사
5. 작업 진행
6. git add
    - add : 로컬에서 작업한 파일들을 스테이지에 추가
7. git commit
    - commit : 스테이지에 올라온 파일들을 가지고 내 로컬에 저장(세이브)
8. git push
    - push : 커밋들을 원격 저장소에 업로드

## 2. 스테이지에 올리지 않은(add 명령 하기 전) 변경사항 취소하기
- `chekout`
    - checkout 명령을 통하여 마지막 커밋으로 되돌아 갈 수 있다.
    - `$ git checkout .` : repository 내 모든 수정 되돌리기
    - `$ git checkout {dir}` : 특정 디렉토리 아래 모든 수정 되돌리기
    - `$ git checkout {filename}` : 특정 파일의 수정 되돌리기
- sourceTree의 `코드뭉치 버리기`(discard hunk) 기능을 사용하면 변경사항을 되돌릴 수 있다.




## Links
- [코드스쿼드 Git 입문 강의 : 정호영님](https://www.youtube.com/watch?v=8AtHcXnJSdA&t=0s&list=PLAHa1zfLtLiPrxoBo9a1HVmauvE2Mn3xX&index=2)