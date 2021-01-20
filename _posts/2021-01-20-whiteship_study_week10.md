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











## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [커멘드 패턴(Command Pattern)](https://johngrib.github.io/wiki/command-pattern/)
