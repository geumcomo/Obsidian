```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<header>
			<h1>로그인</h1>
		</header>
		<article>
			<form action="ex2_login_ok.jsp" method="post">
				<table>
					<caption>로그인테이블</caption>
					<colgroup>
						<col width="90px" />
						<col width="*" />

					</colgroup>
					<tbody>
						<tr>
							<td><label for="s_id">아이디:</label></td>
							<td><input type="text" id="s_id" name="m_id"
								placeholder="사용자ID 입력" /></td>
						</tr>
						<tr>
							<td><label for="s_pw">비밀번호:</label></td>
							<td><input type="text" id="s_pw" name="m_pw"
								placeholder="사용자PW 입력" /></td>
						</tr>
						<tr>
							<td cols="2">
								<button type="button" onclick="login(this.form)">로그인</button> <!-- 현재객체가 포함된 폼을 주겠다 원래 버튼인데 -->
							</td>

						</tr>
					</tbody>
				</table>
			</form>
		</article>
	</div>
	<script>
		function login(frm) { //인자로 버튼이 포함된 폼객체가 들어온다. 
			//유효성검사
			let id = frm.m_id.value; //폼에있는 이름의 값을 가져오고
			if (id.trim().length < 1) { //사길 공백제거는 정규표현식에 의해 제거하는 것이 바람직하다. 

				alert("아이디를 입력하세요");
				frm.m_id.value = "";
				frm.m_id.focus();

				return; //수정보완을 해놓고 이제 하지마!

			}
			let pw = frm.m_pw.value; //폼에있는 이름의 값을 가져오고
			if (pw.trim().length < 1) { //사길 공백제거는 정규표현식에 의해 제거하는 것이 바람직하다. 

				alert("아이디를 입력하세요");
				frm.m_pw.value = "";
				frm.m_pw.focus();

				return; //수정보완을 해놓고 이제 하지마!

			}
			//document.forms[0].submit();
			frm.submit();
		}
	</script>
</body>
</html>
```
# login ok
```jsp
<%@page import="mybatis.vo.MemVO"%>
<%@page import="java.util.List"%>
<%@page import="org.apache.ibatis.session.SqlSession"%>
<%@page import="mybatis.service.FactoryService"%>
<%@page import="org.apache.ibatis.session.SqlSessionFactory"%>
<%@page import="java.util.HashMap"%>
<%@page import="java.util.Map"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
//전달해오는 파라미터들 받기
String id = request.getParameter("m_id");
String pw = request.getParameter("m_pw");

Map<String, String> map = new HashMap<>();
map.put("mid", id);
map.put("mpw", pw);

SqlSession ss = FactoryService.getFactory().openSession();

MemVO mvo = ss.selectOne("member.login", map);
//위의mbo가 null이면 아이디 또는 비밀번호가 틀린경우다.
//null이 아니라면 아이디와 비밀번호를 정혹히 입력한경우다.
String path = "ex2_login_after.jsp";
if(mvo != null){
	session.setAttribute("mvo", mvo);
}else{
	//로그인 실패한 페이지로 이동하기 위해 준비
	path = "ex2_login_fail.jsp";
}
//다른페이지로 강제 이동
response.sendRedirect(path);
%>



</body>
</html>
```

# login after
```jsp 
<%@page import="mybatis.vo.MemVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
.name {
	font-weight: bold;
	color: #00bfff;
}
</style>
</head>
<body>
	<%
	//세션에서  mvo라는 키로 저장된 값을 얻어낸다.
	Object obj = session.getAttribute("mvo");
	if (obj != null) {
		MemVO vo = (MemVO) obj; //obj는 에는 이름이 없기 때문에 형변환으로 강등시켜서 MemVO데이터를 가져온다.
		String name = vo.getM_name();
	%>
	<h2>
		<span class="name"><%=name%></span>님 환영합니다.
	</h2>
	<button type="button"
		onclick="javascript:location.href='ex2_login.jsp'">로그아웃</button>
	<%
	} else {//obj가 null인 경우
	%>
	<h2>잘못된 정보입니다.</h2>
	<button type="button"
		onclick="javascript:location.href='ex2_login.jsp'">로그인</button>
	<%
	}
	%>
	<script>
		/*function login(){
		location.href="ex2_login.jsp";
		} */
	</script>
</body>
</html>
```

# login fail
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
<h2>아이디 또는 비밀번호가 틀립니다</h2>
<button type="button" onclick="javascript: location.href= 'ex2_login.jsp'">로그인</button>
</body>
</html>
```