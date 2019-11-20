## 테이블 세팅

<br>

```sql
drop table test;

CREATE TABLE test (
  t_no NUMBER PRIMARY KEY,
  t_id VARCHAR2(30) NOT NULL,
  t_pwd VARCHAR2(100) NOT NULL,
  t_name VARCHAR2(15) NOT NULL,
  phone VARCHAR2(13),
  email VARCHAR2(100),
  interest VARCHAR2(100),
  en_date DATE DEFAULT SYSDATE,
  mo_date DATE DEFAULT SYSDATE,
  st VARCHAR2(1) DEFAULT 'Y'
);

create sequence test_no;

insert into test values(test_no.nextval,'admin','1234','관리자','01012345678','admin@12.kr',null,sysdate,default,default);
```
 
<br>
<hr>
<br>
 

### WebContent/resources/images/임의의 이미지 삽입

<br>
<hr>
<br>

### WebContent/index.jsp

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
<body>
	<%@ include file="views/common/testMain.jsp" %>
</body>
</html>
```

<br>
<hr>
<br>

### src/test/mode/vo/Test.java

```java
package test.model.vo;

import java.sql.Date;

public class Test {
//	T_NO	NUMBER	No		1	
//	T_ID	VARCHAR2(30 BYTE)	No		2	
//	T_PWD	VARCHAR2(100 BYTE)	No		3	
//	T_NAME	VARCHAR2(15 BYTE)	No		4	
//	PHONE	VARCHAR2(13 BYTE)	Yes		5	
//	EMAIL	VARCHAR2(100 BYTE)	Yes		6	
//	INTEREST	VARCHAR2(100 BYTE)	Yes		7	
//	EN_DATE	DATE	Yes	SYSDATE	8	
//	MO_DATE	DATE	Yes	SYSDATE	9	
//	ST	VARCHAR2(1 BYTE)	Yes	"'Y'
//	"	10	
	private int tNo;
	private String tId;
	private String tPwd;
	private String tName;
	private String phone;
	private String email;
	private String interest;
	private Date enDate;
	private Date moDate;
	private String st;
	
	public Test() {}

	public Test(int tNo, String tId, String tPwd, String tName, String phone, String email, String interest,
			Date enDate, Date moDate, String st) {
		super();
		this.tNo = tNo;
		this.tId = tId;
		this.tPwd = tPwd;
		this.tName = tName;
		this.phone = phone;
		this.email = email;
		this.interest = interest;
		this.enDate = enDate;
		this.moDate = moDate;
		this.st = st;
	}

	public int gettNo() {
		return tNo;
	}

	public void settNo(int tNo) {
		this.tNo = tNo;
	}

	public String gettId() {
		return tId;
	}

	public void settId(String tId) {
		this.tId = tId;
	}

	public String gettPwd() {
		return tPwd;
	}

	public void settPwd(String tPwd) {
		this.tPwd = tPwd;
	}

	public String gettName() {
		return tName;
	}

	public void settName(String tName) {
		this.tName = tName;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getInterest() {
		return interest;
	}

	public void setInterest(String interest) {
		this.interest = interest;
	}

	public Date getEnDate() {
		return enDate;
	}

	public void setEnDate(Date enDate) {
		this.enDate = enDate;
	}

	public Date getMoDate() {
		return moDate;
	}

	public void setMoDate(Date moDate) {
		this.moDate = moDate;
	}

	public String getSt() {
		return st;
	}

	public void setSt(String st) {
		this.st = st;
	}

	@Override
	public String toString() {
		return "Test [tNo=" + tNo + ", tId=" + tId + ", tPwd=" + tPwd + ", tName=" + tName + ", phone=" + phone
				+ ", email=" + email + ", interest=" + interest + ", enDate=" + enDate + ", moDate=" + moDate + ", st="
				+ st + "]";
	}
	
}
```

<br>
<hr><br>

### WebContent/views/common/testMain.jsp

<br>

```jsp
<%@page import="test.model.vo.Test"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%
	String cp = request.getContextPath();
	Test loginUser = (Test) session.getAttribute("loginUser");
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
	
	.loginArea > form, #testeInfo {
		float:right;
	}
	
	#testJoin, #testLogin, #myPage, #logoutBtn{
		width: 100px;
		height: 25px;
		color: white;
		border-radius: 5px;
		margin-top: 5px;
	}
	
	#testJoin, #myPage{
		background: yellowgreen;
	}
	
	#testLogin, #logoutBtn{
		background: orangered;
	}
	
	
</style>
<body>
	<h1 align="center">테스트 메인 메뉴</h1>

	<div class="loginArea">
		<%
			if (loginUser == null) {
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
			<label><%=loginUser.gettName()%>님의 방문을 환영합니다.</label>
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
	</script>
</body>
</html>
```

<br>
