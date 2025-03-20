
생성하지않고 클래스 이름을 통해  사용가능
 
 math.random() 
 Color.RED
  
 system.out
 system.in




```java
public class StaticTest {
	
	int value = 10;
	
	static String msg = getMsg();
	public StaticTest() {
		System.out.println("생성자");
	}
	public static String getMsg() {
		return "힘내라~";
	}
}
//생성되기 전에 static 먼저움직임

public static void main(String[] args) {
		// TODO Auto-generated method stub
		StaticTest t1 = new StaticTest();
		System.out.println(StaticTest.msg); //힘내라~
		
		StaticTest t2 = new StaticTest();
		t2.msg = "대한민국"; 
		
		System.out.println(StaticTest.msg); //대한민국
	}
```
  하나만 만들고 싶다 (싱글톤)
  static 초기화
```java
class FactoryService{
private static sqlSeecionFactory factory;
static{
try{
Reader r = Resoutces.getResourcesReader(~.~onfig.xml)
factory = new sqlsessionfactoryBuilder.build(r);
r.close;
}catch(Exception e){
e.printStackTrace
}
}
}
}

```
factory 불러오기
```java
public static sqlsessionFactory getFactory(){
	return factory;
}
```