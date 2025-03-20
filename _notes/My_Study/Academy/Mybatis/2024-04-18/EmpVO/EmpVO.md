```java
package am;

public class EmpVO {

//사원을 의미하는 클래스다.

//emp테이블에서 필요한 칼럼들을 멤버변수로선언한다 쓰기도하고 저장하기 위해서

private String empno, ename, job, sal, hiredate, deptno;

public EmpVO() {}//기본생성자

public EmpVO(String empno, String ename, String job, String sal, String hiredate, String deptno) {

super();

this.empno = empno;

this.ename = ename;

this.job = job;

this.sal = sal;

this.hiredate = hiredate;

this.deptno = deptno;

}

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

public String getJob() {

return job;

}

public void setJob(String job) {

this.job = job;

}

public String getSal() {

return sal;

}

public void setSal(String sal) {

this.sal = sal;

}

public String getHiredate() {

return hiredate;

}

public void setHiredate(String hiredate) {

this.hiredate = hiredate;

}

public String getDeptno() {

return deptno;

}

public void setDeptno(String deptno) {

this.deptno = deptno;

}

}
```