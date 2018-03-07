**unzip 사용법**
===

**1.unzip 패키지 설치**
- $ apt-get install unzip

**2.zip 압축 풀기**
- test.zip 이라는 파일 있음을 가정
- $ unzip test.zip
- 특정 폴더에 압축을 풀고 싶으면 -d 옵션 사용, mydir 폴더 있음을 가정
- $ unzip test.zip -d ./mydir

**3.zip 압축 하기**
- 현재 폴더의 모든것을 test.zip이란 파일명으로 압축
- $ zip test.zip ./*
- 옵션 : r - 서브 디렉토리 까지 압축, F - 한글이름을 가진 파일까지 압축
