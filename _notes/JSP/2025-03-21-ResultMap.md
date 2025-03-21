---
---

## ResultMap이란?

## ResultMap 사용

일반적인 쿼리문 사용은 지난 글에서 설치 및 사용을 하면서 알아보았다. 이번에는 조금 더 복잡한 조인을 실행할 때의 `ResultMap` 사용에 대해서 알아보겠다.  
기본문법은 다음과 같다.

```xml
<!-- 기본문법 -->
<resultMap type="대상DTO" id="아이디명">
  <!-- 1:1로 DTO의 멤버변수를 매핑-->
  <id column="대상 컬럼명" property="DTO멤버변수명"/>
  <result column="대상 컬럼명" property="DTO멤버변수명"/>
</resultMap>
```

- type : java의 `객체자료형(DTO)`의 이름을 작성
- id : 본인이 사용할 `직관적인 이름` 부여
- id 태그의 column : 테이블 간의 `참조키`로 활용되는 컬럼명
- id 태그의 property : 컬럼에 해당하는 DTO의 `멤버변수`명
- result 태그의 column : 일반 컬럼명
- result 태그의 property : 컬럼에 해당하는 DTO의 `멤버변수`명

위의 설명한 기본문법을 참고하여 ResultMap을 세팅했다면 아래와 같이 실행할 쿼리문에 ResultMap을 지정해준다.

```xml
<select id="select" parameterType="자료형" resultMap="아이디명">
  select * from 테이블명 where 대상컬럼=#{property명}
</select>
```
행번호 를  1~130번까지 출력을 해야한다. 나눠서 페이지를 구성해야한다.


```sql
select * from (select @rn:=@rn+1 as rnum, a.* FROM(
	select * from bbs_t
	where status = 0 
	order by b_idx desc
	) a, (select @rn:=0) b
    )c where c.rnum between 1 and 10 ;
```
where절이 먼저 수행 ()

## ResultMap의 Collection

### collection이란?

> 테이블 간의 1:n의 관계를 가질 때 사용하는 태그

collection은 `서브쿼리 형태`로 데이터를 가져올 수 있다. 이 말은 내부적으로 서브쿼리문을 실행해주기 때문에 각각의 쿼리문을 작성하여 조합하면 된다는 것이다.

### collection 사용

그렇다면 본격적으로 `collection`을 사용하여 조인하는 방법을 알아보자.  
먼저 아래를 보면 collection 사용에 기본적인 문법을 태그로 작성해두었다. 차근차근 알아보자.

```xml
<collection
            column="참조컬럼명"
            javaType="패키지.클래스명"
            ofType="DTO명"
            property="적용할 멤버변수명"
            select="조인할매퍼의 쿼리문"
/>
```

- column : collection의 컬럼은 테이블간의 `참조`가되는 컬럼을 지정
- javaType : 말 그대로 자바의 `자료형`, `패키지 경로`까지 입력
- ofType : `반환`받을 자료형을 의미함
- property : 서브쿼리 `결과를 저장`할 멤버변수명
- select : 서브쿼리를 `실행하는 매퍼`의 쿼리문
![image](/assets/img/2025-03-21-ResultMap/Pasted-image-20240604163842.png)
