목차
=============
* [설치](#설치)<br>
* [기본 세팅](#기본-세팅) <br>
* [Servlet/JSP 기초](#Servlet/JSP-기초) <br>

<br>


## 설치

<br>

### 톰캣 

<br>

[Apache Tomcat® - Welcome! 홈페이지로 이동](http://tomcat.apache.org/) <br>

<br>

* 좌측 Tomcat 8.5 선택

* 톰캣 8.5.47 다운 - zip 눌러서 다운로드

* 임의의 드라이브 dev 폴더 생성 후 해당 경로로 압축 해제(D 드라이브로 진행함)

<br>
<hr>
<br>

## 기본세팅

<br>

### 이클립스 실행 -> window -> preferences -> Server -> runtime environments

* 버전 8.5 선택 -> browse(폴더 선택) -> finish.(이클립스에 톰캣 연동만 한 상태)

<br>

window -> show view -> Servers 탭 생성 -> No servers~new servers 클릭 

* 버전 맞춰서 8.5 선택 후 finish -> Servers에서 Tomcat~~ 더블 클릭 하면

Overview 출력됨

<br>

여기서 HTTP/1.1 포트 8080 -> 8800으로 수정 ( 오라클에서 8080 포트를 사용하므로

포트를 변경함으로서 충돌 방지 )

* Serve modules without publishing를 체크(톰 캣 경로를 기본으로 잡으므로, 

임의로 지정한 프로젝트 경로로 사용하기 위해 체크) 

* ctrl + s 를 통해 저장

* Timeouts 설명(환경에서 톰캣이 실행되는데 시간이 오래 걸리는 경우 

Timeouts로 인해 실행되지 않고 종료될 수 있음)

<br>

서버 시작 -> 방금과 같이 우클릭 후 start / ctrl + alt + r

<br>

### 이클립스

<br>

환경 -> Java -> Java EE로 변경 새 워크스페이스라면 default - Java EE

<br>

* Window -> Perspective -> Customize Perspective -> Shortcuts

<br>

<b>Java(Java project from~제외 전체 체크)</b>

<b>Web(Static~체크 해제, JSP/HTML/CSS File, Dynamic Web~~ 체크)</b>

<b>XML(XML File만 체크)</b>

<b>GIT 체크</b>

<b>Server 체크</b>

<br>

이후 체크 부분은 프로젝트 생성하려 할 시 new 우측에 바로 

나타나게 됨 필요한 부분은 임의로 체크/언체크 해주면 됨

<br>

general -> editors -> text edditer -> spelling -> utf-8 설정

web -> css,html,jsp -> utf-8 설정

xm -> xml files -> utf-8 설정

<br>

### 프로젝트 생성

<br>

new Dynamic Web Project를 통해서 생성- > 

next -> WebContent\WEB-INF\classes 입력을 통해서 경로 변경

-> next -> generate web.xml ~~ 체크 (web.xml이라는 생성 파일을 만들어주라는 의미)

<br>

이후 Servers 에서 톰캣 우클릭 후 add and remove -> 새로 만든 프로젝트 생성 후 

-> 서버 재 시작 -> 생성한 프로젝트의 WebContent 우클릭 Html file 선택 후

index.html을 선택함으로 생성

<br>

servlet 만들 때 next 한 뒤 url mapping에서 /임의의 이름으로 변경

<br>

* JSP는 에러 반영이 느림, 오류를 수정했음에도 빨간 원(X)등이 사라지지 않는다면

잘랐다가 붙여넣기, JSP 파일을 닫았다 열면 해결 됨

<br>
<hr>
<br>

## Servlet/JSP 기초

<br>

### 서블릿?

웹프로그래밍에서 클라이언트의 요청을 처리하고 그 결과를 다시 클라이언트에게 

전송하는 Servlet 클래스의 구현 규칙을 지킨 자바 프로그래밍 기술

* 톰캣 서버에서 구동

* javax.servlet.http.HttpServlet 클래스를 상속 받는다.

<br>

### WebContent 하위에 index.html을 생성한다.

<br>

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2><a href="views/test.html"></a></h2>
</body>
</html>
```

<br>

### WebContent/views/test

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>테스트용 html 입니다.</h2>
	<form action="/test/testF" method="post" name="testForm">
		<input type="text" name="text" size="5">
		<br>
		<input type="submit" name="btn" id="btn" value="제출">
	</form>
</body>
</html>
```

<br>

### src/test/testForm.java(Servlet로 생성)

<br>

```java
package test;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class testForm
 */
//@WebServlet("/testF")
public class testForm extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testForm() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		String text = request.getParameter("text");
		
		request.setAttribute("text", text);
		
		RequestDispatcher view = request.getRequestDispatcher("views/testAll.jsp");
		view.forward(request, response);
		
		doGet(request, response);
	}

}
```

<br>

### WebContent/views/testAll.jsp

<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String text = (String)request.getAttribute("text");

%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>입력하신 값은</title>
</head>
<body>
	<h2>입력하신 값은 <%= text %> &nbsp; 입니다.</h2>
</body>
</html>
```

<br>

### WEB-INF/web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>testSerJsp</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
  	<servlet-name>testF</servlet-name>
	<servlet-class>test.testForm</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>testF</servlet-name>
  	<url-pattern>/testF</url-pattern>
  </servlet-mapping>
</web-app>
```

<br>

web.xml에서 매핑을 추가시킴으로 test.html에서 입력한 값을 매핑을 통해 전달시킬 수 있다.

action="/test/testF" 여기서 test는  context root 이름이며 /testF는 매핑된 이름을 의미한다.

경로를 찾아갈 때는 WebContent(/test) -> class -> test -> testForm.class(/testF)를 찾아가게 된다.

<br>

* web.xml을 수정하는 것보다 더 간단한 방법은

<br>

testForm.java의 상단에 다음과 같이 수정을 시켜주면 된다.

<b>xml에 추가하는 것과의 차이는 아마도 편의성이나 한꺼번에 모아놓아서 보는 것의 차이가 아닐까 싶다.</b>

```xml
@WebServlet("/testF")
public class testForm extends HttpServlet {
```

<br>
