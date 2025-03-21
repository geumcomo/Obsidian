![image](/assets/img/2025-03-21-ex6Interceptor/Pasted-image-20240719165843.png)
src/main/java에 intercept 클래스 생성하자
superclass에 handlerInterceptor를 추가해주자

생성하고 override/implement Methods 클릭
![image](/assets/img/2025-03-21-ex6Interceptor/Pasted-image-20240719094654.png)
1.  **preHandle()** 메서드는  컨트롤러가 호출되기 전에 실행됩니다. 

    매개변수 obj는 Dispatcher의 HandlerMapping 이 찾아준 컨트롤러의 메서드를 참조할 수 있는 HandlerMethod 객체입니다.

  

2.  postHandle() 메서드는 컨트롤러가 실행된 후에 호출됩니다.

  

3.  afterComplete() 은 뷰에서 최종 결과가 생성하는 일을 포함한 모든 일이 완료 되었을 때 실행됩니다.

```java
public class Logininterceptor extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		boolean res = true;
		//로그인 체크를 해서 만약 ! 로그인이 안된 상태이면 res에 false를 저장한다.
		//먼저 로그인 체크를 하기 위해 HttpSession을 얻어내자
		 HttpSession session = request.getSession(true);
		 //true의 의미는 만약에 session이 삭제된 상태라면 새로운 세션을 생성하라는 뜻!
		//예를 들어 로그인시 세션에 "mvo"를 저장했다고 가정을 하면 
		 //여기서는 session에서 "mvo"를 얻어내면 된다. 
		 Object obj = session.getAttribute("mvo");
		 if(obj == null) {
			 //로그인을 안한거
			 response.sendRedirect("/login");
			 res = false;
		 }
		return res; 
		//true를 반환하면 원래 가려고 했던 경로로 진행을 계속 하지만 , false이면
		//원래 경로를 가지않는다.
   	}

}

```

![image](/assets/img/2025-03-21-ex6Interceptor/Pasted-image-20240719171403.png)
