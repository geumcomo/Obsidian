---
---

```java
package ex1.vo;

public class TestVO {
	private String msg;

	public String getMsg() {
		return msg;
	}

	public void setMsg(String msg) {
		this.msg = msg;
	}
	
}
```

```java
package ex1.vo;

public class Test2VO {
	private String str;
	private int value;

	public String getStr() {
		return str;
	}
	
	public Test2VO() {
		System.out.println("굿");
	}
	
	public void setStr(String str) {
		this.str = str;
	}

	public int getValue() {
		return value;
	}

	public void setValue(int value) {
		this.value = value;
	}

}
```

```java
package ex1.vo;

public class Test3VO {
	private String name;
	private int age;
	private boolean live;

	public Test3VO(String name, int age, boolean live) {
		super();
		this.name = name;
		this.age = age;
		this.live = live;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public boolean isLive() {
		return live;
	}

	public void setLive(boolean live) {
		this.live = live;
	}

}
```

```java
package ex1.vo;

public class Test4VO {
	private Test2VO test;

	public Test2VO getTest() {
		return test;
	}

	public void setTest(Test2VO test) {
		this.test = test;
	}
}
```

```java

```