## ex4
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%--파라미터 값을 JSTL변수에 저장 --%>
<c:set var="s_id" value="${param.s_id}"/>

<c:if test="${s_id eq 'admin'}"><%-- ==는 eq와 같다. --%>
<h2>${s_id}관리자님 환영합니다.</h2>
</c:if>
<c:if test="${s_id ne 'admin'}"><%-- !=는 ne와 같다. --%>
<h2>${s_id}회원님 환영합니다.</h2>
</c:if>




</body>
</html>
```
![[Pasted image 20240611113450.png]]
## ex5
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>JSTL의 for문 연습</h1>
<c:set var="cnt" value="10" scope="page"/>
<c:set var="str" value="<strong>쌍용교육센터</strong>" scope="page"/>

<ul>
<c:forEach begin="1" end="${cnt}" varStatus="st"><%--1부터 10까지  --%>
<li>${st.index}/
	<c:out value="${st.index}"/>/
	<c:out value="${str}" escapeXml="false"/>
	
	
</li>
</c:forEach>	
</ul>
</body>
</html>
```
![[Pasted image 20240611115114.png]]
## ex8
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%--
현재페이지는 money 라는 파라미터를 받는다.
money에 따라 과일을 선택할 수 있다.
사과: 1000
배: 1700
샤인머스켓: 2500

 --%>
	<c:choose>
		<c:when test="${param.money >= 2500 }">
 		사과, 배 , 샤인머스켓 중 하나를 선택!
 		
 	</c:when>
		<c:when test="${param.money >= 1700 }">
 		사과, 배  중 하나를 선택!
 		
 	</c:when>
		<c:when test="${param.money >= 1000 }">
 		사과 하나를 선택!
 		
 	</c:when>
		<c:otherwise>
 	선택할수 과일이 없습니다.
 	</c:otherwise>
	</c:choose>
	<%--범위비교를 if문으로 한다면 범위를 정확히 해줘야한다. 많이 불편하다--%>
	<c:if test="${param.money >= 2500 }">
 사과, 배 , 샤인머스켓 중 하나를 선택!
 </c:if>
	<c:if test="${param.money >= 2500 and param.money >= 1700 }">
 사과, 배  중 하나를 선택!
 </c:if>
	<c:if test="${param.money < 1700 and param.money >= 1000 }">
 사과 하나를 선택!
 </c:if>
	<c:if test="${param.money < 1000 }">
 	선택할수 과일이 없습니다.
 </c:if>

</body>
</html>
```

## ex9
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
    <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%--
현재 페이지는 str이라는 파라미터를 받는다 받은 파라미터의 길이를 구해보자!

 --%>
 <h2> str파라미터 값: ${param.str}</h2>
 <h2> str의 길이: ${param.str.length()}</h2>
 <h2> str.substring(0,3): ${param.str.substring(0,3)}${param.str.substring(5,7)}</h2>
 <h2> str.index("E"): ${param.str.indexOf("E")}</h2>
 <h2> str.replace("E","e"): ${param.str.replace("E","e")}</h2>
 
 <%-- JSTL변수 선언한 후 파라미터 값을 저장하자! --%>
<h2>JSTL변수 선언</h1>  
 <c:set var="s1" value="${param.str}"/>
 <h2>fn:substring(s1,0,3): ${fn:substring(s1,0,3)}</h2>
 <h2>fn:indexOf(s1,"E"): ${fn:indexOf(s1,"E")} </h2>
 <h2>fn:replace(s1,"E","e"): ${fn:replace(s1,"E","e")}</h2>
 <h2>fn:toLowerCase(s1): ${fn:toLowerCase(s1)}</h2>
 <h2>fn:startsWith(s1, "A"): ${fn:startsWith(s1,"A")}</h2>
 <h2>fn:join(배열, 구문자): ${fn:join(paramValues.hobby,"와") }</h2>
</body>
</html>
```
![[Pasted image 20240611154621.png]]

## ex10
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
    <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:set var="msg" value="사과,배,딸기,샤인머스켓"/>


<c:set var="ar" value="${fn:split(msg,',')}"/>
<ul>
<c:forEach var="item" items="${ar}">
<li>${item }<li>
</c:forEach>
</ul>
<hr/>
<%
request.setAttribute("fruits", "사과,배,딸리,샤인머스켓");
%>
<ol>
<c:forTokens var="item" items="${fruits}" delims=",">
<li>${item}<li>
</c:forTokens>
</ol>
</body>
</html>
```
![[Pasted image 20240611154823.png]]

## ex11
```jsp
<%@page import="java.util.Date"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:set var="now" value="<%=new Date() %>"/>
<h2>${now}</h2>
<h2></h2><fmt:formatDate value="${now }"/></h2>
<h2><fmt:formatDate value="${now}" pattern="yyyy-MM-dd" /></h2>
<h2><fmt:formatDate value="${now}" pattern="(a)hh:mm:ss"/></h2>
<h2><fmt:formatDate value="${now}" pattern="HH:mm:ss"/></h2>
<h2>------------------숫자표현--------------------------</h2>
<c:set var="money" value="1200000000.313000"/>
<h2><fmt:formatNumber value="${money}" /></h2>
<h2><fmt:formatNumber value="${money}" groupingUsed="false"/></h2>
<h2><fmt:formatNumber value="${money}" pattern="#,##0.00"/></h2><%--#에 아무것도 안들어오면 무시 0은 안들어오면 0으로 표기 --%>
<h2><fmt:formatNumber value="0.195" pattern="0.00%" type="percent"/></h2>
<h2><fmt:formatNumber value="${money}" type="currency" currencySymbol="$"/></h2>
</body>
</html>
```

![[Pasted image 20240611154953.png]]
