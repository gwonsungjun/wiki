# Ubuntu Intellij keymap(단축키) 문제

- 우분투에서 Intellij 설치후 keymap을 올바르게 설정하였음에도 단축키가 제대로 동작하지 않는 경우가 있었다.
- 일반적으로 Ctrl+D를 이용해서 Duplicate line or selection을 사용하는데 해당 명령이 제대로 동작하지 않아 해결법을 찾아보게 되었다.
- 문제는 vim처럼 동작해서 코딩할 때 입력모드로 전환해야만 입력이 되는 경우였다.

## 해결방법 : Settings > plugins > IdeaVim 이 체크되어 있다면 체크 해제

![intellij_keymap_problem](/images/intellijKeyMapProblem.png)
