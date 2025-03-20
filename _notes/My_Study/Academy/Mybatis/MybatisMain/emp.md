```java
package am; // 패키지 선언: am 패키지에 속한다는 것을 선언합니다.

import java.io.IOException; // IOException 클래스를 임포트합니다.
import java.io.Reader; // Reader 클래스를 임포트합니다.
import java.util.List; // List 인터페이스를 임포트합니다.

import org.apache.ibatis.io.Resources; // Resources 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSession; // SqlSession 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSessionFactory; // SqlSessionFactory 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSessionFactoryBuilder; // SqlSessionFactoryBuilder 클래스를 임포트합니다.

public class Main_Ex1 { // Main_Ex1 클래스를 정의합니다.

	public static void main(String[] args) throws Exception { // 메인 메서드 시작
		// 환경설정 파일 읽기에 쓰인 스트림 준비
		Reader r = Resources.getResourceAsReader("am/config.xml"); // config.xml 파일을 읽어 Reader 객체 r에 저장합니다.
		
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); // SqlSessionFactoryBuilder 객체 생성
		
		SqlSessionFactory factory = builder.build(r); // SqlSessionFactoryBuilder를 사용하여 SqlSessionFactory 객체 factory를 생성합니다.
		r.close(); // Reader 객체 r을 닫습니다.
		
		SqlSession ss = factory.openSession(); // SqlSessionFactory 객체 factory를 사용하여 SqlSession 객체 ss를 생성합니다.
		
		List<EmpVO> list = ss.selectList("emp.all"); // SqlSession 객체 ss를 사용하여 "emp.all" 쿼리를 실행하고 그 결과를 List<EmpVO> 객체 list에 저장합니다.
		
		for(int i=0; i<list.size(); i++) { // 리스트의 크기만큼 반복문을 실행합니다.
			// 리스트에 저장된 vo객체를 하나씩 얻어낸다.
			EmpVO vo = list.get(i); // List<EmpVO> 객체 list에서 i번째 EmpVO 객체를 가져와 vo 변수에 저장합니다.
			
			System.out.println(vo.getEmpno()+"/"+ // EmpVO 객체 vo의 getEmpno 메서드를 호출하여 사원번호를 출력합니다.
					vo.getEname()+"/"+ // EmpVO 객체 vo의 getEname 메서드를 호출하여 사원 이름을 출력합니다.
					vo.getJob()); // EmpVO 객체 vo의 getJob 메서드를 호출하여 직업을 출력합니다.
		} // for 반복문 끝
		System.out.println("---------------------"); // 구분선을 출력합니다.
		System.out.println(list.size()+"명"); // 리스트의 크기(사원 수)를 출력합니다.
		ss.close(); // SqlSession 객체 ss를 닫습니다.
	} // 메인 메서드 끝
} // 클래스 끝

```