BookVO.java
```java
package vo;

public class BookVO {

private String b_idx, subject, author, reg_date, price;

private PressVO pvo; //1대1 관계

public PressVO getPvo() {

return pvo;

}

public void setPvo(PressVO pvo) {

this.pvo = pvo;

}

public String getB_idx() {

return b_idx;

}

public void setB_idx(String b_idx) {

this.b_idx = b_idx;

}

public String getSubject() {

return subject;

}

public void setSubject(String subject) {

this.subject = subject;

}

public String getAuthor() {

return author;

}

public void setAuthor(String author) {

this.author = author;

}

public String getReg_date() {

return reg_date;

}

public void setReg_date(String reg_date) {

this.reg_date = reg_date;

}

public String getPrice() {

return price;

}

public void setPrice(String price) {

this.price = price;

}

}
```
PressVO
```java
package vo;

public class PressVO {

private String p_idx, p_name, loc_code;

public String getLoc_code() {

return loc_code;

}

public void setLoc_code(String loc_code) {

this.loc_code = loc_code;

}

public String getP_idx() {

return p_idx;

}

public void setP_idx(String p_idx) {

this.p_idx = p_idx;

}

public String getP_name() {

return p_name;

}

public void setP_name(String p_name) {

this.p_name = p_name;

}

}
```



book.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="book">

<resultMap type="vo.BookVO" id="mm">

<!-- 1대 1관계를 의미하는 구현을 해야 하며 현재객체(BookVO)의 멤버변수인 pvo에 pressVO를 저장 -->

<association property="pvo" javaType= "vo.PressVO"

select="press.getPress" column = "p_idx"/> <!-- press -->

</resultMap>

<!-- 도서 전체 반환 기능

select * from book_t 이 sql 문을 수행하고 발생되는 결과

발생되는 결과를 레코드 단위로 아이디가 mm인 resultmap에 전달

하여 결과를 만든다.

-->

<select id="total" resultMap="mm">

select * from book_t

</select>

</mapper>
```
press.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="press">

<!-- 인자로 p_idx의 값을 받아서 출판사 정보를 PressVO로 반환하는 기능 -->

<select id="getPress" resultType="vo.PressVO" parameterType="String">

select * from press_t

where p_idx = #{no}

</select> <!-- rdbms -->

</mapper>
```
Main.java
```java
package client;

import java.io.IOException;
import java.io.Reader;

import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import vo.BookVO;
import vo.PressVO;

public class Main {

	public static void main(String[] args) throws Exception {
		
		Reader r = Resources.getResourceAsReader("config/config.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(r);
		r.close();
		
		SqlSession ss = factory.openSession();
		
		
		List<BookVO> list = ss.selectList("book.total");
		
		for(int i=0; i<list.size(); i++) {
			BookVO bvo = list.get(i);
			PressVO pvo = bvo.getPvo(); 
			System.out.println(bvo.getSubject()+"/"+
					bvo.getAuthor()+"/"+
					bvo.getPrice()+"/"+
					pvo.getP_name()+"/"+
					pvo.getLoc_code());
			
		}
		if(ss != null)
			ss.close();
	}

}

```
