## 회원가입 구현, 아이디 중복체크 기능

<br>

```prop
sql/test/test-query.properties

loginTest=select * from test where t_id=? and t_pwd=? and st='Y'
idCheck=select * from test where t_id=
insertTest=insert into test values(seq_test.nextval,?,?,?,?,?,?,sysdate,sysdate,default)
```

<br>
<hr>
<br>

## test/controller/idCheckServlet.java
<br>

```java
package test.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import test.model.service.TestService;

/**
 * Servlet implementation class idCheckServlet
 */
@WebServlet("/idCheck.me")
public class idCheckServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public idCheckServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String id = request.getParameter("id");
		
		int result = new TestService().idCheck(id);
		
		request.setAttribute("result", result);
		request.setAttribute("id", id);
		
		request.getRequestDispatcher("views/tMember/idCheck.jsp").forward(request,response);
		
		
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
<hr>
<br>

## test/controller/insertTestServlet.java

<br>

```java
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
 * Servlet implementation class insertTestServlet
 */
@WebServlet("/insert.me")
public class insertTestServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public insertTestServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		String id = request.getParameter("testId");
		String pwd = request.getParameter("pwd1");
		String name = request.getParameter("name");
		String phone = request.getParameter("phone");
		String email = request.getParameter("email");
		String its[] = request.getParameterValues("interest");
		
		String it = "";
		
		if(its != null) {
			it = String.join(",", its);
		}else {
			it = "없음";
		}
		
		Test t = new Test(id,pwd,name,phone,email,it);
		
		int result = new TestService().insertTest(t);
		
		if(result > 0) {
			request.getSession().setAttribute("msg", "회원 가입 성공");
			response.sendRedirect(request.getContextPath());
		}else {
			request.setAttribute("msg", "회원 가입 실패.");
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
<hr>
<br>

##test/model/service/TestService.java

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

	public int idCheck(String id) {
		Connection con = getConnection();
		
		int result = td.idCheck(con, id);
		
		close(con);
		return result;
	}

	public int insertTest(Test t) {
		Connection con = getConnection();
		int result = td.insertTest(con, t);
		
		if(result > 0) {
			commit(con);
		}else {
			rollback(con);
		}
		
		close(con);
		return result;
	}

}
```

<br>
<hr>
<br>


## test/model/dao/TestDao.java

<br>

```java
package test.model.dao;

