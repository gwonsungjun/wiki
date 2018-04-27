# GitLab Repository URL 변경

-  아래의 그림 처럼 저장소의 URL을 변경하고 싶은 경우

![gitlabchangeurl](/images/gitlab1.PNG)

1. /etc/gitlab/gitlab.rb 실행
2. external_url을 변경하고 싶은 url로 수정 후 저장
3. gitlab-ctl reconfigure 입력 후 실행
4. 적용된 것을 확인할 수 있음.
