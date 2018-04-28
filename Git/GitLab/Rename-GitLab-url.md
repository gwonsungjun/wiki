# GitLab Repository URL 변경

-  아래의 그림 처럼 저장소의 URL을 변경하고 싶은 경우

![gitlabchangeurl](/images/gitlab1.PNG)

1. $vi /etc/gitlab/gitlab.rb 입력
2. external_url을 변경하고 싶은 url로 수정 후 저장
3. $gitlab-ctl reconfigure 입력
4. 적용된 것을 확인할 수 있음.
