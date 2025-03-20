```java
package am; // 'am'이라는 패키지를 정의합니다.

import java.io.IOException; // Java의 입출력 예외를 처리하기 위한 클래스를 임포트합니다.
import java.io.Reader; // Reader 클래스를 임포트합니다. (파일이나 문자열을 읽는 데 사용)
import java.util.HashMap; // HashMap 클래스를 임포트합니다. (키-값 쌍의 데이터를 저장하는 데 사용)
import java.util.Map; // Map 인터페이스를 임포트합니다. (키-값 쌍의 데이터를 처리하는 데 사용)

import org.apache.ibatis.io.Resources; // MyBatis의 리소스 관련 유틸리티를 임포트합니다.
import org.apache.ibatis.session.SqlSession; // MyBatis의 SqlSession 클래스를 임포트합니다. (SQL 실행)
import org.apache.ibatis.session.SqlSessionFactory; // MyBatis의 SqlSessionFactory 클래스를 임포트합니다. (SqlSession을 생성)
import org.apache.ibatis.session.SqlSessionFactoryBuilder; // MyBatis의 SqlSessionFactoryBuilder 클래스를 임포트합니다. (SqlSessionFactory를 생성하는 빌더)

public class Main_Ex1 { // Main_Ex1 클래스를 정의합니다.
	
	SqlSessionFactory factory; // SqlSessionFactory 객체를 선언합니다.
	
	public Main_Ex1() { // Main_Ex1 클래스의 생성자입니다.
		// 다른 일들....
		makeFactory(); // makeFactory 메서드를 호출하여 SqlSessionFactory를 생성합니다.
	}
	
	private void makeFactory() { // SqlSessionFactory를 생성하는 메서드입니다.
		try {
			// 1) 환경설정파일과 연결되는 스트림 준비
			Reader r = Resources.getResourceAsReader(
					"am/config.xml"); // "am/config.xml" 파일을 읽는 Reader 객체를 생성합니다.
			
			// 2) FactoryBuilder를 생성하면서 바로 Factory 생성
			factory = 
				new SqlSessionFactoryBuilder().build(r); // SqlSessionFactoryBuilder를 사용하여 factory를 생성합니다.
			r.close(); // Reader를 닫습니다.
		} catch (Exception e) { // 예외 처리
			e.printStackTrace(); // 예외 내용을 출력합니다.
		}
	}
	
	// 저장 기능
	public void add(String empno, String ename, 
			String sal, String job, 
			String hiredate, String deptno) {
		//mybatis환경에 인자로 전달할 객체 생성
		EmpVO vo = new EmpVO(empno, ename, 
				job, sal, hiredate, deptno); // EmpVO 객체를 생성하고 인자로 받은 데이터로 초기화합니다.
		
		//위의 vo객체를 인자로 보내면서 저장한다.
		SqlSession ss = factory.openSession(); // SqlSession을 생성합니다.
		int cnt = ss.insert("emp.add", vo); // "emp.add" 쿼리를 실행하여 데이터를 추가합니다.
		
		//cnt에는 저장을 수행한 레코드 수가 저장된다.
		if(cnt > 0) { // 저장 성공 여부를 확인합니다.
			System.out.println("저장완료!"); // 저장 완료 메시지를 출력합니다.
			ss.commit(); // 커밋합니다.
		}else {
			System.out.println("저장실패!"); // 저장 실패 메시지를 출력합니다.
			ss.rollback(); // 롤백합니다.
		}
		
		if(ss != null)
			ss.close(); // SqlSession을 닫습니다.
	}
	
	public void login(String id, String pw) { // 로그인 기능을 처리하는 메서드입니다.
		// 로그인 SQL문장을 호출하기 위해 SqlSession을 얻어낸다.
		SqlSession ss = factory.openSession(); // SqlSession을 생성합니다.
		
		// 로그인을 하기 위해 필요한 객체 준비
		MemberVO mvo = new MemberVO(); // MemberVO 객체를 생성합니다.
		mvo.setM_id(id); // 아이디를 설정합니다.
		mvo.setM_pw(pw); // 비밀번호를 설정합니다.
		
		// 로그인은 결과객체가 1개다.
		MemberVO vo = ss.selectOne(
				"member.login", mvo); // "member.login" 쿼리를 실행하여 결과를 받습니다.
		
		if(vo != null) // 결과가 있으면
			System.out.println("로그인 성공"); // 로그인 성공 메시지를 출력합니다.
		else
			System.out.println("아이디 또는 비밀번호가 다릅니다."); // 로그인 실패 메시지를 출력합니다.
		
		if(ss != null)
			ss.close(); // SqlSession을 닫습니다.
	}
	
	public void login2(String id, String pw) { // 두 번째 로그인 기능을 처리하는 메서드입니다.
		SqlSession ss = factory.openSession(); // SqlSession을 생성합니다.
		
		// member.login2를 호출하기 위해 필요한 객체 준비
		Map<String, String> map = // Map 객체를 생성합니다.
				new HashMap<>();
		map.put("id", id); // 아이디를 설정합니다.
		map.put("pw", pw); // 비밀번호를 설정합니다.
		
		MemberVO vo = ss.selectOne(
				"member.login2", map); // "member.login2" 쿼리를 실행하여 결과를 받습니다.
		
		if(vo != null) // 결과가 있으면
			System.out.println("로그인 성공"); // 로그인 성공 메시지를 출력합니다.
		else
			System.out.println("아이디 또는 비밀번호가 다릅니다."); // 로그인 실패 메시지를 출력합니다.
		
		if(ss != null)
			ss.close(); // SqlSession을 닫습니다.
	}

	public static void main(String[] args) throws Exception {
		//현재 객체 생성
		Main_Ex1 ex1 = new Main_Ex1(); // Main_Ex1 객체를 생성합니다.
		
		//저장을 하기 위해서 Main_Ex1안에 있는 add를 호출한다.
		//ex1.add("1300","마동탁","900","DEVLOP",
		//		"2024-04-18","10");
		
		// 로그인
		ex1.login("test1", "1212");
		ex1.login2("test1", "1212");

```