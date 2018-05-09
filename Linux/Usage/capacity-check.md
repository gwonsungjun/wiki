# Linux 용량 확인

## 디스크 사용량 확인 df

- df : 킬로바이트 단위
- df -h : human-readable, 메가바이트 & 기가바이트 등 용량단위를 적절히 맞추어 보여줌
- df -P : 한줄로 출력 (파일시스템 경로가 긴 경우 2줄로 나올 수도 있음)  

![df](/images/df.PNG)

## 디렉토리 용량 확인 du

- du : disk usage
- du -h : human-readable, 메가바이트 &
기가바이트 등 용량단위를 적절히 맞추어 보여줌
- du -s : (summarize) 사용량의 총 합계만 출력

![du](/images/du.PNG)

## df와 du 명령어 차이

- 현재 실행 중인 프로세스가 오픈한 파일에 대해서 삭제처리를 한 후에 해당 프로세스(태스크)를 종료하지 않으면 그 파일은 deleted 상태로 남게 된다. 즉, 파일시스템에 deleted 상태정보로 유지되고 있는 것이다. 그렇기 때문에 df 명령으로 확인하게 되면 deleted 파일이 차지하는 용량까지 더해져서 du 명령과는 차이를 나타낼 수가 있다. (특히 Mysql)
- 확인하는 방법은 ? lsof | grep deleted
- 더 자세한 내용은 [FAQ ,Linux 디스크 사용량의 차이 (df 명령과 du)](http://tumblr.lunatine.net/post/13628840969/faq-linux-%EB%94%94%EC%8A%A4%ED%81%AC-%EC%82%AC%EC%9A%A9%EB%9F%89%EC%9D%98-%EC%B0%A8%EC%9D%B4-df-%EB%AA%85%EB%A0%B9%EA%B3%BC-du) 확인


## Links
- [FAQ ,Linux 디스크 사용량의 차이 (df 명령과 du)](http://tumblr.lunatine.net/post/13628840969/faq-linux-%EB%94%94%EC%8A%A4%ED%81%AC-%EC%82%AC%EC%9A%A9%EB%9F%89%EC%9D%98-%EC%B0%A8%EC%9D%B4-df-%EB%AA%85%EB%A0%B9%EA%B3%BC-du)
- [리눅스 디렉토리 용량 확인 du](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%94%94%EB%A0%89%ED%86%A0%EB%A6%AC_%EC%9A%A9%EB%9F%89_%ED%99%95%EC%9D%B8_du)
- [리눅스 디스크 사용량 확인 df](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%94%94%EC%8A%A4%ED%81%AC_%EC%82%AC%EC%9A%A9%EB%9F%89_%ED%99%95%EC%9D%B8_df)