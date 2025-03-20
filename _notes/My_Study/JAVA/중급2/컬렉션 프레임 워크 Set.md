# 문제 1
```java
package collection.set.test;  
  
import java.util.HashSet;  
import java.util.Set;  
  
public class UniqueNamesTest1 {  
    public static void main(String[] args) {  
        Integer[] inputArr = {30, 20, 20, 10, 10};  
        Set<Integer> set = new HashSet<>();  
        for (Integer num : inputArr) {  
            set.add(num);  
        }  
//        set.add(30);  
//        set.add(20);  
//        set.add(20);  
//        set.add(10);  
//        set.add(10);  
        for (Integer num : set) {  
            System.out.println(num);  
        }  
    }  
}
```

# 문제 2
```java
package collection.set.test;  
  
import java.util.HashSet;  
import java.util.LinkedHashSet;  
import java.util.List;  
import java.util.Set;  
  
public class UniqueNamesTest2 {  
    public static void main(String[] args) {  
        Integer[] inputArr = {30, 20, 20, 10, 10};  
        Set<Integer> set = new LinkedHashSet<>(List.of(inputArr));  
  
        for (Integer num : set) {  
            System.out.println(num);  
        }  
    }  
}
```

> [!NOTE]
> Set` 구현체의 생성자에 배열은 전달할 수 없지만 ` List` 는 전달할 수 있다. 다음과 같이 배열을 ` List` 로 변환한다.

# 문제 3
```java
package collection.set.test;  
  
import java.util.*;  
  
public class UniqueNamesTest3 {  
    public static void main(String[] args) {  
        Set<Integer> set = new TreeSet<>(List.of(30, 20, 20, 10, 10));  
  
        for (Integer num : set) {  
            System.out.println(num);  
        }  
    }  
}
```

# 문제 4
```java
package collection.set.test;  
  
import java.util.HashSet;  
import java.util.List;  
import java.util.Set;  
import java.util.TreeSet;  
  
public class SetOperationsTest {  
    public static void main(String[] args) {  
        Set<Integer> set1 = new HashSet<>(List.of(1, 2, 3, 4, 5));  
        Set<Integer> set2 = new HashSet<>(List.of(3, 4, 5, 6, 7));  
  
        Set<Integer> union  = new TreeSet<>(set1);  
        union.addAll(set2);  
  
        Set<Integer> intersection = new TreeSet<>(set1);  
        intersection.retainAll(set2);  
  
        Set<Integer> difference = new TreeSet<>(set1);  
        difference.removeAll(set2);  
  
        System.out.println("합집합 = " + union);  
        System.out.println("교집합 = " + intersection);  
        System.out.println("차집합 = " + difference);  
    }  
}
```

# 문제 5
```java
package collection.set.test;  
  
import java.util.Objects;  
  
public class Rectangle {  
    private int width, height;  
  
    public Rectangle(int width, int height) {  
        this.width = width;  
        this.height = height;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (o == null || getClass() != o.getClass()) return false;  
        Rectangle rectangle = (Rectangle) o;  
        return width == rectangle.width && height == rectangle.height;  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(width, height);  
    }  
  
    @Override  
    public String toString() {  
        return "Rectangle{" +  
                "width=" + width +  
                ", height=" + height +  
                '}';  
    }  
}
```

```java
package collection.set.test;  
  
import java.util.HashSet;  
import java.util.Set;  
  
public class RectangleTest {  
    public static void main(String[] args) {  
        Set<Rectangle> rectangles = new HashSet<>();  
        rectangles.add(new Rectangle(10, 10));  
        rectangles.add(new Rectangle(10, 10));  
        rectangles.add(new Rectangle(20, 20));  
  
        for(Rectangle rectangle : rectangles){  
            System.out.println("rectangle = " + rectangle);  
        }  
    }  
}
```