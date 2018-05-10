# cron, crontab 사용법

## 1.cron, cornd, crontab.. 이란?
- 작업을 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링
- 프로세스 예약 데몬
- 리눅스용 작업 스케줄러
- 서버는 늘 깨어있다는 것을 이용한 최대한의 활용법
- 특정시간에 명령어가 사용되도록 등록가능
- cronie(패키지) = crond(데몬) + crontab(크론 계획표)
- 로그: /var/log/cron에 변경/수행 이력이 기록됨.
- cron은 데몬, crontab은 지정된 시간에 다른 프로그램을 실행하면서 연속적으로 실행하는 프로그램

## 2.등록형식과 예시
- " * 분(0-59) * 시간(0-23) * 일(1-31) * 월(1-12) * 요일(0-7) "
- 등록형식은 정리가 아주 잘 되어있는 [제타위키](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B0%98%EB%B3%B5_%EC%98%88%EC%95%BD%EC%9E%91%EC%97%85_cron,_crond,_crontab)를 참조

## 3.기본사용법
- crontab -l : crontab 작업 목록 확인
- crontab -e : cron 편집 작업, 수동 등록/수정/확인 (저장은 vi처럼 :wq로 저장)
- crontab -r : 현재 사용자의 예약 작업을 모두 삭제
- ps -ef | grep crond : crond 실행 확인
- /etc/rc.d/init.d/crond start{restart | stop} : cron 데몬의 시작과 종료
- /usr/bin/crontab : 명령어 위치

## 4.cron이 참조하는 crontab 파일 위치
- /var/spool/cron : 시스템 개별 사용자를 위한 crontab 파일 위치이며 일반적으로 root 계정용 하나와 계정 사용자당 1개의 파일을 가진다.
- /etc/cron.d : 소프트웨어 패키지 설치 등 주기적인 작업 등록
- /etc/crontab : 시스템 관련 작업들을 등록
- cron은 시작할 때 모든 곳에 저장된 설정파일들을 읽어 메모리에 저장해두고 휴지 상태에 들어간다.

## 5.Links
- [제타위키](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B0%98%EB%B3%B5_%EC%98%88%EC%95%BD%EC%9E%91%EC%97%85_cron,_crond,_crontab)
- [위키피디아](https://ko.wikipedia.org/wiki/Cron)
- [리눅스 크론탭(Linux Crontab) 사용법
](http://jdm.kr/blog/2)
- [리눅스 스케줄러 crontab 이용하기](http://luckys.tistory.com/162)
- [리눅스 cron - 작업 예약 명령](http://webdir.tistory.com/174)
- [리눅스 스케줄 설정 crontab](http://zeronica.tistory.com/105)
