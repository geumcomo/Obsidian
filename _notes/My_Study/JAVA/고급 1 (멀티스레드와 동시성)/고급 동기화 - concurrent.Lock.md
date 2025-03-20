## LockSupport
- park(); = waiting 
- unpark(); = runnable
- parkNano(); = timewaiting
세가지 모두 인터럽트를 사용하면 스레드를 깨울 수 있다.

## ReentrantLock
- 비공정 / 공정으로 나눌수있다.
- 공정으로 한다면 순서유지를 위해 성능이 느려질  수 있다.
- 비공정은 어느정도 새치기가 가능하다고 보면 된다.
-