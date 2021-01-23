---
title: "[#10 백기선님 라이브 스터디] 멀티쓰레드 프로그래밍"
date: 2021-01-20 15:49:10 -0900
categories: whiteship
---

# 목표
자바의 멀티쓰레드 프로그래밍에 대해 학습하세요.

# 학습 내용
* Thread 클래스와 Runnable 인터페이스
* 쓰레드의 상태
* 쓰레드의 우선순위
* Main 쓰레드
* 동기화
* 데드락



## Thread란?

프로세스의 자원을 이용해서 실제로 작업을 실행 하는 것



### Process vs Thread

Process는 *실행중인 프로그램* 을 의미하며 실행하면 OS로부터 자원을 할당 받아서 프로세스가 된다.  Thread는 프로세스의 일부분이라고 보면 되며, 모든 프로세스는 1개 이상의 Thread를 가지고 있다.

### Single Thread Process vs Multi Thread Process

Single Thread Process: 자원 + Thread

Multi Thread Process: 자원 + Thread + Thread + ....

> 각 프로세스가 가질 수 있는 Thread의 수는 제한되어 있지 않으나, 각 Thread별로 개별의 메모리 공간 (Call Stack)이 필요하기에, **프로세스의 메모리 한계(call stack size)에 따라 생성할 수 있는 Thread의 수가 결정된다.

###  Multi Tasking vs Multi Threading

Multi Tasking: 여러개의 *프로세스*가 동시에 실행 되는 것 (프로세스가 2개 이상)

Multi Threading: 하나의 프로세스에서 여러개의 Thread가 실행 되는 것 (Thread가 2개 이상)

> Multi Threading이 무조건 빠른 것은 아니다. 실제로는 한개의 CPU가 한번에 단 한가지 작업만 수행할 수 있기에 아주 짧은 시간동안 여러 작업을 번갈아 수행함으로써 동시에 여러작업이 수행되는 것 처럼 보이는 것이다.

### Multi Threading의 장점 및 고려사항

| 장점                                                         | 고려사항                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - CPU 사용률을 향상시켜 준다.<br />- 자원을 보다 효율적으로 사용한다. <br />- 사용자에 대한 응답성이 향상된다. <br />- 작업이 분리되어 코드가 간결해 질 수 있다. | - 동기화 이슈가 발생할 수 있다.(Synchronization)<br />- 교착상태가 발생할 수 있다. (Dead Lock) |



## Thread 클래스와 Runnable 인터페이스

Thread를 구현하는 방법은 아래 두가지가 있다.

1. Thread 클래스를 상속 받는 방법

   ```java
   public class ThreadChild extends Thread {
   
       @Override
       public void run() {
            System.out.println("Custom Thread run!! ---- " + getName());
       }
   }
   
   public class ThreadExample {
   
       public static void main(String[] args) {
           System.out.println("Main Start! ---- " + Thread.currentThread().getName());
           ThreadChild threadChild = new ThreadChild();
           threadChild.start();
       }
   }
   ```

   getName() 은 Thread 클래스에서 제공해주는 final 메소드이며 현재 Thread의 이름을 반환해준다. 

   Main 메소드에서는 Thread를 상속받고 있지 않기에 Thread.currentThread().getName(); 을 이용해서 이름을 반환 받는다.

   

   ```
   Main Start! ---- main
   Custom Thread run!! ---- Thread-0
   ```

2. Runnable 인터페이스를 구현하는 방법

```java
public class MyRunnableImpl implements Runnable {

    @Override
    public void run() {
        System.out.println("Custom Thread run!! ---- " + Thread.currentThread().getName()); 
    }
}

public class ThreadExample {

    public static void main(String[] args) {
        System.out.println("Main Start! ---- " + Thread.currentThread().getName());
        Thread thread = new Thread(new MyRunnableImpl());

        thread.start();
    }

}
```

Runnable 인터페이스를 구현하고 Thread 클래스의 인스턴스를 생성할 때, 구현한 클래스를 넘겨준다. 이러한 방식은 Thread 클래스가 **Command Pattern** 을 사용함을 보여준다.

```java
Main Start! ---- main
Custom Thread run!! ---- Thread-0
```



#### Thread class (Open JDK 11.0.2)

