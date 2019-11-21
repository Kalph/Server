## &lt;회원가입 폼 구현&gt;

<br>

## WebContent/views/common/testMain.jsp

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
		
		function newJoin(){
			console.log(1);
			location.href ="<%= cp%>/views/tMember/testMemberJoin.jsp";
		}
	</script>
</body>
</html>

```
<br>
<hr>
<br>

## WebContent/views/tMembe/testMemberJoin.jsp

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
					<td width="150px"><button id="idCheck" type="button" onclick="checkId();">중복확인</button></td>
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
				<button id="Main" onclick="returnMain();" type="button">메인으로</button>
				<button id="joinBtn">가입하기</button>
			</div>
		</form>
	</div>
	
	<script type="text/javascript">
		function returnMain(){
			location.href="<%= cp %>";
		}
		
		function chkJoin(){
			if(!/^[a-z가-힣]{6,11}$/.test($("#testJoinForm input[name=testId]").val())){
				alert('아이디는 최소 영/숫자로 시작하여 6자부터 12자');
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
		
		$("#pwd2"){
			console.log("123");
		}
		
		function checkId(){
			window.open("idCheckForm.jsp","checkForm","width=300, height=200");
		}
	</script>

</body>
</html>
```
