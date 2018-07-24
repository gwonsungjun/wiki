

## Spring , static content를 인식하는 방법

- What is the DefaultServlet 
  - The default servlet is the servlet which serves static resources as well as serves the directory listings (if directory listings are enabled). 
  - default servlet은 정적 리소스를 제공하고 디렉토리 목록을 제공하는 서블릿이다.(디렉토리 목록이 활성화 된 경우)

```xml
- $CATALINA_BASE/conf/web.xml에 정의 되어 있다.
- 기본 서블릿을 정의 (일반적으로 서비스 정적 리소스 용)
    <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>
          org.apache.catalina.servlets.DefaultServlet
        </servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>true</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

```xml
- webapp이 시작되면 dafault servlet이 로딩되므로 해당 webapplication의 web.xml에 아래를 적용 시켜준다
- 개발할 당시에는 보통 apache+tomcat이 아닌 tomcat만으로 구성하여 개발해서 static한 파일을 모두 dispatcher servlet이 가져간다는 문제가 있다. 따라서 아래와 같이 설정을 한다. (pattern은 사용하는 확장자에 따라.)

	<servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.css</url-pattern>
        <url-pattern>*.js</url-pattern>
        <url-pattern>*.gif</url-pattern>
        <url-pattern>*.png</url-pattern>
        <url-pattern>*.zip</url-pattern>
    </servlet-mapping>

```

- 이외에도 방법은 여러가지가 있다. 
  - url-pattern에 확장자(extension) 대신 file path로 설정
  - Spring - mvc:default-servlet-handler/, mvc:resources 사용



## 추가) url-pattern에 / (default servlet) 으로 지정된 경우 의미

```xml
    <servlet-mapping>
        <servlet-name>appServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.css</url-pattern>
        <url-pattern>*.js</url-pattern>
        <url-pattern>*.gif</url-pattern>
        <url-pattern>*.png</url-pattern>
        <url-pattern>*.zip</url-pattern>
    </servlet-mapping>
```

- 이와 같이 url-pattern에 / (default servlet) 으로 지정을 하면 어떤 servlet-mapping에도 매핑 되지 않은 부분의 처리(정적 리소스의 경우)를 default servlet (아래 정의한 default) 으로 넘기게 된다. 즉, 바로 디폴트 서블릿으로 가는게 아니고 애플리케이션의 web.xml에서 받아주지못한 모든 요청이 디폴트서블릿으로 전달(servlet mapping에 하나도 걸리지 않는 녀석)
- 그러면 뭐가 남을까? 바로 정적인 리소스만 남게 된다. 
- 톰캣만으로도 이미지 등 정적인 리소스(css, js ...) + jsp를 보여 줄 수 있는 것은 이 default servlet이 있기 때문이다. (tomcat의 conf/web.xml 에 정의된 servlet을 이용을 해서). 
- `즉, servlet-mapping에도 매핑 되지 않은 부분의 처리(정적 리소스의 경우)를 default servlet 으로 넘긴다.`
- default servlet을 web application에 재정의를 하게 되면, implicit Mapping 보다 우선적으로 적용된다.
- 만약 url-patten에 /* 로 설정을 해주게 되면 요청 받는 모든 URL을 처리한다는 의미가 된다. 즉, 정적인 컨텐츠나 jsp에 대한 호출도 dispatcher에서 처리를 하려고 한다는 의미.

```
보통 서블릿 컨테이너에는 스태틱 리소스(HTML, 자바 스크립트, CSS, 이미지 등)를 처리하는 디폴트 서블릿과 JSP를 처리하는 JSP 서블릿이 기본으로 등록되어있다. 디폴트 서블릿은 /에 매핑되어 있고 JSP는 확장자를 이용해서 *.jsp로 되어있다. 그리고 여기에 추가로 서블릿을 등록하면 URL 우선순위(매핑 조건이 긴게 우선)에 따라서 그 서블릿의 매핑에 해당하는 것은 등록된 것으로 가고 나머지는 디폴트 서블릿이나 JSP 서블릿이 담당한다. 
- 참고 : <http://toby.epril.com/?p=1107>
```



- 혹시 해당 글에 문제가 있을시 <sungjunpizz@gmail.com>으로 연락 주시면 감사하겠습니다 :)

## Links

- <http://tomcat.apache.org/tomcat-6.0-doc/default-servlet.html#what>
- <https://okky.kr/article/145481>
- http://toby.epril.com/?p=1107
- [Spring 에서 static content 인식하는 방법](http://i5on9i.blogspot.com/2014/02/spring-30-url-file-path.html)