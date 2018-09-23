# intelliJ 가이드
- [IntelliJ를 시작하시는 분들을 위한 IntelliJ 가이드](https://www.inflearn.com/course/intellij-guide/) 수강 후 정리.

## Toolbox 소개 (추천)
- [Toolbox App](https://www.jetbrains.com/toolbox/app/)을 통해 JetBrains IDE 제품들을 Install, Update, Setting(Maximum heap size...) 등의 작업을 할 수 있음.

## 프로젝트 생성
- Create New Project
- 순수 Java Application
    - Java 선택보단, Maven이나 Gradle 같은 빌드 환경 선택 추천
        - java application을 작성한다 하더라도 junit같은 테스트코드를 작성할때 필요한 라이브러리나, apache commons 등이 필요
        - Maven, Gradle 의존성 관리가 가능
- Gradle을 선택하고 프로젝트 생성
     - GroupId - 프로젝트 그룹
     - ArtifactId - 그룹의 하위 모듈, 스프링을 예로들어) spring security, spring mvc 같은 것
- OS 마다 단축키가 다름
    - 맥에서 개발할 때 해당 단축키가 윈도우나 리눅스에서는 어떻게 보이는지 알 수 있는 플러그인 : `Presentation Assistant`

## 코드 Edit

### 메인메소드 생성 및 실행

- 디렉토리, 패키지, 클래스 등 생성 목록 보기 : `Alt + Insert (Mac: Command + N)`
    - Project, Directory 우클릭 New 단축키
    - 내가 현재 위치에 생성할 수 있는 모든 리스트
- Code templete (흔히 사용하는 메소드 미리 정리 : 축약어)
    - `psvm` : public static void main() 생성
    -  `sout` : System.out.println() 생성
- Run (실행환경 실행)
    - 헌재 포커스 : ` Shift + Ctrl + F10 (Mac : Ctrl + Shift + R)`
    - 이전 실행 : `Shift + F10 (Mac : Ctrl + R)`

### 라인 수정하기
- 라인 복제하기 : `Ctrl + D (Mac : Command + D)`
- 라인 삭제하기 : `Ctrl + Y (Mac : Command + Delete(백스페이스))`
- 문자열 라인 합치기 : `Ctrl + Shift + J (Mac: Ctrl + Shift + J)`
- 라인 단위로 옮기기
    - 구문 이동 : `Shfit + Ctrl + 위아래 화살표 (Mac :Shift + Command + 위아래)` 
    - 라인 이동(구문에 관계없이 이동) : `Shift + Alt + 위아래 (Mac : Shift + Alt(Option) + 위아래)`
- Element 단위로 옮기기 : `Alt+ Ctrl + Shift + 좌우 화살표 (Mac: Alt + Shift + Command + 좌우)` 

### 코드 즉시보기
- 인자값 즉시 보기 : `Ctrl + P (Mac: Command + P)`
- 코드 구현부 즉시 보기 : `Shift + Ctrl + I (Mac: Alt + Space)`
    - 클래스, 생성자, 메소드 등에 직접 들어가지 않아도 구현을 볼 수 있다.
    - 자바뿐만 아니라  html -> js, css 호출 시 호출하는 다른 정적파일도 미리보기 가능하다.
- Doc 즉시 보기 : `Ctrl + Q (Mac: F1)`
    - 오라클 홈페이지에 갈 필요가 없어진다.
    - js의 경우 MDN의 내용이 보여진다.

### 포커스 - 에디터
- 단어별 이동 : `Ctrl + Right/Left (Mac : Alt + Right/Left)`
- 단어별 선택 : `Ctrl + Shift + Right/Left (Mac : Alt + Shift + Right/Left)`
- 라인 첫/끝 이동 : `Home, End (Mac : fn + Right/Left)`
- 라인 전체 선택 : `Shift + Home, End (Mac : fn + Shift + Right/Left)`
- Page Up/Down : `Page Up/Down (Mac : fn + Up/Down)`

### 포커스 - 특수키
- 포커스 범위 한 단계씩 늘리기 : `Ctrl + W (Mac: Alt + Up)`
- 포커스 범위 한 단계씩 줄이기 : `Shift + Ctrl + W (Mac: Alt + Down)`
- 포커스 앞으로 가기(클래스의 이동까지 가능) : `Ctrl + Alt + Right (Mac: Alt + ])`
- 포커스 뒤로 가기 : `Ctrl + Alt + Left (Mac: Alt + [)`
- 멀티 포커스 : `Ctrl + Ctrl + Down (Mac: Alt + Alt + Down)`
- 오류 라인으로 자동 포커스 : `F2`

### 검색 - 텍스트
- 현재 파일에서 검색 : `Ctrl + F (Mac: Command + F)`
- 현재 파일에서 교체 : `Ctrl + R (Mac: Command + R)` / Replace, Replace All
- (프로젝트) 전체에서 검색 : `Ctrl + Shift + F (Mac: Command + Shift + F)`
- 전체에서 교체 : `Ctrl + Shift + R (Mac: Command + Shift + R)`
- 정규표현식으로 검색, 교체 : `Regex 체크`, [참조](https://jojoldu.tistory.com/160)

### 검색 - 기타
1. 파일 검색 : `Ctrl + Shift + N (Mac : Command + Shift + O)`
    - 패키지까지 포함해서 검색할 수 있다.
    - 예를들어 sp2라는 패키지 안에 Member.java 파일을 찾고 싶다면 검색 창에 "sp2/Member" 입력
2. 메소드 검색 : `Ctrl + Shift + Alt + N (Mac : Command + Alt + O)`
3. `(중요)` Action 검색 : `Ctrl + Shift +A (Mac : Command + Shift + A)`
    - 인텔리제이의 모든 event와 option들을 모두 찾아낼 수 있다.
4. 최근 열었던 파일 목록 보기 : `Ctrl + E (Mac : Command + E)`
5. 최근 수정한 파일 목록 보기 : `Ctrl + Shift + E (Mac : Command + Shift + E)`

### 자동 완성
1. 스마트 자동 완성 : `Ctrl + Shift + Space (Mac : Ctrl + Shift + Space)`
    - 일반적인 자동 완성 : `Ctrl + Space (Mac : Ctrl + Space)`
2. 스태틱 메소드 자동 완성 :  `Ctrl + Space 2번 (Mac : Ctrl + Space 2번)`
3. Getter / Setter / 생성자 자동 완성 :  `Alt + Insert (Mac : Command + N)`  생성할 수 있는 모든 경우를 보여준다.(Generate)
4. Override 메소드 자동 완성 : `Ctrl + i (Mac : Ctrl + i)`

### Live Template
1. Live Template 소개
    - 반복되는 코드 또는 항상 사용해야하는 코드들을 추가 후 호출
    - 예를들어) psvm, sout, ifn과 같은 축약어
    - 포커스가 있는 위치에 맞춰서 나올 수 있는 모든 축약어(Live Template) 목록 : `Ctrl + J (Mac : Command + J)`
2. Live Template 추가하기 
    - (1) Ctrl + Shift + A -> Live Template 검색 후 선택
    - (2) '+' 클릭 -> 1.Live Template 선택
    - (3) Abbreviation : 축약어 입력, Description : 설명, Template text : 적용될 코드 입력
    - (4) 에러 메시지 옆 define 클릭 -> java에서만 사용할 것이기 때문에 java 선택
    - (5) apply -> ok

### 리팩토링 - Extract(추출하기)
1. 변수 추출하기(extract variable) : `Ctrl + Alt + V (Mac : Command + Option + V)`
2. 파라미터 추출하기 : `Ctrl + Alt + P (Mac : Command + Option + P)`
3. 메소드 추출하기 : `Ctrl + Alt + M (Mac : Command + Option + M)`
4. 이너클래스 추출하기(이동하기) : 해당 이너 클래스명에 포커스를 두고 `F6`

### 리팩토링 기타
1. 이름 일괄 변경하기(클래스명, 메소드명, 변수명 등) : `Shift + F6`
2. 타입 일괄 변경하기(메소드의 리턴타입 + 해당 메소드를 사용(선언)하는 쪽도 모두 변경됨) : `Ctrl + Shift + F6 (Mac : Command + Shift + F6)`
3. Import 정리하기 : `Ctrl + Alt + O (Mac : Command + Option + O)`, (자동으로 사용하지 않는 Import 정리) Ctrl + Shift + A -> Optimize import on 검색 -> Auto Import: Java: Optimize imports on the ... `On`으로 변경
4. 코드 자동 정렬하기 : `Ctrl + Alt + L (Mac : Command + Option + L)`

### 디버깅
- break point : 해당 line에 클릭 or Ctrl + F8
1. Debug 모드로 실행하기 - 즉시 실행 : `윈도우/리눅스 단축키 없음 (Mac : Ctrl + Shift + D)` (현재 포커스가 있는 메서드 디버그 모드로 실행)
2. Debug 모드로 실행하기 - 이전 실행 : `Shift + F9 (Mac : Ctrl + D)` (인텔리제이 상단 오른쪽에 남아있는 이전에 실행했던 메소드를 실행)
3. Resume(다음 브레이크 포인트로 넘어가기) : `F9 (Mac : Command + Option + R)`
4. Step Over(현재 브레이크 포인트의 다음 한 줄 실행) : `F8` 
5. Step Into(현재 브레이크 포인트의 다음 실행할 메소드 안으로 들어감) : `F7` 
6. Step Out(Step Into를 통해 포커스가 특정 메소드 안으로 들어왔을 때 다시 밖으로 나감) : `Shift + F8`
7. Condition : `브레이크 포인트에서 우클릭 해당 조건 입력 후 Done.` (반복문에서 해당 조건을 가졌을 때만 브레이크하도록 설정 가능)
8. Evaluate Expression(브레이크 된 상태에서 코드 사용하기) : `Alt + F8 (Mac : Option + F8)`
9. Watch(브레이크 이후의 코드 변경 확인하기) : 단축키는 없음.

### Git 기본 기능 사용하기
1. Git View On : View -> Tool Windows -> Version Control or `Alt + 9 (Mac : Command + 9)`
2. Git Option Popup(VCS Operations) : `Alt + Back Quote (Mac : Ctrl + V)`
3. Git History : `VCS Operations -> 4. Show History`
4. Branch : `VCS Operations -> 7. Branches`
5. Commit : `VCS Operations -> 1. Commit` or `Ctrl+K (Mac : Command + K)`
    - 커밋 창에서 Reformat code와 Rearrange code를 체크하면 커밋 전에 자동으로 재정렬 후 커밋한다.
    - Optimize imports : 사용하지 않는 import문 제거
    - Perform code analysis : 코드 분석(Sonar, find bug 등을 이용해서)
6. Push : `Ctrl + Shift +K (Mac : Command + Shift + K)`
    - 파일이 생성된 경우에는 커밋&푸시를 지원하지 않으므로 push는 별도로 진행해야 한다.
7. Pull : 단축키 없음. (Ctrl(Command) + Shift + A : git pull 검색 -> pull... 선택)