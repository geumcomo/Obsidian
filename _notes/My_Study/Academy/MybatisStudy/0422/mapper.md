dept
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="dept">

<resultMap type="pm.vo.DeptVO" id="m1">

<!--

<result property="deptno" column="deptno"/>

-->

<!-- 기본키를 의미 -->

<id property="deptno" column="deptno"/>

<!-- 1:1관계 -->

<association property="lvo"

javaType="pm.vo.LocVO"

select="loc.get_loc" column="loc_code"/>

<!-- 1대 다관계를 형성하기 위해 다음을 준비한다. -->

<collection property="e_list"

javaType="java.util.List"

ofType="pm.vo.EmpVO"

select="emp.emp_list" column="deptno"/>

</resultMap>

<!-- 모든 부서들을 반환한다. -->

<select id="all" resultMap="m1">

select * from dept

</select>

</mapper>
```
emp
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="emp">

<!-- 부서코드를 인자로 받아서 해당부서에 있는 사원정보들 반환 -->

<select id="emp_list" resultType="pm.vo.EmpVO"

parameterType="String">

select * from emp

where deptno = #{no}

</select>

</mapper>
```
loc
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="loc">

<select id="get_loc" resultType="pm.vo.LocVO"

parameterType="String">

SELECT * FROM locations

WHERE loc_code = #{no}

</select>

</mapper>
```
