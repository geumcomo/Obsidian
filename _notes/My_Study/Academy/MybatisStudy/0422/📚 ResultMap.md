**복잡한 결과 매핑을 간편하게** 만들어주기 위해 만들어진 태그다.
아래와 같은 User 객체가 있다고 가정해보자

```
@Getter @Setter
public class User {
  private int id;
  private String username;
  private String hashedPassword;
}
```

Mybatis의 resultType을 User 객체로 바꾸어주었다.

```
<select id="selectUsers" resultType="User객체경로.User">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

User객체의 필드명과 selectUsers의 조회 컬럼명이 일치하므로 쉽게 매핑된다.

하지만 쿼리문이 아래와 같이 달라진다면 이야기가 달라진다.

```
<select id="selectUsers" resultType="User객체경로.User">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

물론 AS 키워드를 이용해서 매핑할 수는 있다.

하지만 (그럴 일은 없겠지만) 필드명이 자주 바뀐다거나, 매칭되는 타입(number -> String)이 다른 경우에는 매핑할 수 없게 된다.

이 때 사용하게 되는 것이 ResultMap이다.

아래와 같은 ResultMap을 적용해본 코드를 보자.

```
<resultMap id="testMap" type="User객체경로.User">
  <result column="user_id" property="id" jdbcType="NVARCHAR" javaType="String"/>
  <result column="user_name" property="username" jdbcType="NVARCHAR" javaType="String"/>
  <result column="hashed_password" property="hashedPassword" jdbcType="NVARCHAR" javaType="String"/>
</resultMap>

<select id="selectUsers" resultMap="testMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

user_id는 resultMap을 통해 User 객체의 id에 매칭된다.

jdbcType이나 javaType도 지정이 가능하다.

