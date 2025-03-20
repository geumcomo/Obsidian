
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="emp">

<!-- 사원추가 기능 -->

<insert id="add" parameterType="am.EmpVO">

insert into emp(empno,ename,job,sal,hiredate,deptno)

values(#{empno},#{ename},#{job},#{sal},#{hiredate},#{deptno})

</insert>

</mapper>
```