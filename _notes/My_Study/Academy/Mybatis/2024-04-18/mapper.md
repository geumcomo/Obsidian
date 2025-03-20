
```xml
<select id = "search" parameterType = "String" resultType = "am.EmpVO" - 수행하고 결과
select * from emp 
where ename LIKE concat('%', #{str},'%') 
							-파라미터 타입이 있어야 된다.
```
```xml
<insert id= 'add' parameterType= "am.EmpVO">
		i   nsert into emp(empno,enmae)
		value(#{empno},#{ename})
</insert>
```
</select>
EmpVO obj = ss.selectOne("emp.search","100"); - 하나만
List<EmpVO> obj = ss.selectList("emp.search","100"); -   리스트
"100" 자료형 맞춰줘야한다. string
							ㄴ (namespace.id(mapper 경로) , 100 인자) 파라미터 값으로 변수로
              



