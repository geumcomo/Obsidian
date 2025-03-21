---
---


```jsp
<tr>
					<td colspan="2">
						<input type="button" value="수정" 
						onclick="javascript:location.href='Controller?type=edit&b_idx=<%=vo.getB_idx() %>&bname=${param.bname}&cPage=${param.cPage}	'"/>
						<input type="button" value="삭제" onclick="delBbs()"/>
						<input type="button" value="목록"
						onclick="javascript:location.href='Controller?type=list&bname=${param.bname}&cPage=${param.cPage}'"/>
					</td>
</tr>
```


JSP 표현 언어(JSP Expression Language, EL)를 사용하여 JSP 페이지에서 전달된 요청 매개변수의 값을 참조하는 코드 `param` 객체의 `name`이라는 이름의 매개변수를 참조합니다. JSP EL은 `${...}` 형식을 사용하여 표현됩니다.\


`param` 객체는 JSP 페이지에서 클라이언트가 요청한 URL의 쿼리 문자열 또는 폼 데이터를 추출하고, 이를 쉽게 접근하고 처리할 수 있도록 도와주는 객체