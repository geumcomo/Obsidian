```java
package am;

import java.io.IOException;

import java.io.Reader;

import org.apache.ibatis.io.Resources;

import org.apache.ibatis.session.SqlSession;

import org.apache.ibatis.session.SqlSessionFactory;

import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class Main_Ex1 {

SqlSessionFactory factory;

public Main_Ex1() {

//다른 일들....

makeFactory();//DB 를 활용할 수있는 factory생성

}

private void makeFactory() {

try {

// 1)환경설정파일과 연결되는 스트림 준비

Reader r = Resources.getResourceAsReader(

"am/config.xml");

//2)factorybuilder를 생성하면서 바로 factory생성

factory = new SqlSessionFactoryBuilder().build(r);

r.close();

} catch (Exception e) {

e.printStackTrace();

}

}

// 저장 기능

public void add(String empno,String ename, String sal, String job, String hiredate,String deptno) {

//myvatis환경에 인자로 전달할 객체 생성

EmpVO vo = new EmpVO(empno, ename, job, sal, hiredate, deptno);

//위의 vo객체를 인자로 보내면서 저장한다.

SqlSession ss = factory.openSession();

int cnt = ss.insert("emp.add", vo);

//cnt에는 저장을 수행한 레코드 수가 저장된다.

if(cnt>0) {

System.out.println("저장완료");

ss.commit();

}else {

System.out.println("저장실패!");

ss.rollback();

}

if(ss != null)

ss.close();

}

public static void main(String[] args) throws Exception {

//현재 객체 생서

Main_Ex1 ex1 = new Main_Ex1();

//저장을 하기 위해서 Main_Ex1안에 있는 add를 호출하면 된다.

ex1.add("1200", "을블", "900", "DEVLOP", "2024-04-18", "10");

}

}
```
