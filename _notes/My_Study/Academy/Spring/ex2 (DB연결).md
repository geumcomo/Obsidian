![[spring-tool-suite-4-4.23.1.RELEASE-e4.32.0-win32.win32.x86_64.self-extracting.jar]]
sts 다운 후 압축 풀기!

marketplace
enterprise다운

---
>[!info] 이 방식은 번거롭다
https://mvnrepository.com/
spring-context 
spring-webmvn
버전 일치! maven 복사 후 porm.xml에 붙여넣기

---
	이클립스 marketplace Spring 설치
	![[Pasted image 20240712103938.png]]
---
lagacy에 mvc가 없을 경우

C:\My_Study\My_Study\Spring_Study\work\.metadata\.plugins\org.springsource.ide.eclipse.commons.content.core
![[https-content.xml]]

추가 후 재실행

---

![[Pasted image 20240712121524.png]]
DB를 servlet-context.xml에 했다가는 속도가 람보르기니를 타고 자전거 속도로 가는거랑 같다

root-context.xml 로 꼭 명시를 해야한다. 

## 왜?
순서적으로 root가 먼저 움직이니까

---
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
<!--appServlet -->
</web-app>

```






