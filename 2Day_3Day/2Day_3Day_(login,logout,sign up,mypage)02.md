



## &lt;Prop 파일 설정, JDBCTemplate, 로그인, 로그아웃 구현&gt;

<br>

### WebContent/views/testMain.jsp

<br>

```jsp
<%@page import="test.model.vo.Test"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%
	String cp = request.getContextPath();
	Test loginTest = (Test) session.getAttribute("loginTest");
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
</head>
<style>
	body {
		background: url('<%=cp%>/resources/images/cat.jpg');
		background-size: cover;
	}
	
	.loginArea > form, #testInfo {
		float:right;
	}
	
	#testJoin, #testLogin, #myPage, #logoutBtn{
		width: 100px;
		height: 25px;
		color: white;
		border-radius: 5px;
		margin-top: 5px;
	}
	
	#testJoin:hover, #testLogin:hover, #myPage:hover, #logoutBtn:hover{
		cursor:pointer;
	}
	
	#testJoin, #myPage{
		background: yellowgreen;
		border:yellowgreen;
	}
	
	#testLogin, #logoutBtn{
		background: orangered;
		border:orangered;
	}
	
	
</style>
<body>
	<h1 align="center">테스트 메인 메뉴</h1>

	<div class="loginArea">
		<%
			if (loginTest == null) {
		%>
		<form id="loginForm" action="<%=cp%>/login.me" method="post"
			onsubmit="return loginCheck();">
			<table>
				<tr>
					<td><label>ID : </label></td>
					<td><input type="text" name="logId" id="logId"></td>
				</tr>
				<tr>
					<td><label>PWD : </label></td>
					<td><input type="password" name="logPwd" id="logPwd"></td>
				</tr>
			</table>
			<div class="btns" align="center">
				<button id="testJoin" onclick="testLogin();" type="button">회원가입</button>
				<button id="testLogin" type="submit">로그인</button>
			</div>
		</form>
		<%
			} else {
		%>
		<div id="testInfo">
			<label><%=loginTest.gettName()%>님의 방문을 환영합니다.</label>
			<div class="btns" align="center">
				<button id="myPage"
					onclick="location.href='<%=cp%>/views/test/myPage.jsp';">정보수정</button>
				<button id="logoutBtn" onclick="logout();">로그아웃</button>
			</div>
		</div>
		<%
			}
		%>
	</div>

	<script type="text/javascript">
		function loginCheck() {
			if ($("#logId").val().trim().length == 0) {
				alert("아이디를 입력하세요");
				$("#logId").focus();
				return false;
			}

			if ($("#logPwd").val().trim().length == 0) {
				alert("비밀번호를 입력하세요");
				$("#logPwd").focus();
				return false;
			}

			return true;
		}
		
		function logout(){
			location.href= '<%= cp %>/logout.me'
		}
		
		function testLogin(){
			location.href ='<%= cp%>/views/tMember/testMemberJoin.jsp';
		}
	</script>
</body>
</html>
```

<br>
<hr>
<br>

### WebContent/views/common/errorPage.jsp

<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<%
	String msg = (String) request.getAttribute("msg");
%>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2 align="center"><%=msg%></h2>
	<div align="center">
		<button onclick="location.href='<%=request.getContextPath()%>'" style="width:100px">홈으로</button>
	</div>
</body>
</html>
```

<br>
<hr>
<br>

### src/common/JDBCTemplate.java

<br>

```java
package common;

import java.io.FileReader;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCTemplate {
	public static Connection getConnection() {
		Connection con = null;
		Properties prop = new Properties();
		
		
		try {
			prop.load(new FileReader(JDBCTemplate.class.getResource("/sql/driver.properties").getPath()));
			
			Class.forName(prop.getProperty("driver"));
			con = DriverManager.getConnection(prop.getProperty("url"), prop.getProperty("user"), prop.getProperty("password"));
			con.setAutoCommit(false);
		} catch (IOException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
		}
		
		return con;
	}
	
	public static void close(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				conn.close();
			}
		} catch (SQLException e) {
		}
	}
	
	public static void close(Statement stmt) {
		try { 
			if(stmt != null && !stmt.isClosed()) {
				stmt.close();
			}
		} catch (SQLException e) {
		}
	}
	
	public static void close(ResultSet rset) {
		try {
			if(rset != null && !rset.isClosed()) {
				rset.close();
			}
		} catch (SQLException e) {
		}
	}
	
	public static void commit(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				conn.commit();
			}
		} catch (SQLException e) {
		}
		
	}
	
	public static void rollback(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				conn.rollback();
			}
		} catch (SQLException e) {
		}
		
	}
	
}
```

<br>
<hr><br>

### sql/test/test-query.properties

<br>

```properties
loginTest=select * from test where t_id=? and t_pwd=? and st='Y'
```

<br>
<hr><br>

### sql/driver.properties

<br>

```properties
driver=oracle.jdbc.driver.OracleDriver
url=jdbc:oracle:thin:@127.0.0.1:1521:XE
user=test
password=test
```

<br><hr><br>

### src/test/controller/testLoginServlet.java

<br>

```java
package test.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import test.model.service.TestService;
import test.model.vo.Test;

/**
 * Servlet implementation class testLoginServlet
 */
@WebServlet("/login.me")
public class testLoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String logId = request.getParameter("logId");
		String logPwd = request.getParameter("logPwd");
		
		Test loginTest = new TestService().loginTest(logId, logPwd);
		if(loginTest != null) {
			request.getSession().setMaxInactiveInterval(600);
			request.getSession().setAttribute("loginTest", loginTest);
			response.sendRedirect(request.getContextPath());
		} else {
			request.setAttribute("msg", "로그인 실패, 관리자에게 문의해 주세요.");
			request.getRequestDispatcher("views/common/errorPage.jsp").forward(request, response);
		}
				
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

<br>
<hr><br>

### src/test/controller/testLogoutServlet.java

<br>

```java
package test.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class testLogoutServlet
 */
@WebServlet("/logout.me")
public class testLogoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testLogoutServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.getSession().invalidate();
		response.sendRedirect(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

<br>
<hr><br>

### src/test/model/dao

<br>

```java
package test.model.dao;

import java.io.FileReader;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

import test.model.vo.Test;
import static common.JDBCTemplate.*;

public class TestDao {
	private Properties prop = new Properties();

	public TestDao() {
		try {
			prop.load(new FileReader(TestDao.class.getResource("/sql/test/test-query.properties").getPath()));
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public Test loginTest(Connection con, String logId, String logPwd) {
		Test testLogin = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		String sql = prop.getProperty("loginTest");

		try {
			pstmt = con.prepareStatement(sql);

			pstmt.setString(1, logId);
			pstmt.setString(2, logPwd);

			rs = pstmt.executeQuery();

			if (rs.next()) {
				testLogin = new Test(rs.getInt(1), rs.getString(2), rs.getString(3), 
						rs.getString(4), rs.getString(5),rs.getString(6), rs.getString(7), 
						rs.getDate(8), rs.getDate(9), rs.getString(10));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
		}
		return testLogin;
	}

}

```

<br>
<hr><br>

### src/test/model/service

<br>

```java
package test.model.service;

import test.model.dao.TestDao;
import test.model.vo.Test;
import static common.JDBCTemplate.*;

import java.sql.Connection;

public class TestService {
	TestDao td = new TestDao();

	public Test loginTest(String logId, String logPwd) {
		Connection con = getConnection();
		
		Test loginTest = td.loginTest(con,logId,logPwd);
		
		close(con);
		
		return loginTest;
	}

}
```

<br>
