# linux 명령어 정리

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

##

## Links
- [만화로 배우는 리눅스 시스템 관리 1](http://book.naver.com/bookdb/book_detail.nhn?bid=10995037)
