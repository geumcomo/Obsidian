만약  서버 기능을 업데이트를 위해서 재시작해야 한다고 가정해보자.
서버 애플리케이션이 고객의 주문을 처리하고 있는 도중에 갑자기 재시작 된다면, 해당 고객의 주문이 제대로 진행되지 못할 것이다.
==가장 이상적인 방향은 새로운 주문 요청은 막고, 이미 진행중인 주문은 모두 완료한 다음에 서버를 재시작 하는 것이 가장 좋을 것이다.

> 이렇게 문제 없이 우아하게 종료하는 방식을 grceful shutdown이라 한다.

---

## Executor 종료 메서드

### shutdown()
- 작업이 없는 경우: 새로운 요청을 거절하고 스레드 풀을 정리한다.
- 작업이 있는 경우: 큐에 남아있는 작업을 꺼내서 완료한다.

### awaitTermination(10, TimeUnit.SECONDS)
- main 스레드는 대기하며 서비스 종료를 10초간 기다린다.

### shutdown()
1. 큐를 비우면서 , 큐에 있는 작업을 모두 꺼네서 컬렉션으로 반환한다.
2. 작업 중인 스레드에 인터럽트 발생한다.

### close()
1. shutdown() 호출하고
2. 하루를 기다려도 작업이 완료되지 않으면 shutdownNow()를 호출한다.


## Executor 스레드 풀 관리

### ThreadPoolExecutor(corePoolSize, maxiumPoolSize,keepAliveTime, TimeUnit, blockingQueue)

스레드 요청이 계속 들어온다고 가정하자.

1. 코어 사이즈를 확인한다.
2. 코어 사이즈가 가득차면 큐에 보관한다.
3. 큐에 가득 차면 max 사이즈만큼 초과 스레드 만큼 늘린다.
4. 초과스레드와 큐에도 가득 차면 요청을 거절한다. (예외가 발생한다.)
5. keepAliveTime 까지 작업을 하지 않고 대기하면 초과스레드는 사라진다.

### pprestartAllCoreThreads

- 먼저 스레드를 만들어 놓는다. (트래픽이 많다면 시간을 줄이기 위해서 사용한다.)


## 고정 풀 전략

- 스레드 풀에 nTreads 만큼의 기본 스레드를 생성한다. 초과 스레드는 생성하지 않는다.
- 큐 사이에 제한이 없다. (LinkedBlockingQueue)
- 스레드 수가 고정되어 있어 CPU, 메모리 리소스가 어느정도 예측 가능한 안전한 방식이다.

```java
new ThreadPoolExecutor(1, 1,0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue())
```

## 캐시 풀 전략

- 기본 스레드를 사용하지 않고 60초 생존 주기를 가진 초과 스레드만 사용한다.
- 초과 스레드의 수는 제한이 없다.
- 큐에 작업을 저장하지 않는다. ( SynchronousQueue )
	- 대신에 생산자의 요청을 스레드 풀의 소비자 스레드가 직접 받아서 바로 처리한다.
- 모든 요청이 대기하지 않고 스레드가 바로바로 처리한다. 빠른 처리가 가능하다.

```java
new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue());
```
```
Integer.MAX_VALUE = 무제한 
60L = 60초
```

> [!NOTE]
> 고정 스레드 풀 전략은 서버 자원은 여유가 있는데, 사용자만 점점 느려지는 문제가 발생할 수 있다. 반면에 캐시 스레드 풀 전략은 서버의 자원을 최대한 사용하지만, 서버가 감당할 수 있는 임계점을 넘는 순간 시스템이 다운될 수 있다.


## 사용자 정의 풀 전략

```java
ExecutorService es = new ThreadPoolExecutor(100, 200, 60, TimeUnit.SECONDS, new ArrayBlockingQueue<>(1000));
```

- 100개의 기본 스레드를 사용한다. 
- 추가로 긴급 대응 가능한 긴급 스레드 100개를 사용한다. 긴급 스레드는 60초의 생존 주기를 가진다.
- 1000개의 작업이 큐에 대기할 수 있다.

## 실무에서 자주 하는 실수

```java
new ThreadPoolExecutor(100, 200, 60, TimeUnit.SECONDS, new LinkedBlockingQueue());
```

- 기본 스레드 100개 
- 최대 스레드 200개 
- 큐 사이즈: 무한대

> [!NOTE] 
> 이렇게 설정하면 절대로 최대 사이즈 만큼 늘어나지 않는다. 왜냐하면 큐가 가득차야 긴급 상황으로 인지 되는데, ` LinkedBlockingQueue ` 를 기본 생성자를 통해 무한대의 사이즈로 사용하게 되면, 큐가 가득찰 수 가 없다. 결국 기 본 스레드 100개만으로 무한대의 작업을 처리해야 하는 문제가 발생한다. 실무에서 자주하는 실수 중에 하나이다.

## Executor 예외 정책

==ThreadPoolExecutor에 작업을 요청할 때, 큐도 가득차고, 초과 스레드도 더는 할당할 수 없다면 작업을 거절한다.==

ThreadPoolExecutor는 작업을 거절하는 다양한 정책을 제공한다.

- **AbortPolicy**: 새로운 작업을 제출할 때 RejectExecutionException을 발생시킨다. (기본정책)
- **DiscardPolicy**: 새로운 작업을 조용히 버린다.
- **CallerRunsPolicy**: 새로운 작업을 제출한 스레드가 대신해서 직접 작업을 실행한다.
- **사용자 정의(RejectedExecutionHandler )**: 개발자가 직접 정의한 거절 정책을 사용할 수 있다.
