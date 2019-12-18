## 회원정보 수정, 비밀번호 변경, 탈퇴하기 

<br>

### WebContent/views/common/testMain.jsp

<br>

```java
<%@page import="test.model.vo.Test"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%
	String cp = request.getContextPath();
	Test loginTest = (Test) session.getAttribute("loginTest");
	String msg = (String)session.getAttribute("msg");
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script type="text/javascript">
	var msg = "<%= msg %>";
	$(function(){
		if(msg != "null"){
			alert(msg);
			<% session.removeAttribute("msg"); %>
		}
	});
</script>
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
	<h1 align="center" style="color:#f5830a;">테스트 메인 메뉴</h1>

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
				<button id="testJoin" onclick="newJoin();" type="button">회원가입</button>
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
					onclick="location.href='<%=cp%>/views/tMember/chgMy.jsp';">정보수정</button>
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
		
		function newJoin(){
			console.log(1);
			location.href ="<%= cp%>/views/tMember/testMemberJoin.jsp";
		}
	</script>
</body>
</html>
```
<br>

### WebContent/views/common/pwdUpdateForm.jsp

<br>

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String msg = (String)request.getAttribute("msg");
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<title>Insert title here</title>
<script type="text/javascript">
	var msg = "<%= msg %>";
	
	$(function(){
		if(msg != 'null'){
			alert("비밀번호 변경 성공");
		}
		
		if(msg == "true"){
			window.close();
		}
	});
</script>
</head>
<style>
	h3{
		text-align: center;
	}
	table{
		margin:auto;
	}
	td{
		text-align: right;
	}
	
	button{
		height: 21px;
		width: 100px;
		background: yellowgreen;
		border: yellowgreen;
		color: white;
		border-radius: 5px; 
	}
	button:hover{
		cursor:pointer;
	}
</style>
<body>
	<h3>비밀번호 변경</h3>
	<br>
	<form id="updatePwdForm" action="<%= request.getContextPath() %>/updatePwd.me" method="post" onsubmit="return checkPwd()">
		<table>
			<tr>
				<td><label>현재 비밀번호</label></td>
				<td width="30"></td>
				<td><input type="password" name="pwd" id="pwd" maxlength="15"></td>
			</tr>
			<tr>
				<td><label>변경할 비밀번호</label></td>
				<td width="30"></td>
				<td><input type="password" name="nPwd" id="nPwd" maxlength="15"></td>
 			</tr>
 			<tr>
				<td><label>변경할 비밀번호 확인</label></td>
				<td width="30"></td>
				<td><input type="password" name="nPwd2" id="nPwd2" maxlength="15"></td>
 			</tr>
		</table>
		<br><br>
		<div class="btns" align="center">
			<button id="updatePwdBtn">변경하기</button>
		</div>
	</form>
	<script>
		function checkPwd(){
			var pwd = $("#pwd");
			var nPwd = $("#nPwd");
			var nPwd2 = $("#nPwd2");
			
			if(pwd.val().trim() == "" || nPwd.val().trim() == "" || nPwd2.val().trim() == ""){
				alert("비밀번호를 입력해주세요.");
				return false;
			}
			
			if(nPwd.val() != nPwd2.val()){
				alert('비밀번호가 다릅니다.');
				nPwd2.select();
				return false;
			}
			
			return true;
			
		}
	</script>

