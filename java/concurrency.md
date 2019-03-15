# Java Concurrency
`ReentrantLock` 과 `synchronized` 키워드 

### ReentrantLock
 `ReentrantLock` 은 java 에서 `synchronized` 키워드와 동일하게 메모리 가시성과 상호배제 기능을 제공한다.
 추가로 `synchronized` 키워드 만으로는 해결이 어려운 복잡한 상황에서 사용될 수 있는 몇몇 기능이 있다.
````java
import java.util.concurrent.locks.ReentrantLock; // ReentrantLock 은 concurrent package 에서 제공된다.

public class ExampleLock {
  private final ReentrantLock lock = new ReentrantLock();
  private int count = 0;
  
  public void increaseCount() {
    this.lock.lock(); // lock 획득
    try {
      count++;
    } finally {
      this.lock.unlock(); // lock 해제 
    }
  }
}
````
> 모든 작업이 명시적으로 진행되기 때문에 '명시적인 락'이라고 한다. 반면 `synchronized` 는 `암묵적인 락`이라고 한다. 

### ReentrantLock vs synchronized
#### fairness
우선 두 lock 은 공정성에 차이가 있다. `ReentrantLock` 은 공정성을 지원한다. **공정 방법**은 lock 을 획득하고자 하는 스레드를 항상 큐의 맨 끝에 추가한다. 
반면 `synchronized` 에서 제공되는 불공정 방법은 락을 획득하려는 시점에 락이 사용 중이라면 대기열에 들어가고, 획득하는 타이밍에 락이 해제가 될 경우 
대기열에 대기중인 스레드들보다 먼저 락을 획득할 수 있도록 한다. 
따라서 `ReentrantLock` 을 사용하면 대기 시간이 긴 스레드에 lock 를 제공하기 때문에 starvation 을 방지 할 수 있다.
> 그러나 공정 방법은 스레드를 반드시 멈추고, 다시 실행시키는 작업에 대한 비용을 추가적으로 소모하고 성능에 영향을 줄 수 있다는 단점이 있다. 

#### interrupt
`ReentrantLock` 은 lock 을 기다리는 동안 thread 인터럽트가 가능하다. `lockInterruptibly()` 메소드는 lock 을 대기 중일 때 스레드에 인터럽트를 걸 수 있다. 
`synchronized` 키워드는 대기 시간이 길어짐에 따라 무기한으로 block 될 수 있지만(불공정 lock 이므로) 제어할 수 있는 방법이 없다. 

#### tryLock
`ReentrantLock` 의 `tryLock()` 메소드로 lock 을 가져올 수 있는 경우에만 lock 을 획득하도록 한다. 
````java
if (!lock.tryLock(nanoTime, NANOSECONDS)) 
  return false;
````
`tryLock()` 을 사용하면 락을 확보하지 못하는 상황에서 제어권을 다시 얻는 것이 가능하다. 이 방법은 java 애플리케이션에서 
대기중인 스레드로 인한 blocking 을 줄일 수 있다.

#### API
`ReentrantLock` 은 lock 을 기다리고 있는 모든 스레드 목록을 가져올 수 있는 API 를 제공한다. 

위와 같은 장점이 있음에도 불구하고, 코드 가독성을 떨어뜨릴 수 있다는 단점이 있다. 
프로그래머는 lock 이 걸리고 해제되는 상황에 대해 정확하게 인지하고 있어야 한다. 
`ReentrantLock` 은 lock 을 획득한 후에 반드시 해제를 해주어야 하기 때문에 `try-finally` 블록으로 메소드 내부를 한번 감싸주게 되는데, 
프로그래머가 `finally` 블록에서 lock 을 해제하는 것을 잊어 버릴 경우, 문제가 발생할 수 있다. 

추가적으로 java5 부터 `ReentrantLock` 이 사용하능 하지만, 스레드 덤프를 통해 어느 메소드에 어떤 락이 걸려있는지, 데드락에 걸린 스레드가 있는지 등이 
확인하기 어렵다는 문제점이 있다. java6 이상부터 모니터링 인터페이스가 추가되어 스레드 덤프를 떠서 확인하는 것이 가능하다.
