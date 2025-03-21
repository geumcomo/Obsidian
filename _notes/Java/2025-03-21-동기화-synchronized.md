---
---


여러 스레드가 접근하는 자원을 공유자원 에 대한 접근을 적절하게 동기화해서 동시성 문제가 발생하지 않게 방지해야한다.

## 임계영역

여러 스레드가 동시에 접근하면 데이터 불일치나 예상치 못한 동작이 발생할 수 있는 위험하고 또 중요한 코드 부분을 뜻한다.
synchronized키워드를 통해 안전하게 처리할 수 있다.

## Synchronized

- 메서드 접근제어자 뒤에 선언 
- 하나의 스레드만이 락을 부여(모든 인스턴스에는 락을 걸수 있음)
- 하나의 스레드가 메서드에서 작업중일때 다른 스레드는 락을 부여받을때까지 대기하고 있어야한다(Blocked)
- 굳이 volatile 을 사용하지않아도 메모리 가시성 문제 해결 가능하다.

### 단점
여러 스레드가 동시에 실행되지 못해서 성능이 떨어진다. 필요한 곳으로 한정해서 설정해야한다.

한정된 코드 블럭만 사용하려면 synchronized(this){안에 임계영역 코드를 넣으면 된다.

## 문제 1 - 공유 자원
```java
package thread.sync.test;  
  
public class SyncTestBadMain {  
    public static void main(String[] args) throws InterruptedException{  
        Counter counter = new Counter();  
        Runnable task = new Runnable() {  
            @Override  
            public void run() {  
                for (int i = 0; i < 10000; i++) {  
                    counter.increment();  
                }  
            }  
        };  
        Thread thread1 = new Thread(task);  
        Thread thread2 = new Thread(task);  
        thread1.start();  
        thread2.start();  
        thread1.join();  
        thread2.join();  
        System.out.println("결과: " + counter.getCount());  
    }  
  
    static class Counter {  
        private int count = 0;  
  
        public synchronized void increment() {  
            count  
                    = count + 1;  
        }  
  
        public int getCount() {  
            return count;  
        }  
    }  
}
```

## 문제 2 - 지역 변수의 공유
```java
package thread.sync.test;  
  
import util.MyLogger;  
  
public class SyncTest2Main {  
    public static void main(String[] args) throws InterruptedException {  
        MyCounter myCounter = new MyCounter();  
        Runnable task = new Runnable() {  
            @Override  
            public void run() {  
                myCounter.count();  
            }  
        };  
        Thread thread1 = new Thread(task, "Thread-1");  
        Thread thread2 = new Thread(task, "Thread-2");  
        thread1.start();  
        thread2.start();  
    }  
  
    static class MyCounter {  
        public void count() {  
            int localValue = 0;  
            for (int i = 0; i < 1000; i++) {  
                localValue = localValue + 1;  
            }  
            MyLogger.log("결과: " + localValue);  
        }  
    }  
}
```

## 문제 3 - final 필드
```java
package thread.sync.test;  
  
public class Immutable {  
    private final int value;  
  
    public Immutable(int value) {  
        this.value = value;  
    }  
  
    public int getValue() {  
        return value;  
    }  
}
```

> 여러 스레드가 공유 자원에 접근하는 것 자체는 사실 문제가 되지 않는다. 진짜 문제는 공유 자원을 사용하는 중간에 다른 스레드가 공유 자원의 값을 변경해버리기 때문에 발생한다. 결국 변경이 문제가 되는 것이다
{: .prompt-info}
> 여러 스레드가 접근 가능한 공유 자원이라도 그 값을 아무도 변경할 수 없다면 문제 되지 않는다. 이 경우 모든 스레드가 항상 같은 값을 읽기 때문이다. 필드에 된다. 정리 ` final ` 이 붙으면 어떤 스레드도 값을 변경할 수 없다. 따라서 멀티스레드 상황에 문제 없는 안전한 공유 자원이 된다.

