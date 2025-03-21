---
---

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<%@ page import="java.util.List"%>
<%@ page import="am.vo.DeptVO"%>
<%@ page import="org.apache.ibatis.session.SqlSession"%>
<%@ page import="pm.service.FactoryService"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>부서 목록</title>
<style>
table {
	border-collapse: collapse;
	width: 100%;
}

th, td {
	border: 1px solid black;
	padding: 8px;
	text-align: left;
}

th {
	background-color: #f2f2f2;
}
</style>
</head>
<body>
	<h2>부서 목록</h2>
	<table>
		<thead>
			<tr>
				<th>부서코드</th>
				<th>부서명</th>
				<th>도시코드</th>
			</tr>
		</thead>
		<tbody>
			<%
			SqlSession ss = FactoryService.getFactory().openSession();
			List<DeptVO> deptList = ss.selectList("dept.search");
			for (DeptVO dept : deptList) {
			%>
			<tr>
				<td><%=dept.getDeptno()%></td>
				<td><%=dept.getDname()%></td>
				<td><%=dept.getLoc_code()%></td>
			</tr>
			<%
			}
			%>
		</tbody>
	</table>
</body>
</html>

```
## 출력화면
![image](/assets/img/2025-03-21-deptlistjsp/Pasted-image-20240524002230.png)