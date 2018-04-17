# Intellij Properties file Ascii code 값이 대문자로 변경되는 문제 해결

- Intellij에서 SpringMessage Properties 수정 후 Commit 전 확인을 해보니 아래 그림처럼 Value 값이 대문자로 변경되는 이슈를 발견. 

![ASCII_lowercase](/images/asciiUppercase.PNG)

- 다른 팀원들의 Eclipse에서 개발 후 커밋시 아래 그림과 같이 소문자로 표시되었다.

![ASCII_Uppercase](/images/asciiLowercase.PNG)

- 따라서, commit(push) & pull 할 때마다 매번 변경할 수 없으므로 해결방법을 찾게 되었다.
- [WINDOWS] C:\Program Files (x86)\JetBrains\IntelliJ IDEA 12.1.4\bin\idea.properties에 idea.native2ascii.lowercase=true 추가
- [MAC] application > intellij 패키지보기 > bin > idea.properties에 idea.native2ascii.lowercase=true 추가 

### Links
[Intellij 프로퍼티 파일 Ascii로 변경시 key값이 대문자로 변경되는 문제](http://hnsnmn.blogspot.kr/2014/02/intellij-ascii-key.html)
