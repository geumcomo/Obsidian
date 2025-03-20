```java
package pm; // 패키지 선언: pm 패키지에 속한다는 것을 선언합니다.

import java.io.IOException; // IOException 클래스를 임포트합니다.
import java.io.Reader; // Reader 클래스를 임포트합니다.
import java.util.List; // List 인터페이스를 임포트합니다.
import java.util.Scanner; // Scanner 클래스를 임포트합니다.

import org.apache.ibatis.io.Resources; // Resources 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSession; // SqlSession 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSessionFactory; // SqlSessionFactory 클래스를 임포트합니다.
import org.apache.ibatis.session.SqlSessionFactoryBuilder; // SqlSessionFactoryBuilder 클래스를 임포트합니다.

public class Main_Ex1 { // Main_Ex1 클래스를 정의합니다.

	public static void main(String[] args) throws Exception { // 메인 메서드 시작
		// 환경설정 파일(config.xml)과 연결되는 스트림 준비
		Reader r = Resources.getResourceAsReader("pm/config.xml"); // config.xml 파일을 읽어 Reader 객체 r에 저장합니다.
		
		// 우리가 필요한 객체는 SqlSession이다. 이것을 얻어내기 위해
		// SqlSession을 만드는 공장을 세워야 하고 그러기 위해서는
		// 앞서 공장을 세우는 일꾼(builder)을 만들어야 한다.
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); // SqlSessionFactoryBuilder 객체 builder 생성
		
		// 만들어진 일꾼을 가지고 공장을 세우자!
		SqlSessionFactory factory = builder.build(r); // SqlSessionFactoryBuilder를 사용하여 SqlSessionFactory 객체 factory를 생성합니다.
		r.close(); // Reader 객체 r을 닫습니다.
		// ----- 한번만 수행하는 것이 유익하다고 생각됨 -----
		
		// 위에서 만든 공장(factory)를 통해 SqlSession을 얻는다.
		SqlSession ss = factory.openSession(); // SqlSessionFactory 객체 factory를 사용하여 SqlSession 객체 ss를 생성합니다.
		
		// 위의 SqlSession이 있어야 그것을 통해 SQL문장을 호출한다.
		List<DeptVO> list = ss.selectList("dept.all"); // SqlSession 객체 ss를 사용하여 "dept.all" 쿼리를 실행하고 그 결과를 List<DeptVO> 객체 list에 저장합니다.
		
		System.out.println(list.size()+"개 부서"); // 리스트의 크기(부서 수)를 출력합니다.
		
		System.out.println("------ 부서명 검색 -------------"); // 부서명 검색을 위한 메시지를 출력합니다.
		Scanner scan = new Scanner(System.in); // Scanner 객체 scan 생성
		System.out.println("부서명:"); // 사용자에게 부서명을 입력하라는 메시지를 출력합니다.
		String dname = scan.nextLine(); // 사용자로부터 부서명을 입력받아 dname 변수에 저장합니다.
		
		List<DeptVO> list2 = ss.selectList("dept.search_name", dname); // SqlSession 객체 ss를 사용하여 "dept.search_name" 쿼리를 실행하고 그 결과를 List<DeptVO> 객체 list2에 저장합니다.
		for(int i=0;i<list2.size(); i++) { // 리스트의 크기만큼 반복문을 실행합니다.
			DeptVO vo = list2.get(i); // List<DeptVO> 객체 list2에서 i번째 DeptVO 객체를 가져와 vo 변수에 저장합니다.
			System.out.println(vo.getDeptno()+"/"+ // DeptVO 객체 vo의 getDeptno 메서드를 호출하여 부서번호를 출력합니다.
					vo.getDname()+"/"+ // DeptVO 객체 vo의 getDname 메서드를 호출하여 부서명을 출력합니다.
					vo.getLoc_code()); // DeptVO 객체 vo의 getLoc_code 메서드를 호출하여 위치 코드를 출력합니다.
		}
		
		ss.close(); // SqlSession 객체 ss를 닫습니다.
	}

}

```