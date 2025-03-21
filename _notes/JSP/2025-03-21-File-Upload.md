---
---



```jsp
<%-- 첨부된 파일을 받기 위해서는 servlets.com에 있는 cos라이브러리가 필요하다. 
    사이트로 왼쪽 메뉴에[COS File upload Library]라는 메뉴를 선택한 후 화면 아래로 내려[download]항목에 있는 표에 cos-22.05.zip을 다운받아 압축해제 후 그 안에 있는 lib/cos.jar파일을 현재 프로젝트의 webapp/WEB-INF/lib에 복사해넣는다.
    --%> 
    <%
    String dir = (String)session.getAttribute("dir");
    //파일이 저장될 위치를 반드시 서버의 절대 경로로 준비해야한다.
    String realPath = application.getRealPath("/members/"+dir);
    
    
    //첨부파일을 받아서 서버에 올리기 위해 필요한 객체를 생성한다.
    MultipartRequest mr = new  MultipartRequest(request, realPath, 1024*1024*5, new DefaultFileRenamePolicy());//같은 파일이면 이름 변경후 저장한다.
    
    //이때 첨부되는 파일들이 지정된 realpath에 저장된다.
    //파일명이 변경될 수도 있으므로 확인하기 위해서
    //먼저 첨부파일 File객체로 얻어낸다.
    File f = mr.getFile("upload");//파라미터명
    
    String f_name= f.getName();//현재 파일명
    
    //변경 전의 파일명
    String o_name = mr.getOriginalFileName("upload");
    
    //페이지 강제이동 : get방식
    response.sendRedirect("myDisk.jsp?cPath="+dir);
    
    %>
```
// ↓ request 객체, ↓ 저장될 서버 경로, ↓ 파일 최대 크기, ↓ 인코딩 방식, ↓ 같은 이름의 파일명 방지 처리 // (HttpServletRequest request, String saveDirectory, int maxPostSize, String encoding, FileRenamePolicy policy) // 아래와 같이 MultipartRequest를 생성만 해주면 파일이 업로드 된다.(파일 자체의 업로드 완료)

![image](/assets/img/2025-03-21-File-Upload/Pasted-image-20240529123716.png)
```jsp
<form action="upload.jsp" method="post" name="frm2"

enctype="multipart/form-data">
```

파일첨부가 되는 form은 반드시 enctype이라는 속성이 있어야 한다.

enctype="multipart/form-data"

## 왜?****
음. enctype은 인코딩 타입이며, 영어 알파벳 이외의 문자들도 분명히 있을 이진파일을 POST로 보내기 위해서는 `multipart/form-data`를 이용해야 하는구나

multipart란 말 그대로, 하나, 또는 하나 이상의 **다른 타입의 데이터**들이 하나의 body에 결합된 형태 입니다.



# JSP Action Tags

각 JSP 작업 태그는 특정 작업을 수행하는 데 사용  
JSP 태그는 페이지 간 플로우를 제어하고 Java Bean을 사용하는 데 사용

| JSP             | Action Tags Description                         |
| --------------- | ----------------------------------------------- |
| jsp:forward     | 요청과 응답을 다른 리소스로 전달                              |
| jsp:include     | 다른 리소스를 포함                                      |
| jsp:useBean     | Bean 오브젝트를 작성 및 찾기                              |
| jsp:setProperty | Bean 객체의 속성 값을 설정                               |
| jsp:getProperty | Bean의 특성 값을 인쇄                                  |
| jsp:plugin      | 애플릿과 같은 다른 구성 요소를 포함                            |
| jsp:param       | 매개 변수 값을 설정, 대부분 forward, include에서 사용          |
| jsp:fallback    | 플러그인이 작동하는 경우 메시지를 인쇄하는 데 사용(jsp : plugin에서 사용) |

->jsp:useBean, jsp:setProperty and jsp:getProperty tags은 bean develop에만 씀

했던 거
- # include
	 include 액션요소는 포함된 jsp문서의 코드가 들어온 것이 아니라 
	컴파일된 결과(HTML)가 포함된다.
	jsp 안에 있던 변수나 함수 등 현재 페이지에서 사용할수 없다.
	


# forward
# usebean
# property
