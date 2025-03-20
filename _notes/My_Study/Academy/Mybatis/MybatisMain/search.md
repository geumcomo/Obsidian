```java
package pm;

import java.io.Reader;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class Main_Ex1 {

    SqlSessionFactory factory;
    
    // 생성자: factory 생성
    public Main_Ex1() {
        makeFactory();
    }
    
    // SqlSessionFactory 생성
    public void makeFactory() {
        try {
            // config.xml 파일을 읽어와 Reader 객체 생성
            Reader r = Resources.getResourceAsReader("pm/config.xml");
            
            // SqlSessionFactoryBuilder를 사용하여 factory 생성
            factory = new SqlSessionFactoryBuilder().build(r);
            
            r.close(); // Reader 닫기
        } catch (Exception e) {
            e.printStackTrace(); // 예외 처리
        }
    }
    
    // 이름으로 검색하는 메서드
    public void searchName(String name) {
        // SqlSession을 얻어옴
        SqlSession ss = factory.openSession();
        
        // "mem.search_name" 쿼리 실행 후 결과를 리스트에 저장
        List<MemVO> list = ss.selectList("mem.search_name", name);
        
        // 리스트 출력
        printList(list);
        
        if(ss != null)
            ss.close(); // SqlSession 닫기
    }
    
    // 이메일과 연락처로 검색하는 메서드
    public void search(String email, String phone) {
        SqlSession ss = factory.openSession();
        
        // 검색 조건을 담을 Map 생성
        Map<String, String> map = new HashMap<>();
        
        // email이 null이 아니면 map에 추가
        if(email != null)
            map.put("key1", email);
        
        // phone이 null이 아니면 map에 추가
        if(phone != null)
            map.put("key2", phone);
        
        // email과 phone 모두 null이면 메서드 종료
        if(email == null && phone == null)
            return;
        
        // "mem.search" 쿼리 실행 후 결과를 리스트에 저장
        List<MemVO> list = ss.selectList("mem.search", map);
        
        // 리스트 출력
        printList(list);        
        
        if(ss != null)
            ss.close(); // SqlSession 닫기
    }
    
    // 리스트 출력
    private void printList(List<MemVO> list) {
        for(int i=0; i<list.size(); i++) {
            MemVO mvo = list.get(i); // 리스트에서 MemVO 객체 하나를 가져옴
            
            // MemVO 객체의 정보 출력
            System.out.println(mvo.getM_idx() + "/" +
                               mvo.getM_id() + "/" +
                               mvo.getM_name() + "/" +
                               mvo.getM_phone() + "/" +
                               mvo.getM_email());
        }//for의 끝
    }
    
    public static void main(String[] args) {
        // Main_Ex1 객체 생성
        Main_Ex1 ex1 = new Main_Ex1();
        
        // 이름으로 검색
        ex1.searchName("김");
        System.out.println("---- searchName끝 -----");
        
        // 이메일과 연락처로 검색
        ex1.search("korea", "010");
        System.out.println("---- search끝 -----");
        
        // 이메일로 검색
        ex1.search("korea", null);
        System.out.println("---- search끝 -----");
        
        // 연락처로 검색
        ex1.search(null, "010");
    }

}

```