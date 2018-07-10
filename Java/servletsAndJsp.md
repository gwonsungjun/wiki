# Servlet & JSP 정리

### Web Application 등장 배경
- 기존 네트워크 프로그램은 소켓과 멀티 쓰레드를 이용하여 개발자가 직접 파일입출력을 다뤘다.
- 클라이언트의 데이터 수집 방법이 링크형태를 띄고 FTP 프로토콜 방식을 사용해서 데이터를 얻었다.
- 이를 개선하고자 HTTP 프로토콜 방식이 등장하였다.

### Web Application
- WAS에 설치(deploy)되어 동작하는 어플리케이션.
- 자바 웹 어플리케이션에는 HTML, CSS, 이미지, 자바로 작성된 클래스(Servlet도 포함됨, package, 인터페이스 등), 각종 설정 파일 등이 포함된다.

### JAVA 웹 어플리케이션의 폴더 구조

![javawebappDirStr](/images/javawebappDirStr.PNG)
- 반드시 WEB-INF 폴더가 존재 해야한다. [그림참조](https://www.edwith.org/boostcourse-web/lecture/16686/)

### 서블릿이란?
- Server+Applet으로 “Client의 요청을 처리하고 그 결과를 다시 Client에게 전송하는 Servlet Class의 구현 규칙을 지킨 자바 프로그램”.
  - 자바프로그램이라고 정의했다면 당연히 JDK가 필요하며, 자바 API를 동작시키는 JVM이 필요하다.
  - Servlet은 Web Application 확장이 용이하고 플랫폼에 독립적이다.
  - HttpServlet 클래스를 상속받아야 한다.

### Servlet Container란?
- 서블릿 컨테이너는 개발자가 웹서버와 통신하기 위하여 소켓을 생성하고, 특정 포트에 리스닝하고, 스트림을 생성하는 등의 복잡한 일들을 할 필요가 없게 해준다. 컨테이너는 servlet의 생성부터 소멸까지의 일련의 과정(Lifer Cycle)을 관리한다. 서블릿 컨테이너는 요청이 들어올 때마다 새로운 자바 스레드를 만든다.
- 서블릿 컨테이너는 개발자가 웹서버와 통신하기 위하여 소켓을 생성하고, 특정 포트에 리스닝하고, 스트림을 생성하는 등의 복잡한 일들을 할 필요가 없게 해준다. 컨테이너는 servlet의 생성부터 소멸까지의 일련의 과정(Lifer Cycle)을 관리한다. 서블릿 컨테이너는 요청이 들어올 때마다 새로운 자바 스레드를 만든다. 우리가 알고 있는 대표적인 Servlet Container가 Tomcat 이다. 톰켓같은 was가 java파일을 컴파일해서 Class로 만들고 메모리에 올려 servlet객체를 만든다.

### 컨테이너가 주는 혜택
- 컨테이너 (대표적인 예: 톰캣)는 서블릿을 실행하고 관리한다.
- 통신(커뮤니케이션) 지원
  - 서블릿과 웹 서버가 통신할 수 있는 손쉬운 방법을 제공한다. 우리가 통신을 한다고 생각할 때 소켓을 만들고, 특정 포트를 리스닝하고, 연결 요청이 들어오면 스트림을 생성해서 요청을 받는다고 알고있는데 이 과정을 서블릿 컨테이너가 대신 해주는 것이다. 서블릿 컨테이너는 이런 통신 과정을 API 로 제공하고 있기 때문에 우리가 쉽게 사용할 수 있다.
- 생명주기(라이프사이클) 관리
  - 서블릿 컨테이너가 기동되는 순간 서블릿 클래스를 로딩해서 인스턴스화하고, 초기화 메서드를 호출하고, 요청이 들어오면 적절한 서블릿 메소드를 찾아서 호출한다. 만약 서블릿의 생명이 다하는 순간 가비지 컬렉션을 진행한다.
- 멀티스레딩 지원
  - 서블릿 컨테이너는 해당 서블릿의 요청이 들어오면 스레드를 생성해서 작업을 수행한다. 즉 동시의 여러 요청이 들어온다면 멀티스레딩 환경으로 동시다발적인 작업을 관리한다.
- 선언적인 보안 관리
  - 서블릿 컨테이너는 보안 관련된 기능을 지원한다. 따라서 서블릿 코드 안에 보안 관련된 메소드를 구현하지 않아도 된다.
- JSP 지원 등

### 컨테이너는 요청을 어떻게 다룰까?
1. 사용자가 서블릿에 대한 링크를 클릭한다. web.xml 파일의 설정들은 Web Application 시작시 메모리에 로딩됨. (수정을 할 경우 web application을 재시작 해야함.)
2. 컨테이너는 들어온 요청이ㅁ 서블릿이라는 것을 간파하곤 다음 두 객체를 생성한다.
  (1) HttpServletRequest
  (2) HttpServletResponse
3. 컨테이너는 사용자가 날린 URL을 분석하여 어떤 서블릿에 대한 요청인지 알아낸다(DD == web.xml 참조) 그 다음 해당 서블릿 스레드를 생성하여 Request, Response 객체를 인자로 넘긴다.
4. 컨테이너는 서블릿 service() 메소드를 호출한다. 요청에 지정한 방식(Method)에 따라 doGet()을 호출할지, doPost()를 호출할지 결정한다.
5. doGet() 호출을 가정하고 doGet() 메소드는 동적인 페이지를 생성한 다음, 이를 Response 객체에 실어 컨테이너로 보낸다. 보내고 난 후에도 컨테이너는 Response 객체에 대한 참조(레퍼런스)를 가지고 있다는 것을 잊지 말자.
6. 스레드 작업이 끝나면, 컨테이너는 Response 객체를 HTTP Response로 전환하여 클라이언트로 내려 보낸다. 이제 마지막으로 Request와 Response 객체를 소멸시킨다.(가비지 컬렉션)

![servelt](/images/servelt1.jpg)[그림참조](https://showbang.github.io/typistShow/2017/01/11/%ED%8F%B4%EB%8D%94%ED%85%8C%EC%8A%A4%ED%8A%B8/)


### HttpServletRequest
- http프로토콜의 request정보를 서블릿에게 전달하기 위한 목적으로 사용
- 헤더정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽어 들이는 메소드를 가지고 있다.
- Body의 Stream을 읽어 들이는 메소드를 가지고 있다.

### HttpServletResponse
- WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한 HttpServleResponse 객체를 생성하여 서블릿에게 전달한다.
- 서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지등을 전송한다.

### 서블릿의 생명주기
- 컨테이너는 서블릿을 로딩한다. 그 다음 디폴트 생성자를 호출하고, init() 메소드를 실행한다.
- init() 메소드는 서블릿 일생 중 단 한번만 호출된다. ( 클라이언트에게 서비스를 제공하기 전에 실행, 서버가 켜질 때)
- init() 메소드에서 ServletConfig(서블릿 당 하나) 객체와 ServletContext (웹 애플리케이션 당 하나) 객체에 접근할 수 있다. 이 두 객체를 통해 서블릿 밑 웹 어플리케이션 설정 정보를 파악할 수 있음.
- 컨테이너는 서블릿의 destroy() 메소드를 호출하여 서블릿 일생을 마감(서버가 꺼질 때)
- 서블릿은 일생의 대부분을 클라이언트 요청에 대한 응답으로 service() 메소드를 실행하는데 보낸다.
- 서블릿에 대한 클라이언트 요청은 별개의 스레드에서 실행된다. 서블릿 인스턴스는 하나 밖에 없음.

![servletLifeCycle](/images/servletLifeCycle.png)[그림참조](https://showbang.github.io/typistShow/2017/01/11/%ED%8F%B4%EB%8D%94%ED%85%8C%EC%8A%A4%ED%8A%B8/)

### 정리해보면..
```java
- 컨테이너는 서블릿 디폴트 생성자를 실행 (생성된 서블릿 인스턴스는 새로 생성없이 계속 사용), init() 실행 (최초에 한번 실행)
- 클라이언트 요청
- 컨테이너는 서블릿 스레드 생성 (요청당 하나의 스레드 생성)
- DD 분석해서 어느 서블릿 요청인지 확인
- 해당 서블릿의 service() 실행 -> doGet()/doPOst() 실행, 이때 request, response 인자로 넘김
- 로직 수행
- 응답 후 스레드 소멸
- destroy() - 서버 다운 전 한번
```

### 리다이렉트와 디스패치
- 리다이렉트(Redirect) : 요청에 대한 응답을 누가할 지 선택, 요청을 완전히 다른 URL로 방향을 바꿈, 브라우저는 서버에서 정한 URL을 받아 새로운 요청을 보냄. `클라이언트에서 동작`
- 디스패치(dispatch) : 웹 애플리케이션에 있는 다른 컴포넌트에게 처리를 위임 (서블릿 -> jsp) `서버에서 동작`

### 세 가지 생존범위 : Context, Request, Session
- Context : 전체 애플리케이션에서 공유. 스레드-안전(x)
- Session : 하나의 Request에만 관련된 것이 아니라 클라이언트 세션에 관련. 스레드-안전(x)
- Request : 특정 요청에만 관련된 데이터 등. 스레드-안전(o)

### 세션관리
- 컨테이너는 클라이언트에 Response의 일부로 세션 ID를 보낸다. 그러면 클라이언트는 다음 요청부터 Request의 일부로 세션 ID를 돌려 보낸다.
  - 세션 ID 정보를 교환하는 가장 간단하며, 일반적인 방법은 쿠키를 사용하는 것이다.
- Response 객체에 세션 쿠키 보내기 : HttpSession session = request.getSession();
- Request 객체로부터 세션 ID 가져오기 : HttpSession session = request.getSession();
  - 위의 두개 같음. 우리는 getSession() 메소드만 사용, 나머지 작업은 컨테이너가 다한다.
- 서버와 클라이언트 간 이름/값의 쌍으로 쿠키를 교환
- 서버는 클라이언트로 쿠키를 보내고, 이후 클라이언트는 매번 요청에 이 값을 전송.
- Httpsession 삶에 있어 주요 순간들
  - 세선의 생명과 소멸 (new = getSession(), invalidate)
  - 세션 속성의 추가, 제거, 대체 (setAttribute(), removeAttribute())
  - 분산 환경에서는 한 VM에서 세션을 비활성화(passivate)시키고, 다른 VM에서 이를 활성화한다.

### JSP란
- [Java Server Page](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%84%9C%EB%B2%84_%ED%8E%98%EC%9D%B4%EC%A7%80) : JSP는 클라이언트의 요청으로부터 동적 컨텐츠를 생성하여 응답해주는 역할을 하기위한 기술
- JSP는 서블릿으로 변환되어 사용된다.
- `myJsp.jsp` -> `myJsp_jsp.java` -> `myJsp_jsp.class` -> `myJsp_jsp servelt`

### JSP의 일생
1. jsp 파일을 작성하고 배포
2. 컨테이너는 이 애플리케이션의 DD(web.xml) 파일을 읽는다. 그렇다고 .jsp 파일에 어떤 작업을 하는 것은 아니다. (클라이언트 최초 호출 전까지 .jsp 파일은 서버에 그대로 그냥 있는다.)
3. 사용자가 .jsp를 요청하는 링크를 클릭
4. 컨테이너는 .jsp 파일을 서블릿 파일을 만들기 위한 .java 소스 파일로 변환한다.
5. 컨테이너는 .java 파일을 컴파일하여 .class 파일로 만든다.
6. 컨테이너는 새로 생성된 서블릿 클래스를 메모리로 로딩
7. 컨테이너가 서블릿을 인스턴스화 하면 인스턴스 jspInit() 메소드가 실행된다.(이제 이 객체는 클라이언트 요청을 처리할 수 있는 완전한 서블릿으로 거듭남.)
8. 요청이 들어올 때마다 컨테이너는 새로운 스레드를 만들어 jspService() 메소드를 실행한다. 서블릿과 동일하게 요청을 처리한다.
- **JSP 파일을 서블릿으로 변환과 컴파일 작업은 해당 JSP 일생에 있어 딱 한번 일어난다.**

## 추가)

### 배포 서술자(DD, Deployment Descriptor) == Web.xml이란?
- ``톰캣의 실행환경``에 대한 정보를 담당하는 '환경설정' 파일
- 각종 servlet의 설정과 servlet 매핑, 필터, 인코딩 등을 담당
- 톰캣에 있는 모든 web applicweb.xml 파일의 설정들은 Web Application 시작시 메모리에 로딩됨. (수정을 할 경우 web application을 재시작 해야함.)
- 각 application이 deploy될 때 각 application의 'WEB-INF/web.xml'(웹 어플리케이션의 모든 정보가 들어있다고 간주) Deployment Descriptor에 따라서 처리
  - 각 application 마다 설정시, web.xml은 파일을 복사해서 필요한 것만 적으면 된다
- 모든 Web application은 반드시 하나의 web.xml 파일을 가져야 함
- 위치 : `WEB-INF 폴더 아래`

### Servelt 웹 서버 구동 순서
1. 웹 서버 구동에 필요한 포트 및 설정 정보를 인식.
  - `[톰캣폴더]/conf/server.xml`
2. 모든 프로젝트에 공통적으로 적용되는 설정 정보 인식.
  - `[톰캣폴더]/conf/web.xml`
3. 모든 프로젝트에 공통으로 적용되는 라이브러리 파일을 인식. 더불어 %JAVA_HOME%lib,%JAVA_HOME%/jre/lib/ext 폴더 내의 jar 파일들도 자동으로 인식.
  - `[톰캣폴더]/common/lib`
4. 프로젝트별 환경 설정 정보를 인식.
  - `[프로젝트이름]/WEB-INF/web.xml`
5. 프로젝트별 라이브러리를 인식.
  - `[프로젝트이름]/WEB-INF/lib`
6. 프로젝트 별로 적용되는 서블릿 파일을 인식. 설정에 따라 init()을 실행.
  - `[프로젝트이름]/WEB-INF/classes`

### Servelt 웹 서버 종료 순서
1. 프로젝트별로 적용되는 서블릿 파일을 인식하고 destroy()를 실행하여 메모리를 해제.
  - `[프로젝트이름]/WEB-INF/classes`
2. 프로젝트별로 환경 설정에 사용된 메모리를 해제.
3. 모든 프로젝트에 공통적인 환경을 설정하기 위해 사용된 메모리를 해제.
4. 웹 서버를 구동하기 위해 열어둔 포트를 닫음.


### Links
- [Head First Servlet & JSP](http://book.naver.com/bookdb/book_detail.nhn?bid=5902081)
- [edwith, Full-Stack Web Developer servlet](https://www.edwith.org/boostcourse-web/lecture/16686/)
-  [Servlet 이란 무엇인가?](http://breath91.tistory.com/entry/Servlet-이란-무엇인가)
- [Servlet이란](https://showbang.github.io/typistShow/2017/01/11/%ED%8F%B4%EB%8D%94%ED%85%8C%EC%8A%A4%ED%8A%B8/)
- [web application 등장 배경 및 구동 원리와 tomcat설치 및 간단한 web app 프로그램 만들기.](http://ktk08.blogspot.com/2013/08/web-application-tomcat-web-app.html?m=1)
- [웹 서버의 구동, 종료와 서블릿의 라이프 사이클](http://bloodygale.tistory.com/entry/Servlet-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%9D%98-%EA%B5%AC%EB%8F%99-%EC%A2%85%EB%A3%8C%EC%99%80-%EC%84%9C%EB%B8%94%EB%A6%BF%EC%9D%98-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4)