import java.io.FileReader;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
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
				System.out.println(1);
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

	public int idCheck(Connection con, String id) {
		int result = 0;
		Statement stmt = null;
		ResultSet rs = null;
		
		String sql = prop.getProperty("idCheck");
		
		try {
			stmt = con.createStatement();
			rs = stmt.executeQuery(sql+"'"+id+"'");
			
			if(rs.next()) {
				result = rs.getInt(1);
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(rs);
			close(stmt);
		}
		return result;
	}

	public int insertTest(Connection con, Test t) {
		int result = 0;
		PreparedStatement pstmt = null;
		
		String sql = prop.getProperty("insertTest");
		
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, t.gettId());
			pstmt.setString(2, t.gettPwd());
			pstmt.setString(3, t.gettName());
			pstmt.setString(4, t.getPhone());
			pstmt.setString(5, t.getEmail());
			pstmt.setString(6, t.getInterest());
			result = pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			close(pstmt);
		}
		return result;
	}

}
```

<br>
<hr>
<br>

## WebContent/views/tMember/testMemberJoin.jsp

<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>

	.fo{
		width: 500px;
		height: 500px;
		background: #ffd663;
		color: #40b004;
		margin:auto;
	}
	
	#testJoinform table{
		width: 100%;
		margin:auto;
	}
	
	
	#testJoinForm td{
		text-align: right;
	}
	
	#idCheck, #pwdResult{
		float:left;
	}
	
	input{
		margin-top: 2px;
	}
	
	button{
		height: 25px;
		width: 80px;
		background: yellowgreen;
		border:yellowgreen;
		color: white;
		border-radius: 5px;
	}
	
	button:hover{
		cursor: pointer;
	}

</style>
</head>
<body>
	<%@ include file="../common/testMain.jsp" %>
	
	<div class="fo">
		<br>
		<h2 align="center">회원가입</h2>
		<form id="testJoinForm" name="testJoinForm" action="<%= cp %>/insert.me" method="post" onsubmit="return chkJoin();">
			<table>
				<tr>
					<td width="150px">아이디</td>
					<td><input type="text" maxlength="13" name="testId" required="required"></td>
					<td width="150px"><button id="idCheck" type="button" onclick="checkId()">중복확인</button></td>
				</tr>
				<tr>
					<td>비밀번호</td>
					<td><input type="password" maxlength="15" name="pwd1" required="required"></td>
				</tr>
				<tr>
					<td>비밀번호 확인</td>
					<td><input type="password" maxlength="15" name="pwd2" required="required"></td>
					<td><label id="pwdResult"></label></td>
				</tr>
				<tr>
					<td>이름</td>
					<td><input type="text" maxlength="10" name="name" required></td>
				</tr>
				<tr>
					<td>연락처</td>
					<td><input type="tel" maxlength="11" name="phone" placeholder="전화번호를 입력하세요"></td>
				</tr>
				<tr>
					<td>이메일</td>
					<td><input type="email" name="email"></td>
					<td></td>
				</tr>
				<tr>
					<td>흥미</td>
					<td>
						<input type="checkbox" id="sport" name="interest" value="운동"><label for="sport">운동</label>
						<input type="checkbox" id="game" name="interest" value="게임"><label for="sport">게임</label>
						<input type="checkbox" id="movie" name="interest" value="영화"><label for="movie">영화</label>
						<input type="checkbox" id="else" name="interest" value="기타"><label for="sport">기타</label>
					</td>
					<td></td>
				</tr>
			</table>
			<div class="sub" align="center">
				<button id="Main" onclick="returnMain()" type="button">메인으로</button>
				<button id="joinBtn">가입하기</button>
			</div>
		</form>
	</div>
	
	<script type="text/javascript">
		function returnMain(){
			location.href="<%= cp %>";
		}
		
		function chkJoin(){
			if(!(/^[a-z\d가-힣]{4,121}$/.test($("#testJoinForm input[name=testId]").val()))){
				alert('아이디는 최소 영/한/숫자로 시작하여 4자부터 12자');
				$("#testJoinForm input[name=testId]").select();
				return false;
			}
			
			if($("#testJoinForm input[name=pwd1]").val() != $("#testJoinForm input[name=pwd2]").val()){
				$("#pwdResult").text('비밀번호 불일치').css("color","red");
				return false;
			}else{
				$("#pwdResult").text('비밀번호 일치').css("color","green");
			}
			
			return true;
		}
		
		function checkId(){
			window.open("idCheck.jsp","checkForm","width=300, height=200");
		}
	</script>

</body>
</html>
```

<br>
<hr>
<br>

## WebContent/views/tMember/idCheck.jsp

<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<script type="text/javascript">
	function idValue(){
		var id;
		
		if("<%= request.getAttribute("result") %>" == "null"){
			id = opener.document.testJoinForm.testId.value;
		} else{
			id = "<%= request.getAttribute("id") %> ";
			if(<%= request.getAttribute("result") %> == 0){
				document.getElementById("confirm").removeAttribute("disabled");
			}else{
				document.getElementById("confirm").setAttribute("disabled","disabled");
			}
		}
		
		document.getElementById("id").value = id;
	}
	
	function confirm(){
		if(<%= request.getAttribute("result") %> == 0){
			opener.document.testJoinForm.testId.value = document.getElementById("id").value;
			opener.document.testJoinForm.testId.setAttribute("readonly","readonly");
		}
		
		if(opener != null){
			self.close();
		}
	}
	
</script>
<body onload="idValue();">
	<b>아이디 중복 체크</b>
	<br>
	<form action="<%= request.getContextPath() %>/idCheck.me" id="idCheckForm" method="post">
		<input type="text" id="id" name="id">
		<input type="submit" value="중복확인">
	</form>
	<br>
	
	<% if(request.getAttribute("result") != null) {
		int result = (int)request.getAttribute("result");
		
			if(result>0){
	%>
				이미 사용중인 아이디입니다.
	<%
			} else {
	%>
				사용 가능한 아이디입니다.
	<%
			}
		}
	%>
	
	<br>
	
	<input type="button" id="cancel" value="취소" onclick="window.close();">
	<input type="button" id="confirm" value="확인" onclick="confirm()" disabled>
</body>
</html>
```

<br>
