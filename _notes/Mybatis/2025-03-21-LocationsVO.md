---
---

```java
package test;

public class LocationsVO {

private String loc_code, city;

public String getLoc_code() {

return loc_code;

}

public void setLoc_code(String loc_code) {

this.loc_code = loc_code;

}

public String getCity() {

return city;

}

public void setCity(String city) {

this.city = city;

}

}
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="locations">

<select id="all" resultType="test.LocationsVO">

select * from locations

</select>

<select id="search_name" resultType="test.LocationsVO"

parameterType="String">

select *from locations

where city LIKE CONCAT('%',#{str},'%')

</select>

</mapper>
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

<environments default="">

<environment id="">

<transactionManager type="JDBC"/>

<dataSource type="POOLED">

<property name="driver"

value="com.mysql.cj.jdbc.Driver"/>

<property name="url"

value="jdbc:mysql://localhost:3306/my_db"/>

<property name="username" value="test"/>

<property name="password" value="1111"/>

</dataSource>

</environment>

</environments>

<mappers>

<mapper resource="test/locations.xml"/>

</mappers>

</configuration>

```java 
package test;

import java.io.IOException;
import java.io.Reader;
import java.util.List;
import java.util.Scanner;

import javax.tools.DocumentationTool.Location;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class Main {

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		Reader r = Resources.getResourceAsReader("test/config.xml");
		
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
		
		SqlSessionFactory factory = builder.build(r);
		r.close();
		
		SqlSession ss = factory.openSession();
		List<LocationsVO> list = ss.selectList("locations.all");
		for(int i=0; i<list.size(); i++) {
			LocationsVO vo = list.get(i);
			System.out.println(vo.getLoc_code()+"/"+vo.getCity());
			
		}
		System.out.println("------------------");
		System.out.println(list.size()+ "개의 도시");
		Scanner scan = new Scanner(System.in);
		System.out.println("도시명 입력");
		String city = scan.nextLine();
		List<LocationsVO> list2 = ss.selectList("locations.search_name", city);
		for(int i=0; i<list2.size(); i++) {
			LocationsVO vo = list2.get(i);
			System.out.println(vo.getLoc_code()+"/"+vo.getCity());
		}
				
		ss.close();
	}

}

```