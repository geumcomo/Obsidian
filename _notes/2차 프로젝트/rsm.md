
1:1
```xml
<select id="selectUsers" resultType="map">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
유저 아이디에 해당하는 id, name, password 를 선택한다.

하지만 (그럴 일은 없겠지만) 필드명이 자주 바뀐다거나, 매칭되는 타입(number -> String)이 다른 경우에는 매핑할 수 없게 된다.

이 때 사용하게 되는 것이 ResultMap이다.

<select id="selectUsers" resultType="User객체경로.User">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>


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

user_id는 resultMap을 통해 User 객체의 id에 매칭된다.

jdbcType이나 javaType도 지정이 가능하다.
```

```java
public class User {
  private int id;
  private String username;
  private String hashedPassword;
}

```
1:M

```xml
<resultMap id="productBoard" type="BoardVO">
    <!-- pd_idx 컬럼을 BoardVO 객체의 pd_idx 프로퍼티에 매핑합니다. -->
    <id property="pd_idx" column="pd_idx" />
    <!-- boards_t 테이블에서 조회된 데이터를 BoardVO 타입의 boards 프로퍼티에 매핑합니다.
         이때 boards.getBoard 쿼리를 사용하여 데이터를 가져옵니다. -->
   <collection property="boards" ofType="BoardVO" select="boards.getBoard"/>
</resultMap>

<!-- getProduct_Board 쿼리는 boards_t 테이블에서 데이터를 조회합니다.
     조회된 데이터는 productBoard resultMap을 사용하여 매핑됩니다. -->
<select id="getProduct_Board" resultMap="productBoard">
	SELECT * FROM boards_t
</select>
```

