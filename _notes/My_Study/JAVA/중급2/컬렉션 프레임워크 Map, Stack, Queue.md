# 문제 1
```java
package collection.map.test;  
  
import java.util.*;  
  
public class ArrayToMapTest {  
    public static void main(String[] args) {  
        String[][] productArr = {{"Java", "10000"}, {"Spring", "20000"},  
                {"JPA", "30000"}};  
        Map<String, Integer> productMap = new HashMap<>();  
        for (String[] product : productArr) {  
            productMap.put(product[0], Integer.valueOf(product[1]));  
        }  
        for (String key : productMap.keySet()) {  
            Integer value = productMap.get(key);  
            System.out.println("제품: " + key + ", " + "가격: " + value);  
        }  
    }  
}
```

# 문제 2
```java
package collection.map.test;  
  
import java.util.HashMap;  
import java.util.Map;  
  
public class CommonKeyValueSum1 {  
    public static void main(String[] args) {  
//        Map<String, Integer> map1 = Map.of("A", 1, "B", 2, "C", 3);  
//        Map<String, Integer> map2 = Map.of("B", 4, "C", 5, "D", 6);  
  
        Map<Character, Integer> map1 = new HashMap<>();  
        map1.put('A', 1);  
        map1.put('B', 2);  
        map1.put('C', 3);  
        Map<Character, Integer> map2 = new HashMap<>();  
        map2.put('B', 4);  
        map2.put('C', 5);  
        map2.put('D', 6);  
  
        Map<Character, Integer> result = new HashMap<>();  
  
        for (Character key : map1.keySet()) {  
            if (map2.containsKey(key)) {  
                result.put(key, map1.get(key) + map2.get(key));  
            }  
        }  
        System.out.println(result);  
    }  
}
```

# 문제 3
```java
package collection.map.test;  
  
import java.util.HashMap;  
import java.util.Map;  
  
public class WordFrequencyTest1 {  
    public static void main(String[] args) {  
        String text = "orange banana apple apple banana apple";  
  
        String[] words = text.split(" ");  
        Map<String, Integer> map = new HashMap<>();  
//        for (String word : words) {  
//            Integer count = map.get(word);  
//            if (count == null)  
//                count = 0;  
//            count++;  
//            map.put(word, count);  
//        }  
        for (String word : words) {  
            Integer count = map.getOrDefault(word, 0);  
            count++;  
            map.put(word, count);  
        }  
  
        System.out.println("map = " + map);  
    }  
}
```

# 문제 4
```java
package collection.map.test;  
  
import java.util.*;  
  
public class ItemPriceTest {  
    public static void main(String[] args) {  
//        Map<String, Integer> map = new HashMap<>();  
//        map.put("사과", 500);  
//        map.put("바나나", 500);  
//        map.put("망고", 1000);  
//        map.put("딸기", 1000);  
        Map<String, Integer> map = new HashMap<>(Map.of("사과", 500, "바나나", 500, "망고", 1000, "딸기", 1000));  
        List<String> list = new ArrayList<>();  
//        for(String key: map.keySet()){  
//            Integer value = map.get(key);  
//            if(value == 1000){  
//                list.add(key);  
//            }  
//        }  
        for (Map.Entry<String, Integer> entry : map.entrySet()) {  
            if (entry.getValue().equals(1000)) {  
                list.add(entry.getKey());  
            }  
        }  
        System.out.println(list);  
    }  
}
```

# 문제 5
```java
package collection.map.test;  
  
import java.util.HashMap;  
import java.util.Map;  
import java.util.Scanner;  
  
public class DictionaryTest {  
    public static void main(String[] args) {  
        Scanner scanner = new Scanner(System.in);  
        System.out.println("==단어 입력 단계==");  
        Map<String, String> dictionary = new HashMap<>();  
        while (true) {  
            System.out.println("영어 단어를 입력하세요 (종료는 'q'): ");  
            String english = scanner.next();  
            if (english.equals("q")) {  
                break;  
            }  
            System.out.println("한글 뜻을 입력하세요: ");  
            String korean = scanner.next();  
            dictionary.put(english, korean);  
        }  
        System.out.println("==단어 검색 단계==");  
        while (true) {  
            System.out.println("찾을 영어 단어를 입력하세요 (종료는 'q'): ");  
            String find = scanner.next();  
            if (find.equals("q"))  
                break;  
            if (dictionary.containsKey(find))  
                System.out.println(find + "의 뜻: " + dictionary.get(find));  
            else  
                System.out.println(find + "은(는) 사전에 없는 단어입니다.");  
        }  
    }  
}
```

