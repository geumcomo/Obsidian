```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

<environments default="">

<environment id="">

<transactionManager type="JDBC"/>

<dataSource type="POOLED"> <!-- 프리패스 미리 준비 주고 반납 -->

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

<mapper resource="am/emp.xml"/>

</mappers>

</configuration>
```