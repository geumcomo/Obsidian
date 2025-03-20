 - Model VIew Controller
- 무조건 컨트롤러를 불러야된다.
- 
![[Pasted image 20240531102000.png]]
![[Pasted image 20240531102102.png]]

1. 다양한 함수 선언 식
```js
	//function goNext(){}
	//let goNext = function(){}
let goNext = () => {
	location.href="view2.jsp";//redirect
}
```

2. controller1.java
```java
package ex1;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class Controller1
 */
@WebServlet("/Controller1")
public class Controller1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Controller1() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// "type"이라는 파라미터를 받아서 변수 type에 저장!
		String type = request.getParameter("type");
		/*
		 * type의 값이 null이거나 "greet"이면 view.jsp로 경로를 지정, 그렇지 않고, type 값이
		 * "hi"이면 view2.jsp로 지정하자!
		 * */
		String viewPath = null; //경로를 저장할 변수!
		if(type == null || type.equalsIgnoreCase("greet")) {
			viewPath = "/ex1/view1.jsp";
			request.setAttribute("v1", "Hello~");
		}else if(type.equalsIgnoreCase("hi")) {
			viewPath = "/ex1/view2.jsp";
			request.setAttribute("v1", "안녕하세요~");
			
		}
		//MVC패턴에서는 뷰페이지 이동을 forward를 시킨다.
		RequestDispatcher disp = request.getRequestDispatcher(viewPath);
		
		disp.forward(request, response); //forward로 이동!
		
		
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
![[Pasted image 20240531111348.png]]
3. view1.jsp
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
	<h1>페이지1</h1>
	<button type = "button" onclick="goNext()">다음</button>
	<%--request에 v1이라는 이름으로 저장된 값을 출력한다. --%>
	<h2>${v1}</h2>
<script>
	//function goNext(){}
	//let goNext = function(){}
	let goNext = () => {
	location.href="ex1/view2.jsp";//redirect
}
</script>	
</body>

</html>
```

${v1}
내장객체 저장소에서 v1을 다 찾는다.

람다식
```java
public interFace InterA{
	void test();
}
//인터페이스는 접근제한자 public 으로 무조건!
InterA  a = new InterA(){
	public void test(){
		System.out.println("A");
	}
};
//위에 꺼 람다로 바꾼다면
//소괄호와 중괄호 사이에 화살표 모양 만들어주고
//중괄호 없애준다
InterA  a = () -> {
		System.out.println("A");
	
};
```
![[Pasted image 20240531143135.png]]
## `getAttribute` - 이전에 다른 JSP 또는 Servlet 페이지에 설정된 매개 변수를 가져 오는 데 사용됩니다.

기본적으로 하나의 JSP / 서블릿에서 다른 JSP / 서블릿으로 포워딩하거나 이동하는 경우 Object에 넣고 세션 변수에 저장하기 위해 set-attribute를 사용하지 않는 한 원하는 정보를 얻을 수있는 방법이 없습니다.

getAttribute를 사용하여 Session 변수를 검색 할 수 있습니다.

## getAttribute () 와 getParameter ()의 기본적인 차이점은 반환 유형입니다.

```
java.lang.Object getAttribute(java.lang.String name)
java.lang.String getParameter(java.lang.String name)
```

## `request.setAttribute()` 를 사용하여 추가 정보를 추가하고 현재 요청을 다른 자원으로 전달 / 리디렉션 할 수 있습니다.



