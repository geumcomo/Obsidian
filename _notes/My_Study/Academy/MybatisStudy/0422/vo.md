dept
```java
package pm.vo;

import java.util.List;

public class DeptVO {

private String deptno, dname;

//현 부서에 종사는 사원들...

private List<EmpVO> e_list;

//도시정보

private LocVO lvo;

public LocVO getLvo() {

return lvo;

}

public void setLvo(LocVO lvo) {

this.lvo = lvo;

}

public String getDeptno() {

return deptno;

}

public void setDeptno(String deptno) {

this.deptno = deptno;

}

public String getDname() {

return dname;

}

public void setDname(String dname) {

this.dname = dname;

}

public List<EmpVO> getE_list() {

return e_list;

}

public void setE_list(List<EmpVO> e_list) {

this.e_list = e_list;

}

}
```
emp
```java
package pm.vo;

public class EmpVO {

private String empno, ename;

public String getEmpno() {

return empno;

}

public void setEmpno(String empno) {

this.empno = empno;

}

public String getEname() {

return ename;

}

public void setEname(String ename) {

this.ename = ename;

}

}
```
loc
```java
package pm.vo;

public class LocVO {

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