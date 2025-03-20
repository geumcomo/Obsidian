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

기본적인 문법에 대해서 알아보았다면 이제 실제로 사용한 예시를 아래 그림으로 확인해보자.

![](https://velog.velcdn.com/images/ung6860/post/39712e60-d760-4597-acf4-3b4e076af736/image.png)

위 코드에서 사용한 DB 테이블의 구조는 아래와 같다.

|news_idx|title|writer|content|regdate|hit|
|---|---|---|---|---|---|
|PK|제목|작성자|내용|등록일|조회수|

또한 위 테이블과 대응하는 DTO의 코드는 아래를 참고하자.

```java
public class News {
	private int news_idx;
	private String title;
	private String writer;
	private String content;
	private String regdate;
	private int hit;
}
```

테이블과 DTO 코드를 같이보면 컬럼에 `1:1`로 대응하는 멤버변수들을 보유하고있다. 이것으로 기본문법에 대입해보면 어떤 위치에 어떠한 값이 입력되어야하는지 감이 올 것이다. 그렇다면 아래와 같이 컬럼명과 동일하지 않은 멤버변수와 매핑을 해야하는 경우를 아래에서 살펴보자. 테이블의 컬럼명은 위와 동일하다고 보고, DTO의 멤버변수 중 하나가 다르다고 가정하겠다.

```java
public class News {
	private int news_idx;
	private String title;
	private String author; //작성자를 writer -> author로 변경
	private String content;
	private String regdate;
	private int hit;
}
```

이 부분에 대해서 아래 xml 태그를 확인해보자. 해당하는 컬럼에 대해서만 작성을 해보겠다.

```xml
 <result column="writer" property="author"/>
```

위에서 처럼 column에는 `테이블의 컬럼명`, property에는 `DTO의 멤버변수명`을 작성해주면 된다. 즉, 테이블의 컬럼명과 DTO의 멤버변수명을 1:1로 대응하게 작성해주면 Mybatis가 내부적으로 매핑을 해준다는 말이다.