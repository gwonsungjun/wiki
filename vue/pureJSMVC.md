# VueJS 인프런 강의 정리 (1)

- 인프런 김정환님의 [실습 UI 개발로 배워보는 순수 javascript 와 VueJS 개발](https://www.inflearn.com/course/%EC%88%9C%EC%88%98js-vuejs-%EA%B0%9C%EB%B0%9C-%EA%B0%95%EC%A2%8C/)을 듣고 정리를 하였다.

## 개발 환경 구성
1. [Node.js 사이트](https://nodejs.org/ko/)에서 LTS 버전 설치
 - Terminal에서 `node --version` 입력으로 설치 여부 확인
 - ubuntu일 경우 : `sudo gedit /etc/environment` PATH 설정
 - `source /etc/environment` 적용
2. Editor는 [VSCode](https://code.visualstudio.com/) 사용
3. [Lite Server(개발 서버) Tool](https://github.com/johnpapa/lite-server)
  - `$ npm install -g lite-server`
  - ubuntu에서 global로 module을 설치하면 권한 오류가 발생
  - 해결방법 : <http://programmingsummaries.tistory.com/374>에서 방법 1, 2 참조
4. Chrome 최선버전
  - 버전 61 이상 - 모듈시스템을 지원
5. git 설치

## 순수 JS (MVC)

### MVC
- `model` : 데이터 관리(입, 출력), 웹 프론트에서의  model은 DB에 직접 접근하지 않고 API 형태로 접근한다.
- `view` : (데이터를 가지고) 화면을 관리 (모델의 data를 화면에 그림)
- `Controller` : view <-> model, 모델과 뷰를 관리하는 역할
