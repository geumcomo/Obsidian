```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div id="wrap">
		<header>
			<h1>로그인</h1>

		</header>
		<div id="content">
			<form action="Ex1_Login" method="post" name= "frm">
				<lable for= "s_id"> 아이디:</lable>
				<input type = "text" id = "s_id" name="mid"/>
				<br/>
				<lable for= "s_pw"> 비밀번호:</lable>
				<input type = "password" id = "s_pw" name="mpw"/>
				<br/>
				<button type="button" onclick="exe(this.form)">로그인</button> <!-- 현재 폼을 던져준다.-->
			</form>
		</div>                      

	</div>
	<script>
	 function exe(frm){
		 //유효성검사
		 let sid = frm.mid;
		 let spw = frm.mpw;
		 if(sid.trim().length< 1){
			 	alert("아이디를 입력하세요")
			 	sid.value = "";청소
			 	return;
		 }
		 if(spw.trim().length< 1){
			 	alert("비밀번호를 입력하세요")
			 	spw.value = "";청소
			 	return;
		 }
		 //action을 바꾸고싶다 frm.action= "Ex1_Service";
		 frm.submit();//서버로 보내기
		 
					 
	
	 }
	</script>
</body>
</html>
```

# Servlet
```java
package am;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;
import java.io.Reader;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import am.vo.MemVO;

/**
 * Servlet implementation class Ex1_login
 */
public class Ex1_Login extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#HttpServlet()
	 */
	public Ex1_Login() {
		super();
		// TODO Auto-generated constructor stub
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
		// 보통 로그인을 수행할때 아이디와 비밀번호는 한글로 하지 않으므로 요청시 한글 처리 생략한다.
		// 1.파라미터들(mid,mpw)받기
		String mid = request.getParameter("mid");
		String mpw = request.getParameter("mpw");

		// db로부터 인증을받기 위해 login mapper를 호출해야 한다.
		Map<String, String> map = new HashMap<>();
		map.put("mid", mid);
		map.put("mpw", mpw);

		Reader r = Resources.getResourceAsReader("am/config/config.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(r);
		SqlSession ss = factory.openSession();

		MemVO mvo = ss.selectOne("member.login", map); // 아이디오 비밀번호 일치할때만 mvo생성
		if (mvo != null) {
			HttpSession session = request.getSession(); // 주소를 준다 주소를 가지고 저장하고 사라진다. session은 서버가 관리 치우고 생성 보안적으로 좋다.
														// 딴데에서 여기있는지 물어보고 확인
			session.setAttribute("mvo", mvo);
		}

	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

## login mapper
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="member">

<!-- 로그인 -->

<select id="login" parameterType="Map"

resultType="am.vo.MemVO">

SELECT * FROM member_t

WHERE m_id = #{mid} AND m_pw = #{mpw}

</select>

</mapper>
```

## logout 
```java
package am;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

/**
 * Servlet implementation class Ex1_Logout
 */
public class Ex1_Logout extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Ex1_Logout() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 세션얻기
		HttpSession session = request.getSession();
		
		//로그인시에 저장했던 값을 삭제
		session.removeAttribute("mvo");
		
		//강제 이동
		response.sendRedirect("Ex1_Service");
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
## Service
```java
package am;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;
import java.io.PrintWriter;

import am.vo.MemVO;

/**
 * Servlet implementation class Ex1_Service
 */
public class Ex1_Service extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Ex1_Service() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		
		//여기는 로그인을 한 회원들만 서비스 하는 곳이다.라고 가정하자!
		//그럼 요청한 사용자가 로그인을 했는지? 안했는지? 판단을 해야 한다.
		// 그 판단은 요청자의 HttpSession을 얻어내어 그 안에
		// "mvo"로 저장한 자원이 있다면 로그인을 수행했다고 봐야 한다.
		HttpSession session = request.getSession();
		//세션으로부터 저장된 속성을 얻어낸다. 이때 키가 "mvo"인 것을 찾아야 함
		Object obj = session.getAttribute("mvo");
		MemVO mvo = null;
		
		if(obj != null) {
			//로그인을 한 상태
			mvo = (MemVO)obj;
			out.println("<h2>"+mvo.getM_name()+"님 환영!</h2>");
			out.println("<button type='button' ");
			out.println(" onclick='javascript:location.href=\"Ex1_Logout\"'>");
			out.println("로그아웃</button>");
		}else {
			//로그인을 안한 상태
			out.println("<h2>로그인이 필요합니다.</h2>");
			out.println("<button type='button' ");
			out.println(" onclick='javascript:location.href=\"ex1_login.html\"'>");
			out.println("로그인</button>");
		}
		out.close();
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
