```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
<%--HttpSession은  HttpServletRequest와 마찬가지로 
JSP에서는 내장 객체다. --%>
<%
	
	String name = "마루치";
//세션에 저장하자
	session.setAttribute("u_name", name);
//세션에 "u_name"이라는 명칭으로 name이 기억하고있는 "마루치"를 저장한 상태
%>
<a href="ex1_session1.jsp">다음페이지</a>
<%--이동할때 request response 버리고 새로 만들저짊 --%>
</body>
</html>
```

# session1
```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
<%
//세션의 getAttribute 메서드는 세션에 저장식 어떤 객체를 
//저장했는지 몰라서 최상위 객체인 Object를 반환한다. 그래서
//저장시 자료형을 형변환을 해야 제대로 사용가능하다.
//개발자는 저장할 때 어떤 자료형으로 저장했는지> 알고 있다.
Object obj = session.getAttribute("u_name");
//obj가 null 이 아닐때만 형변환하자!
if(obj != null){
	String name = (String)obj;
%>

<%=name %>님 환영합니다.<br/>
<button type ="button" onclick="logout('<%=name%>')">로그아웃</button> <%--자바의 영역에서 name은 변수이기때문에 html에 문자열로 출력해야되기때문에 <%= --%>
<%
}
%>
<script>
	function logout(n){
		//자바스크립트에서 페이지 이동을 어떻게 할까.
		location.href="ex1_session2.jsp?name="+n; //값을 전달해주고싶다. parameter 자바스크립트= n
	}
</script>
</body>
</html>
```

# session2
```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
<%
//세션으로부터 "u_name"이라는 명칭으로 저장된 값을 삭제하자!
session.removeAttribute("u_name");

//별도로 전달되는 name이라는 파라미터 받기

String name = request.getParameter("name");

%>
<h2><%=name%>+"님 안녕히가세요"</h2>
<a href="ex1_session3.jsp">다음페이지</a>
</body>
</html>
```

# session3
```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"
	pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
	<%
	//세션으로부터"u_name"이라는 명칭으로 저장된 값을 얻어내자
	Object obj = session.getAttribute("u_name");
	if (obj == null) {
	%>
	<h3>저장된 값이 없습니다.</h3>
	<%
	} else {
	String name = (String) obj;
	%>
	<h3><%=name%>님 반갑습니다.
	</h3>

	<%
	}
	%>
</body>
</html>
```