# Web Browser Rendering
- HTML 파일이 올 때 브라우저가 어떻게 렌더링 과정을 거쳐서 화면에 보이게 되는가

## Brower
- 브라우저는 월드와이드웹(WWW)에서 정보를 검색, 표현하고 탐색하기 위한 소프트웨어

## Brower Components
- 현재 우리가 보고 있는 브라우저 == Brower Components

![BrowerComponents](/images/BrowerComponents.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>

##렌더링 엔진
- 브라우저 별로 가지고 있다.
- Firefox Gecko, Safari WebKit, Chrome 크로미움 (v8+Blink), Opera Blink, ...

### 브라우저 엔진의 Main Flow
1. **HTML Parsing to construct the DOM tree**
  - HTML을 문자 단위로 해석해서 이 내용 가진 의미를 파악(Parsing) 이것들을 어떤 데이터 객체로 구조화시킴
2. **Render tree construction**
  - 아래 그림과 같이, 트리구조 형태로 가지고 있게 됨
  - ex) H1 태그는 Block 이다, "WEb p..."는 Text 이다.
  ![rederTree](/images/rederTree.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>
3. **Layout of render tree**
  - 화면에 어떻게 배치할지, reder tree를 기준으로 CSS(스타일정보)를 매칭 (key와 value로 이루어 져있음.)
4. **Painting the reder tree**
  - 화면에 직접 그리게됨.

### safari 브라우저의 webkit 렌더링엔진 처리 과정
![webkitFlow](/images/webkitFlow.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>
- HTML을 해석해서 DOM Tree 생성
- CSS를 해석해서 역시 CSS Tree(CSS Object Model)을 생성
  - 이 과정에서 Parsing 과정이 필요하며 토큰 단위로 해석됨
- DOM Tree와 CSS Tree, 이 두 개는 연관되어 있으므로 Render Tree로 다시 조합
  - 이렇게 조합된 결과는 화면에 어떻게 배치할지 크기와 위치 정보를 담고 있다.
- 구성된 Render Tree정보를 통해서 화면에 어떤 부분에 어떻게 색칠을 할지 Painting과정을 거치게 된다.



### Parsing-general
- token 단위로 잘라서 의미를 해석하고 그 의미에 따라서 실행
- Syntax 트리를 만들고 처리를 하게 됨
- 예를들어, 2 + 3 - 1

![parsing-general](/images/parsing-general.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>

### HTML Parser
- 아래 그림과 같이 트리구조(Dom Tree)를 만들어서 브라우저가 처리하게 됨.
```html
<html>
  <body>
    <p>
      Hello world
    </p>
  </body>
</html>
```

![htmlparsing](/images/htmlparsing.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>

### parsing CSS

![cssParsing](/images/cssParsing.png)
그림 출처 : <https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/>


## Links
- [edwith, Full-Stack Web Developer 강의](http://www.edwith.org/boostcourse-web/lecture/16663/)
- [How Browsers Work](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

## 같이 보면 좋을 Link
- [브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)
- [크롬을 이용한 프론트엔드 디버깅](https://programmers.co.kr/learn/courses/7)
