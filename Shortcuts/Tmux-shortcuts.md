# Tmux shortcut

- 프리픽스(prefix) : tmux의 기본 프리픽스는 ctrl + b
- 세션(session) : tmux가 관리하는 가장 큰 단위이다. 세션에 attach/detach가 이루어진다.
- 윈도우(window) : 세션 안에 존재하는 탭과 같은 기능이다. 하나의 세션에 여러 개의 윈도우를 가질 수 있다. 세션 안에서 윈도우를 만들고 전환할 수 있으며 탭을 이동할 때 처럼 전체 화면이 전환된다.
- 팬(pane) : 윈도우 안에 존재하는 화면 단위이다. 하나의 윈도우에 여러 개의 팬을 가질 수 있다. 전체 화면을 세로로 2등분 하면 2개의 팬이 생긴다.

## Start

- 세션 실행 : `$ tmux`

## 윈도우

- 윈도우 생성 : `<prefix> + c`
- 윈도우 이름 변경 : `<prefix> + ,`
- 윈도우 이동 : `<prefix> + 윈도우 숫자`
- 모든 윈도우 리스트 보기 : `<prefix> + w`

## 팬

- 세로로 윈도우 분할 : `<prefix> + %`
- 가로로 윈도우 분할 : `<prefix> + "`
- 팬 이동 : `<prefix> + q + 숫자` or `<prefix> + 방향키`
- 줌 : `<prefix> + z`, 한번 더 누르면 원상태로 복구

## Links
- [터미널 멀티플렉서 tmux를 배워보자 · Bluesh](https://bluesh55.github.io/2016/10/10/tmux-tutorial/)