```java
public class Thread implements Runnable {
	//... 중략 ...

   /* What will be run. */
    private Runnable target;
	
	//... 중략 ...

	private Thread(ThreadGroup g, Runnable target, String name,
                   long stackSize, AccessControlContext acc,
                   boolean inheritThreadLocals) {
        //... 중략 ...         
        
        this.target = target
                  
				//... 중략 ...
  }
  
  
     /**
     * If this thread was constructed using a separate
     * {@code Runnable} run object, then that
     * {@code Runnable} object's {@code run} method is called;
     * otherwise, this method does nothing and returns.
     * <p>
     * Subclasses of {@code Thread} should override this method.
     *
     * @see     #start()
     * @see     #stop()
     * @see     #Thread(ThreadGroup, Runnable, String)
     */
    @Override
    public void run() {
        if (target != null) {
            target.run(); // <- Runnable의 run을 결국 캡슐화 해서 호출한다.
        }
    }
}
```



> Thread의 run 메소드를 호출하면 Runnable의 run()이 캡슐화 되어있다.
>
> Thread 인스턴스 화시 Runnable 인스턴스를 넘겨주지 않으면, 아무런 동작을 하지 않는다.



![IMG_4A21BECB3D00-1](https://user-images.githubusercontent.com/37217320/105184456-b40bbe80-5b72-11eb-9621-347ba189ec13.jpeg)



### Command Pattern

 디자인 패턴중 행위(Behavioral)에 해당하는 패턴이며, 실행될 기능을 캡슐화해서 재사용성이 높은 클래스를 설계하는 패턴이다.

> 요구 사항을 객체로 캡슐화할 수 있으며, 매개변수를 써서 여러 가지 다른 요구 사항을 집어넣을 수도 있습니다. 또한 요청 내역을 큐에 저장하거나 로그로 기록할 수도 있으며, 작업취소 기능도 지원 가능합니다.
> [Head First Design Pattern 중 Command Pattern에 대한 설명]



### run() 와 start()

Thread를 정의하고 실행할때 우리는 start() 메소드를 호출하였다.

start() 메소드를 호출시 아래와 같은 과정을 거친다.

1. main Thread에서 start() 메소드를 실행
2. 신규 Thread를 사용할 Call Stack을 내부적으로 생성
3. 신규 Call Stack에 run() 메소드를 첫번째로 넣는다.
4. 신규 쓰레드가 종료되면 사용된 Call Stack은 소멸한다.

![IMG_E77DED51A766-1](https://user-images.githubusercontent.com/37217320/105186143-be2ebc80-5b74-11eb-8b97-88d0e5148abb.jpeg)

[참고: Java의 정석[2판]]



즉 위와 같은 start() 메소드를 사용해야 새로운 Thread가 생성 과정을 거치게 된다. 그러나 run() 메소드를 호출한다면 메인 Thread에서 run()이라는 메소드를 호출 하는 것 이다.

```java
public class ThreadExample {

    public static void main(String[] args) {
        System.out.println("Main Start! ---- " + Thread.currentThread().getName());
        Thread thread = new Thread(new MyRunnableImpl());

        thread.start(); // 메인 쓰레드에서 단순 run() 메소드 실행
    }

}
```



```java
Main Start! ---- main
Custom Thread run!! ---- main
```





## Thread의 상태

Thread의 상태는 java.lang.Thread 클래스 내부의 State라는 이름을 가진 enum type으로 되어있다.

상태의 종류는 아래와 같다.

|              |                                                              |
| ------------ | ------------------------------------------------------------ |
| NEW          | 스레드가 생성되었고, 아직 실행되지 않은 상태                 |
| RUNNABLE     | 현재 CPU를 점유하고 작업을 수행 중인 상태 (OS의 자원 분배로 WAITING 상태가 될 수 도 있다.) |
| BLOCKED      | Monitor를 획득하기 위해 다른 스레드가 락을 해제하기를 기다리는 상태 |
| WAITING      | wait(), join(), peek() 메소드를 이용해 대기중인 상태         |
| TIME_WAITING | sleep(), wait(), join(), peek() 메소드 등을 이용해 대기하는 상태 (WAITING 상태와 다른점은  TIMEOUT 시간을 지정할 수 있으며, 지정한 시간에 의해서도 WAITING 상태가 해지될 수 있음) |
| TERMINATED   | 스레드가 종료된 후 자원이 반납 된 상태                       |



쓰레드의 상태는 java 1.5 이상부터 getState() 함수를 통해서 가져올 수 있다.



```java
import java.lang.Thread.State;

public class ThreadExample {

    public static void main(String[] args) throws InterruptedException {
        Thread newThread = new Thread(new MyRunnableImpl());

        System.out.println("myThreadstate = " + newThread.getState()); // NEW
        System.out.println("mainThreadState = " + Thread.currentThread().getState()); // RUNNABLE
        newThread.start();

        Thread.sleep(1000);

        System.out.println("----------------------------------------");
        System.out.println("myThreadstate = " + newThread.getState()); // TERMINATED
        System.out.println("mainThreadState = " + Thread.currentThread().getState()); // RUNNABLE
    }

}

```

```java
myThreadstate = NEW
mainThreadState = RUNNABLE
----------------------------------------
myThreadstate = TERMINATED
mainThreadState = RUNNABLE
```



### Thread의 실행제어

또한 Thread의 상태는 메서드를 통해 직접 실행 제어를 할 수 있다.

| method                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| void interrupt()                                             | sleep()이나, join()에 의해 일시정지 상태인 Thread를 실행대기 상태로 만든다. 해당 Thread는 InterruptedException이 발생함으로 일시 정지 상태를 벗어나게 된다. |
| void join()<br />void join(long millis)<br />void join(long millis, int nanos) | 지정된 시간동안 쓰레드가 실행되도록 한다. 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 Thread로 다시 돌아와 실행을 계속한다. |
| ~~void resume()~~                                            | suspend()에 의한 일시정지 상태에 있는 Thread를 실행대기상태로 만든다. |
| static void sleep(long millis)<br />static void sleep(long millis, int nanos) | 지정된 시간동안 Thread를 일시정지 시킨다. 지정 시간 후에 다시 실행대기 상태가 된다. |
| ~~void stop()~~                                              | Thread를 즉시 종료시킨다. 교착상태(Dead Lock)에 빠지기 쉽기 때문에 deprecated되었다. |
| ~~void suspend()~~                                           | Thread를 일시정지 시킨다. resume()을 호출하면 다시 실행대기 상태가 된다. |
| static void yield                                            | 실행중 다른 Thread에게 양보(yield)하고 실행 대기 상태가 된다. |



## Thread의 우선순위

Thread에는 우선순위(priority)라는 속성(멤버변수)을 가지고 있다. 이는 우선순위가 높은 쓰레드에게 더 많은 양의 실행시간을 제공하고, 더 빨리 작업이 완료 될 수 있도록 한다.



### Thread의 우선순위 메소드

```java
void setPriority(int newPriority); // Thread의 우선순위를 지정한다.
int getPriority(); // Thread의 우선순위를 반환한다.

public static final int MAX_PRIORITY = 10; // 최대 우선 순위
public static final int MIN_PRIORITY = 1; // 최소 우선 순위
public static final int NORM_PRIORITY = 5; // 보통 우선 순위
```

Thread가 가질 수 있는 우선순위의 범위는 1~10 이며 우선순위는 절대적이 아니라 상대적이다.

우선순위는 Thread를 생성한 Thread로부터 상속 받는다. main Thread는 default 5다.



```java
import java.lang.Thread.State;

public class ThreadExample {

    public static void main(String[] args) throws InterruptedException {
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                System.out.print("-");
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                System.out.print("|");
            }
        });
        thread2.setPriority(9);

        int thread1Priority = thread1.getPriority();
        System.out.println("thread1Priority(-) = " + thread1Priority);
        int thread2Priority = thread2.getPriority();
        System.out.println("thread2Priority(|) = " + thread2Priority);

        thread1.start();
        thread2.start();
        
    }
}

```



```java
thread1Priority(-) = 5
thread2Priority(|) = 9
--||||||||||||||||||||||||----|||-----------------------------------|||||||||||-----||||---|||---|||-----||||||||---||||||||||||||||||||||------||||||-----|||-------|||---|||||||------------|||-------

```



## Main Thread

자바 어플리케이션이 실행되면, JVM은 main과 system이라는 Thread 그룹을 만들고 JVM운영에 필요한 Thread들을 이 그룹에 포함 시킨다.
main 메서드를 수행하는 main이라는 이름의 Thread는 main Thread의 그룹에 속한다. 또한 우리가 생성하는 Thread는 자동적으로 main 그룹에 속하게 된다.



## 동기화

Multi Thread의 경우 여러 Thread가 같은 프로세스 내의 자원을 공유해서 작업을 하기에 서로의 작업에 영향을 주게 된다. 개발자가 의도하는 결과를 얻기 위해 동기화는 매우 중요한 요소이다.



### Synchronized

synchronized 키워드를 이용해 작업에 관련된 공유 데이터에 lock을 걸어 먼저 작업중이던 Thread에게 Thread-safe 하게 구성할 수 있다.

> 메소드에 synchronized를 붙이는 방법과, synchronized 블록을 이용해 사용할 수 있다.



1000원의 잔고가 있는 통장에서 두명이 랜덤하게 출금을 한다고 가정해보자

```java
public class SyncDemo {

    public static void main(String[] args) {
        MyRunnable ru = new MyRunnable();
        Thread thread1 = new Thread(ru);
        Thread thread2 = new Thread(ru);

        thread1.start();
        thread2.start();
    }
}

public class MyRunnable implements Runnable {

    Account account = new Account();

    @Override
    public void run() {
        while (account.balance >0) {
            int money = (int) (Math.random() * 3 + 1) * 100;
            account.withdraw(money);
            System.out.println("balance: " + account.balance);
        }
    }
}


public class Account {
    int balance = 1000;

    public void withdraw(int money) {
        if(balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            balance -= money;
        }
    }

}

```



```bash
balance: 700
balance: 700
balance: 500
balance: 200
balance: 200
balance: 200
balance: 100
balance: -100 // 결과가 음수로 엉망이다.
```



Account 내의 withdraw 를 타 쓰레드에 영향을 받지 않도록 synchronized 키워드를 이용해 개선해보자.



```java
public synchronized void withdraw(int money) {
    if(balance >= money) {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        balance -= money;
    }
} // 메소드에 synchronized

public void withdraw(int money) {
  	synchronized (this) {
    		if (balance >= money) {
      			try {
       	 				Thread.sleep(1000);
      			} catch (InterruptedException e) {
        				e.printStackTrace();
      			}

      		balance -= money;
        }
  	}
} // synchronized block을 이용한 방법

```

```java
balane: 800
balane: 600
balane: 500
balane: 300
balane: 200
balane: 0
balane: 0
```

이렇게하면 synchronized블럭에 들어가면 Account 객체 전체에 lock 이 걸리므로 다른 쓰레드들은 이 객체에 접근할 수 없다. 전과는 달리 음수값이 나오지 않는다.



## Dead Lock

데드락은 프로세스가 자원을 얻지 못해 다음 처리를 못하는 상태로, 교착 상태라고도 하며 시스템적으로 한정된 자원을 여러곳에서 사용하려고 할 때 발생한다.

![IMG_89470B8EF419-1](https://user-images.githubusercontent.com/37217320/105577810-253fb180-5dbf-11eb-8101-3fbe53eea89c.jpeg)

process1이 Resource1을 할당받았고 process2가 Resource2를 할당받은채로 해당 Resource에 Lock이 걸려있다면, 두 프로세스는 무한정 기다리게 되는 현상이다.



###  Dead Lock의 발생 조건

Dead Lock은 다음 네가지 조건 중 하나라도 성립하지 않으면 Dead Lock을 해결할 수 있다.



1. 상호배제 (Multual exclusion)
   - 자원(Resource)는 한 번에 한 프로세스만이 사용할 수 있어야 한다.
2. 점유대기 (Hold and wait)
   - 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 있어야 한다.
3. 비선점(No preemption)
   - 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없어야 한다.
4. 순환 대기(Circular wait)
   - 프로세스의 집합 {P0, P1, ,..Pn} 에서 P0은 P1이 점유한 자원을 대기하고 P1은 P2가 점유한 자원을 대기하고 P2 ... Pn-1은 Pn이 점유한 자원을 대기하며, Pn은 P0가 점유한 자원을 요구해야한다.





## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [커멘드 패턴(Command Pattern)](https://johngrib.github.io/wiki/command-pattern/)
>
> [쓰레드 덤프 분석하기](https://d2.naver.com/helloworld/10963)
>
> [Deadlock 개념이란? 그에 대한 해결책/회피책](https://jwprogramming.tistory.com/12)