# 문제 6

```java
package collection.map.test.member;  
  
public class Member {  
    private String id;  
    private String name;  
  
    public Member(String id, String name) {  
        this.id = id;  
        this.name = name;  
    }  
  
    public String getId() {  
        return id;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    @Override  
    public String toString() {  
        return "Member{" +  
                "id='" + id + '\'' +  
                ", name='" + name + '\'' +  
                '}';  
    }  
}
```

```java
package collection.map.test.member;  
  
public class MemberRepositoryMain {  
    public static void main(String[] args) {  
        Member member1 = new Member("id1", "회원1");  
        Member member2 = new Member("id2", "회원2");  
        Member member3 = new Member("id3", "회원3");  
// 회원 저장  
        MemberRepository repository = new MemberRepository();  
        repository.save(member1);  
        repository.save(member2);  
        repository.save(member3);  
        // 회원 조회  
        Member findMember1 = repository.findById("id1");  
        System.out.println("findMember1 = " + findMember1);  
        Member findMember3 = repository.findByName("회원3");  
        System.out.println("findMember3 = " + findMember3);  
        // 회원 삭제  
        repository.remove("id1");  
        Member removedMember1 = repository.findById("id1");  
        System.out.println("removedMember1 = " + removedMember1);  
    }  
}
```

```java
package collection.map.test.member;  
  
import java.util.HashMap;  
import java.util.Map;  
  
public class MemberRepository {  
    private Map<String, Member> memberMap = new HashMap<>();  
  
    public void save(Member member) {  
        memberMap.put(member.getId(), member);  
    }  
  
    public void remove(String id) {  
        memberMap.remove(id);  
    }  
  
    public Member findById(String id) {  
        return memberMap.get(id);  
    }  
  
    public Member findByName(String name) {  
        for (Member member : memberMap.values()) {  
            if (member.getName().equals(name)) {  
                return member;  
            }  
        }  
        return null;  
    }  
}
```

# 문제 7
```java
package collection.map.test.cart;  
  
import java.util.Objects;  
  
public class Product {  
    private String name;  
    private int price;  
  
    public Product(String name, int price) {  
        this.name = name;  
        this.price = price;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public int getPrice() {  
        return price;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (o == null || getClass() != o.getClass()) return false;  
        Product product = (Product) o;  
        return price == product.price && Objects.equals(name, product.name);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(name, price);  
    }  
  
    @Override  
    public String toString() {  
        return "Product{" +  
                "name='" + name + '\'' +  
                ", price=" + price +  
                '}';  
    }  
}
```

```java
package collection.map.test.cart;  
  
import java.util.HashMap;  
import java.util.Map;  
  
public class Cart {  
    Map<Product, Integer> cartMap = new HashMap<>();  
  
    public void add(Product product, int quantity) {  
        if (cartMap.containsKey(product)) {  
            quantity = cartMap.get(product) + quantity;  
            cartMap.put(product, quantity);  
        }  
        cartMap.put(product, quantity);  
    }  
  
    public void minus(Product product, int quantity) {  
        int minus = cartMap.get(product) - quantity;  
        if (minus == 0) {  
            cartMap.remove(product);  
        }  
    }  
  
    public void printAll() {  
        System.out.println("== 모든 상품 출력");  
        for (Map.Entry<Product, Integer> entry : cartMap.entrySet()) {  
            System.out.println("상품: " + entry.getKey() + "," + "수량: " + entry.getValue());  
        }  
    }  
}
```

