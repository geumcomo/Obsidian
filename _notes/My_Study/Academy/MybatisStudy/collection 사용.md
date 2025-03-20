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

다른 것들은 크게 어려운 부분이 없을 것이라 생각된다. 다만, property와 select는 조금 헷갈릴 수도 있을 것이다.

먼저 property는 DB 테이블과 1:1대응하는 DTO의 멤버변수가 아니다. 이 말은 property에 들어가야하는 멤버변수는 서브쿼리의 `결과를 받을 특정 멤버변수`가 되는 것이다. 그게 기존의 것이 될 수도 있고, 새로운 것이 될 수도 있다는 말이다.

다음으로 select는 조인할 때 실행되어야하는 서브쿼리의 대상이 되는 쿼리문을 이야기한다. 자세한 것은 아래 예제를 보면서 추가적으로 설명하겠다.

![](https://velog.velcdn.com/images/ung6860/post/39712e60-d760-4597-acf4-3b4e076af736/image.png)

위 그림을 보면 ResultMap을 통해 매핑을 완료해주었다. 그 다음은 ResultMap 태그 안에 collection을 작성한다. 다음 그림을 보자.

![](https://velog.velcdn.com/images/ung6860/post/2da3bf6a-d0ca-461e-8a78-ba2cbf92c425/image.png)

그림을 보면 위에서 설명한 기본적인 문법이 나열되어있는 것이 보일 것이다. 아래의 테이블의 참조관계를 보면서 그림을 본다면 무슨 내용인지 이해가 될 것이다.

부모 테이블

|news_idx|title|writer|content|regdate|hit|
|---|---|---|---|---|---|
|PK|제목|작성자|내용|등록일|조회수|

자식 테이블

|comments_idx|news_idx|msg|author|writedate|
|---|---|---|---|---|
|PK|FK|내용|작성자|등록일|

`news_idx`가 두 테이블을 연결하는 컬럼이다. 그렇기 때문에 collection의 column으로 사용이 되는 것이다. 그럼 다음은 DTO를 한번 들여다보자.

```java
public class News {
	//컬럼과 1:1로 대응하는 멤버변수
	private int news_idx;
	private String title;
	private String writer;
	private String content;
	private String regdate;
	private int hit;
    
    //조인(서브쿼리) 결과를 받을 멤버변수
	private List<Comments> commentsList;
}
```

보이는 것처럼 컬럼명과 동일하지 않고 아예 동떨어진 녀석이 보인다. commentsList는 조인의 결과를 담을 변수이기 때문에 컬럼과 1:1 대응하지 않는다. 그럼 자세히 풀어서 설명을 해보겠다.

- javaType : commentsList를 java.util.List 자료형으로 선언
- ofType : commentsList의 제너릭이 Comments형이기 때문에 Comments로 지정
- property : 서브쿼리의 결과가 담겨질 멤버변수명
- select : 서브쿼리는 Comments의 MapperFile에서 실행하기 때문에 해당 쿼리문의 id를 지정

그렇다면 서브쿼리문은 어떻게 되어있는지 보자.

![](https://velog.velcdn.com/images/ung6860/post/1538e1c8-17dd-4a1d-9710-2996805986d6/image.png)

이렇게 서로 다른 MapperFile에 작성된 쿼리문을 호출하는 것이 collection의 select 태그이다. 그렇다면 마지막으로 최종적으로 적용된 상태를 보면서 마무리하자.

![](https://velog.velcdn.com/images/ung6860/post/f1424b57-4ebe-4210-827b-e18daa216dc4/image.png)

조금은 복잡할 수 있다. 다만 각 태그들이 DB, DTO 그리고 서브쿼리와 어떻게 매핑이 되는지 이해하기 위해서는 한번씩 둘러보길 권장한다.