</body>
</html>
```

<br>

### src/test/controller/pwdUpdateServlet.java

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
 * Servlet implementation class pwdUpdateServlet
 */
@WebServlet("/updatePwd.me")
public class pwdUpdateServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public pwdUpdateServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		HttpSession session = request.getSession();
		
		String userId = ((Test)session.getAttribute("loginTest")).gettId();
		request.setCharacterEncoding("utf-8");
		
		String pwd = request.getParameter("pwd");
		String nPwd = request.getParameter("nPwd");
		
		Test upT = new TestService().updatePwd(userId, pwd, nPwd);
		
		if(upT != null) {
			request.setAttribute("msg", "true");
			request.getSession().setAttribute("loginTest", upT);
		}else {
			request.setAttribute("msg", "비밀번호 변경 실패");
		}
		
		request.getRequestDispatcher("views/tMember/pwdUpdateForm.jsp").forward(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
​

src/test/controller/testUpdateServlet.java

package test.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import test.model.service.TestService;
import test.model.vo.Test;

/**
 * Servlet implementation class testUpdateServlet
 */
@WebServlet("/update.me")
public class testUpdateServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testUpdateServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		String id = request.getParameter("testId");
		String name = request.getParameter("name");
		String phone = request.getParameter("phone");
		String email = request.getParameter("email");
		
		String[] irr = request.getParameterValues("interest");
		String inter = "";
		
		if(irr != null) {
			inter = String.join(",",irr);
		}
		
		Test t = new Test(id,name,phone,email,inter);
		
		Test upTest = new TestService().updateTest(t);
		
		if(upTest != null) {
			request.getSession().setAttribute("msg", "회원정보 수정완료");
			request.getSession().setAttribute("loginTest", upTest);
			response.sendRedirect(request.getContextPath());
		}else {
			request.setAttribute("msg", "회원정보 수정에 실패하였습니다.");
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

### src/test/controller/testDeleteServlet.java

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
 * Servlet implementation class testDeleteServlet
 */
@WebServlet("/delete.me")
public class testDeleteServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testDeleteServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		Test t = (Test)request.getSession().getAttribute("loginTest");
		
		int result = new TestService().deleteTest(t.gettId());
		
		if(result > 0) {
			request.getSession().removeAttribute("loginTest");
			request.getSession().setAttribute("msg", "회원 탈퇴 완료");
			response.sendRedirect(request.getContextPath());
		}else {
			request.setAttribute("msg", "회원 탈퇴 실패");
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

## src/test/model/service/TestService.java

<br>

```java
	public Test updateTest(Test t) {
		Connection con = getConnection();
		Test upT = null;
		
		int result = td.updateTest(con, t);
		
		if(result>0) {
			upT = td.selectTest(con, t.gettId());
			commit(con);
		}else {
			rollback(con);
		}
		
		close(con);
		
		return upT;
	}

	public Test updatePwd(String userId, String pwd, String nPwd) {
		Connection con = getConnection();
		Test upT = null;
		
		int result = td.updatePwd(con, userId, pwd, nPwd);
		
		if(result > 0) {
			upT = td.selectTest(con, userId);
			commit(con);
		}else {
			rollback(con);
		}
		close(con);
		return upT;
	}

	public int deleteTest(String gettId) {
		Connection con = getConnection();
		
		int result = td.deleteTest(con, gettId);
		
		if(result > 0) {
			commit(con);
		}else {
			rollback(con);
		}
		
		close(con);
		return result;
	}
```

<br>

### src/test/model/service/TestDao.java

```java
	public int updateTest(Connection con, Test t) {
		int result = 0;
		PreparedStatement pstmt = null;
		String sql = prop.getProperty("updateTest");

		try {
			pstmt = con.prepareStatement(sql);

			pstmt.setString(1, t.gettName());
			pstmt.setString(2, t.getPhone());
			pstmt.setString(3, t.getEmail());
			pstmt.setString(4, t.getInterest());
			pstmt.setString(5, t.gettId());

			result = pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(pstmt);
		}

		return result;
	}

	public Test selectTest(Connection con, String gettId) {
		Test t = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		String sql = prop.getProperty("selectTest");

		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, gettId);

			rs = pstmt.executeQuery();

			if (rs.next()) {
				t = new Test(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getString(4), rs.getString(5),
						rs.getString(6), rs.getString(7), rs.getDate(8), rs.getDate(9), rs.getString(10));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
		}

		return t;
	}

	public int updatePwd(Connection con, String userId, String pwd, String nPwd) {
		PreparedStatement pstmt = null;
		int result = 0;

		String sql = prop.getProperty("updatePwd");

		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, nPwd);
			pstmt.setString(2, userId);
			pstmt.setString(3, pwd);

			result = pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(pstmt);
		}
		return result;
	}

	public int deleteTest(Connection con, String gettId) {
		PreparedStatement pstmt = null;
		int result = 0;
		String sql = prop.getProperty("deleteTest");
		
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, gettId);
			
			result = pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(pstmt);
		}
		return result;
	}
```

<br>
