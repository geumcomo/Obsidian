```java
package pm.client;

import java.io.IOException;
import java.io.Reader;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import pm.vo.DeptVO;
import pm.vo.EmpVO;
import pm.vo.LocVO;

public class Main_Ex1 {

	public static void main(String[] args) throws Exception {
		Reader r = Resources.getResourceAsReader(
				"pm/config/config.xml");
		SqlSessionFactory factory = 
			new SqlSessionFactoryBuilder().build(r);
		r.close();
		//-------------------------------------------
		
		SqlSession ss = factory.openSession();
		
		List<DeptVO> list = ss.selectList("dept.all");
		for(int i=0; i<list.size(); i++) {
			DeptVO dvo = list.get(i);//부서정보 얻기
			
			//도시정보 얻기
			LocVO lvo = dvo.getLvo();
			
			//해당 부서정보 안에 있는 사원들 얻기
			List<EmpVO> e_list = dvo.getE_list();
			
			System.out.println(dvo.getDeptno()+" - "+
					dvo.getDname()+","+
					lvo.getCity()+"("+e_list.size()+
					"명)");
			
			// 구성원들 출력하는 반복문
			for(int j=0; j<e_list.size(); j++) {
				System.out.println("\t -"+
						e_list.get(j).getEmpno()+"/"+
						e_list.get(j).getEname());
			}//안쪽 for의 끝
		}
		

	}

}


```