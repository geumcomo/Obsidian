# 문제 1
```java
package lang.string.test;  
  
public class TestString {  
    public static void main(String[] args) {  
    String url = "https://www.example.com";  
        boolean result = url.startsWith("https://");  
        System.out.println(result);  
    }  
  
}
```
# 문제 2
```java
package lang.string.test;  
  
public class TestString2 {  
    public static void main(String[] args) {  
        int sum = 0;  
        String[] arr = {"hello", "java", "jvm", "spring", "jpa"};  
        for (String s : arr) {  
            System.out.println(s + ":" + s.length());  
            sum += s.length();  
        }  
        System.out.println(sum);  
    }  
}
```

# 문제 3
```java
package lang.string.test;  
  
public class TestString3 {  
    public static void main(String[] args) {  
        String str = "hello.txt";  
        int index = str.indexOf(".txt");  
        System.out.println("index = "+ index);  
    }  
}
```

# 문제 4
```java
package lang.string.test;  
  
public class TestString4 {  
    public static void main(String[] args) {  
        String str = "hello.txt";  
        String filename = str.substring(0, 5);  
        String extName = str.substring(5);  
        System.out.println("filename: " + filename);  
        System.out.println("extName: " + extName);  
    }  
}
```

# 문제 5
```java
package lang.string.test;  
  
public class TestString5 {  
    public static void main(String[] args) {  
        String str = "hello.txt";  
        String ext = ".txt";  
        String filename = str.substring(0, str.indexOf(ext));  
        System.out.println("filename = " + filename);  
        System.out.println("extName = " + ext);  
    }  
}
```

# 문제 6
```java
package lang.string.test;  
  
public class TestString6 {  
    public static void main(String[] args) {  
        int count = 0;  
        String str = "start hello java, hello spring, hello jpa";  
        String key = "hello";  
        int index = str.indexOf(key);  
        while (index >= 0) {  
            index = str.indexOf(key, index + 1);  
            count++;  
        }  
        System.out.println("count" + count);  
    }  
}
```

# 문제 7
```java
package lang.string.test;  
  
public class TestString7 {  
    public static void main(String[] args) {  
        String original = "    Hello Java   ";  
        String trim = original.trim();  
        System.out.println(trim);  
    }  
}
```

# 문제 8
```java
package lang.string.test;  
  
public class TestString8 {  
    public static void main(String[] args) {  
        String input = "hello java spring jpa java";  
        String replace = input.replace("java", "jvm");  
        System.out.println(replace);  
    }  
}
```

# 문제 9
```java
package lang.string.test;  
  
public class TestString9 {  
    public static void main(String[] args) {  
        String email = "hello@example.com";  
        String[] split = email.split("@");  
        System.out.println("ID = " + split[0]);  
        System.out.println("Domain = " + split[1]);  
    }  
}
```

# 문제 10
```java
package lang.string.test;  
  
import static java.lang.String.join;  
  
public class TestString10 {  
    public static void main(String[] args) {  
        String fruits = "apple,banana,mango";  
        String[] splitfruit = fruits.split(",");  
        for (String fruit : splitfruit) {  
            System.out.println(fruit);  
        }  
        String joinedString = String.join("->", splitfruit);  
        System.out.println("joinedString = " + joinedString);  
    }  
}
```

# 문제 11
```java
package lang.string.test;  
  
public class TestString11 {  
    public static void main(String[] args) {  
        String str = "Hello Java";  
        String reverse = new StringBuilder(str).reverse().toString();  
        System.out.println(reverse);  
    }  
}
```