```java
package collection.map.test.cart;  
  
public class CartMain {  
    public static void main(String[] args) {  
  
        Cart cart = new Cart();  
        cart.add(new Product("사과", 1000), 1);  
        cart.add(new Product("바나나", 500), 1);  
        cart.printAll();  
        cart.add(new Product("사과", 1000), 2);  
        cart.printAll();  
        cart.minus(new Product("사과", 1000), 3);  
        cart.printAll();  
    }  
}
```

# Stack
```java
package collection.stack.test;  
  
import java.util.ArrayDeque;  
import java.util.Deque;  
  
public class SimpleHistoryTest {  
    public static void main(String[] args) {  
//        Stack<String> stack = new Stack<>();  
        Deque<String> stack = new ArrayDeque<>();  
  
        stack.push("youtube.com");  
        stack.push("google.com");  
        stack.push("facebook.com");  
  
        System.out.println(stack.pop());  
        System.out.println(stack.pop());  
        System.out.println(stack.pop());  
    }  
  
}
```


---

# Deque
```java
package collection.stack.test;  
  
import java.util.ArrayDeque;  
import java.util.Deque;  
  
public class BrowserHistory {  
    Deque<String> history = new ArrayDeque<>();  
    String currentPage = null;  
  
    public void visitPage(String url) {  
        if (currentPage != null) {  
            history.push(currentPage);  
        }  
        currentPage = url;  
        System.out.println("방문: " + url);  
    }  
  
    public String goBack() {  
        if (!history.isEmpty()) {  
            currentPage = history.pop();  
            System.out.println("뒤로 가기: " + currentPage);  
            return currentPage;  
        }  
        return null;  
    }  
}
```

```java
package collection.stack.test;  
  
public class BrowserHistoryTest {  
    public static void main(String[] args) {  
        BrowserHistory browser = new BrowserHistory();  
        // 사용자가 웹페이지를 방문하는 시나리오  
        browser.visitPage("youtube.com");  
        browser.visitPage("google.com");  
        browser.visitPage("facebook.com");  
        // 뒤로 가기 기능을 사용하는 시나리오  
        String currentPage1 = browser.goBack();  
        System.out.println("currentPage1 = " + currentPage1);  
        String currentPage2 = browser.goBack();  
        System.out.println("currentPage2 = " + currentPage2);  
    }  
}
```

# queue
```java
package collection.deque.test.queue;  
  
import java.util.ArrayDeque;  
import java.util.Queue;  
  
public class PrinterQueueTest {  
    public static void main(String[] args) {  
        Queue<String> printQueue = new ArrayDeque<>();  
        printQueue.offer("doc1");  
        printQueue.offer("doc2");  
        printQueue.offer("doc3");  
  
        System.out.println("출력: "+ printQueue.poll());  
        System.out.println("출력: "+ printQueue.poll());  
        System.out.println("출력: "+ printQueue.poll());  
    }  
}
```


---
```java
package collection.deque.test.queue;  
  
import java.util.ArrayDeque;  
import java.util.Queue;  
  
public class TaskScheduler {  
    Queue<Task> tasks = new ArrayDeque<>();  
  
    public void addTask(Task task){  
        tasks.offer(task);  
    }  
  
    public int getRemainingTasks(){  
        return tasks.size();  
    }  
  
    public void processNextTask(){  
        Task task = tasks.poll();  
        if(task != null)  
            task.execute();  
    }  
}
```

```java
package collection.deque.test.queue;  
  
public class SchedulerTest {  
    public static void main(String[] args) {  
        //낮에 작업을 저장  
        TaskScheduler scheduler = new TaskScheduler();  
        scheduler.addTask(new CompressionTask());  
        scheduler.addTask(new BackupTask());  
        scheduler.addTask(new CleanTask());  
        //새벽 시간에 실행  
        System.out.println("작업 시작");  
        run(scheduler);  
        System.out.println("작업 완료");  
    }  
    private static void run(TaskScheduler scheduler) {  
        while (scheduler.getRemainingTasks() > 0) {  
            scheduler.processNextTask();  
        }  
    }  
}
```
