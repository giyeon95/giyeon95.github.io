---
title: "[WHITESHIP #1] 6. 상속"
date: 2020-12-25 23:57:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[`#Season1 백기선님과 함께하는 자바 온라인 스터디`](https://github.com/whiteship/live-study)

# 목표

자바의 상속에 대해 학습해보자

# 학습 내용
* 자바 상속의 특징
* super 키워드
* 메소드 오버라이딩
* 다이나믹 메소드 디스패치(Dynamic Method Dispatch)
* 추상 클래스
* final 키워드
* Object 클래스

## 1. 자바 상속의 특징

### 1.1 상속이란?

 상속이란, OOP(Object Oriented Programming)에서 하나의 중요한 개념이다.
 Java에서의 상속은 부모 클래스의 변수/ 메소드를 자식 클래스가 물려받아 그대로 사용할 수 있게 해준다.

###  1.2 상속의 특징

* 부모 클래스(super class)의 변수/ 메소드를 자식 클래스(sub class)가 재사용할 수 있다.
* 메소드 오버라이딩을 통해 부모 클래스(super class)의 메소드를 재정의 할 수도 있다.
* 중복 코드를 줄여준다. (재사용)
* Java에서는 하나의 자식 클래스(sub class)는 하나의 부모 클래스(super class)만 상속 받을 수 있다.(단일 상속)

### 1.3 상속 예제

> 숙박업소의 방의 가격, 구조를 출력해주는 프로그램을 만들어보자.
>
> Room을 부모 클래스, TwinRoom, VIPRoom 을  자식 클래스로 정의하였다.

```java
import java.util.Arrays;
import java.util.List;

public class Room {

    public int price = 100_000;
    public List<String> facilities = Arrays.asList("침대", "드라이기", "이불", "세면용품");

    public int getPrice() {
        return price;
    }

    public List<String> getFacilities() {
        return facilities;
    }
  
  	public void printWarningPoint() {
        System.out.println("기본방의 주의사항은 없습니다.");
    }
    
    
}


public class TwinRoom extends Room {
		public int price = 150_000;
}

public class VIPRoom extends Room {

}


public class Main {

    public static void main(String[] args) {
        TwinRoom twinRoom = new TwinRoom();
        VIPRoom vipRoom = new VIPRoom();

        System.out.println("=========" +twinRoom.getClass().getName() + "=========");
        printPrice(twinRoom.price);
        printFacilities(twinRoom.facilities);

        System.out.println("=========" +vipRoom.getClass().getName() + "=========");
        printPrice(vipRoom.price);
        printFacilities(vipRoom.facilities);
    }


    public static void printPrice(int price) {
        System.out.println("기본 방의 가격은 " + price + "원 입니다.");
    }

    public static void printFacilities(List<String> facilities) {
        StringBuilder sb = new StringBuilder("방의 포함 물품은 ");
        facilities.forEach(s -> sb.append(s).append(", "));
        sb.replace(sb.length() - 2, sb.length(), "");
        sb.append("입니다.");

        System.out.println(sb.toString());
    }
}

```

```bash
=========inheritance.TwinRoom=========
기본 방의 가격은 150000원 입니다.
방의 포함 물품은 침대, 드라이기, 이불, 세면용품입니다.
=========inheritance.VIPRoom=========
기본 방의 가격은 100000원 입니다.
방의 포함 물품은 침대, 드라이기, 이불, 세면용품입니다.
```

* TwinRoom, VIPRoom 에 표기된 **extends**는 Room class를 상속한다는 의미이다.

* TwinRoom에는 Room에서 정의한 변수 및 메소드를 사용할 수 있다. (public을 사용하였기 때문에 사용 가능)

* TwinRoom에서는 price를 정의하였기에, 정의한 price를 가져오고,  VIPRoom에서는 부모 클래스에 있는 price를 가져왔다.

  

## 2. super

위와 같은 상황에서 TwinRoom class의 price가 Room price보다 50,000원이 비싸다고 가정해보자. 

Room의 price가 올라간다면, TwinRoom도 코드를 바꿔줘야할 것이다. 따라서 부모의 값을 가져와서 계산하는 방법은 없을까? 정답은 *super* 을 이용하면 된다. 추가로, VIP Room도 100,000이 더 비싸다고 수정해보자

>  super 키워드는 부모 클래스(super class)의 변수, 함수를 호출할 수 있다.

 

```java
public class TwinRoom extends Room {

		public int price = super.price + 50000;
}

public class VIPRoom extends Room {
  
    public int price = super.price + 100000;
}
```

```bash
=========inheritance.TwinRoom=========
기본 방의 가격은 150000원 입니다.
방의 포함 물품은 침대, 드라이기, 이불, 세면용품입니다.
=========inheritance.VIPRoom=========
기본 방의 가격은 200000원 입니다.
방의 포함 물품은 침대, 드라이기, 이불, 세면용품입니다.
```

 TwinRoom은 50,000원, VIP Room은 100,000원 비싸다고 가정하였다. TwinRoom, VIPRoom이 별개로 가격이 올라가면 Room은 영향을 받지 않고,  Room의 가격 상승시(전체적인 가격인상) 일괄 가격 인상이 가능하다.



## 3. 메소드 오버라이딩(Method Overriding)

Room class에서 printWarningPoint() 메소드는 방의 주의사항을 보여주는 함수이다. 다른 상속받는 방의 주의사항을  다르게 보여주려면 어떻게 할까?

> 메소드 오버라이딩(Method Overriding)은 부모 클래스(super class)의 public 메소드로 정의된 함수를 재구현 하는 것이다.

```java
  public void printWarningPoint() {
        System.out.println("기본방의 주의사항은 없습니다.");
    } // Room 주의사항
```



```java
public class TwinRoom extends Room {
    public int price = super.price + 50000;

    @Override
    public void printWarningPoint() {
        System.out.println("유리가 매우 약하니 깨지않게 주의하세요.");
    }
}

public class VIPRoom extends Room {
    public int price = super.price + 100000;

    @Override
    public void printWarningPoint() {
        System.out.println("화장실 타일이 미끄러우니 조심하세요.");
    }
}

import java.util.List;

public class Main {

    public static void main(String[] args) {
        TwinRoom twinRoom = new TwinRoom();
        VIPRoom vipRoom = new VIPRoom();

        twinRoom.printWarningPoint(); // 유리가 매우 약하니 깨지않게 주의하세요.
        vipRoom.printWarningPoint(); // 화장실 타일이 미끄러우니 조심하세요.
    }
}
```



## 4. 메소드 디스패치(Method Dispatch)

### 4.1 메소드 디스패치란? 

어떤 메소드를 호출할지 결정하여 실제로 실행시키는 과정 이다.

### 4.2 메소드 디스패치 종류

- 정적 메소드 디스패치(Static Method Dispatch): **컴파일** 시점에서, 특정 메소드를 호출할 것이라는 걸 명확하게 알고있는 경우
- 동적 메소드 디스패치(Dynamic Method Dispatch): **런타임** 시점에서, 호출되는 메소드가 동적으로 정해지는 경우
- 더블 메소드 디스패치(Double Method Dispatch):**런타임** 시점에서, 정적 또는 동적 메소드 디스패치를 두번 진행하는 경우



### 4.3 정적 메소드 디스패치(Static Method Dispatch)

**컴파일** 시점에서 특정 메소드를 호출할 것이라는걸 명확하게 알고 있는 경우를 의미한다.

> 위에서 작성한 TwinRoom클래스와, VIPRoom은 컴파일 시점에서 어떤 메소드를 사용해야 할지 나타나기에,
> 정적 메소드 디스패치로 볼 수있다.

```java
  TwinRoom twinRoom = new TwinRoom(); // TwinRoom의 메소드를 사용하겠다고 명시
  VIPRoom vipRoom = new VIPRoom(); // VIPRoom의 메소드를 사용하겠다고 명시

  twinRoom.printWarningPoint(); // 정적 메소드 디스패치 발생
	vipRoom.printWarningPoint(); // 정적 메소드 디스패치 발생
```



아래 byteCode에서 #19, #20 을 보면 어떤 클래스의 메소드에서 호출한 것인지 명확하게 나오는것을 확인 할 수 있다. 

```bash
 void printWarningPointTest();
    Code:
       0: new           #2                  // class inheritance/TwinRoom
       3: dup
       4: invokespecial #3                  // Method inheritance/TwinRoom."<init>":()V
       7: astore_1
       8: new           #5                  // class inheritance/VIPRoom
      11: dup
      12: invokespecial #6                  // Method inheritance/VIPRoom."<init>":()V
      15: astore_2
      16: aload_1
      17: invokevirtual #19                 // Method inheritance/TwinRoom.printWarningPoint:()V
      20: aload_2
      21: invokevirtual #20                 // Method inheritance/VIPRoom.printWarningPoint:()V
      24: return
```





### 4.4 동적 메소드 디스패치(Dynamic Method Dispatch)

**런타임** 시점에서, 호출되는 메소드가 동적으로 정해지는 경우를 의미한다.

> ~~위에서 작성한 코드를 **컴파일** 단계에서가 아니라~~, **런타임** 시점에서 호출하는 메소드를 알 수있도록 변경해보자.
> *동적 메소드 디스패치(Dynamic Method Dispatch)* 라고 한다.

```java
  Room twinRoom = new TwinRoom();
  Room vipRoom = new VIPRoom();

  twinRoom.printWarningPoint(); // 동적 메소드 디스패치 발생
	vipRoom.printWarningPoint(); // 동적 메소드 디스패치 발생
```

아래 byteCode에서 #21을 보면 정적 메소드 디스패치와는 다르게, Room으로 지정되어있다.

즉 컴파일 시점에서는 어떤 메소드를 호출할지 알 수 없으며, 런타임 시점의 #3, #6을 호출 해야 어떤 메소드를 호출할지 알 수 있다.

```bash
  void printWarningPointDynamicTest();
    Code:
       0: new           #2                  // class inheritance/TwinRoom
       3: dup
       4: invokespecial #3                  // Method inheritance/TwinRoom."<init>":()V
       7: astore_1
       8: new           #5                  // class inheritance/VIPRoom
      11: dup
      12: invokespecial #6                  // Method inheritance/VIPRoom."<init>":()V
      15: astore_2
      16: aload_1
      17: invokevirtual #21                 // Method inheritance/Room.printWarningPoint:()V
      20: aload_2
      21: invokevirtual #21                 // Method inheritance/Room.printWarningPoint:()V
      24: return

```



### 4.5 더블 메소드 디스패치(Double Method Dispatch)

더블 메소드 디스패치를 알려면 Visitor Pattern을 먼저 인지해야 한다.

> Visitor Pattern: 알고리즘을 객체 구조에서 분리시키는 디자인 패턴이다. OCP 원칙을 적용하는 방법중 하나이다.
> 						 행동의 대상이 되는 객체가 행동을 일으키는 객체를 입력으로 받는 패턴이다.

특정 방을 예약하면 방마다 적립 포인트를 추가한다고 가정하자.



#### 4.5.1 EventVisitor Interface

오버로딩을 이용해, accumulatePoint 의 기능을 구현할 메소드를 정의한다.

```java
public interface EventVisitor {

    void accumulatePoint(TwinRoom twinRoom);

    void accumulatePoint(VIPRoom vipRoom);
}
```

#### 4.5.2 EventVisitorImpl

각 accumulatePoint의 메소드 기능을 구현한다. (각 Room의 타입별로, 기능이 추가되더라도, 다른 Room의 타입에는 영향을 미치지 않는다.)

```java
public class EventVisitorImpl implements EventVisitor {


    @Override
    public void accumulatePoint(TwinRoom twinRoom) {
        System.out.println(twinRoom.price);
        System.out.println(
            twinRoom.getClass().getName() + " 예약 포인트" + (double) twinRoom.price / 100.0 + "지급");
    }

    @Override
    public void accumulatePoint(VIPRoom vipRoom) {
        System.out.println(vipRoom.price);
        System.out.println(
            vipRoom.getClass().getName() + " 예약 포인트" + (double) vipRoom.price / 100.0 + "지급");
    }
}
```



#### 4.5.3 Room, TwinRoom, VIPRoom에 EventVisitor을 매개변수로 받는 메소드 추가 (Visitor Pattern)

EventVisitor을 매개변수로 받는 메소드를 추가하였다. this를 넘겨주면서, EventVisitor에서 정의한 accmulatePoint중 적절한 매개변수를 받는 매서드를 찾아 실행한다. 

> 실질적인 로직을 가지고 있는 것은 eventVisitor 이지만, 적용할 객체를 거쳐 로직을 발생시키는 주체는 TwinRoom, VIPRoom의  eventVisitor.accumulatePoint(this);  이다.

```java
public class Room {

   ... 중략 ...
   
    public void accumulatePoint(EventVisitor eventVisitor) {
        System.out.println("기본방은 포인트가 없습니다.");
    }
}

public class TwinRoom extends Room {
    ... 중략 ...

    @Override
    public void accumulatePoint(EventVisitor eventVisitor) {
        eventVisitor.accumulatePoint(this);
    }
}

public class VIPRoom extends Room {
    ... 중략 ...
    
    @Override
    public void accumulatePoint(EventVisitor eventVisitor) {
        eventVisitor.accumulatePoint(this);
    }
}	
```



#### 4.5.4 실행

```java
@Test
@DisplayName("더블 디스패치 테스트")
void doubleDispatchTest() {
    EventVisitor eventVisitor = new EventVisitorImpl();
    Room twinRoom = new TwinRoom();
    Room vipRoom = new VIPRoom();

    twinRoom.accumulatePoint(eventVisitor);
    vipRoom.accumulatePoint(eventVisitor);

}
```

 

#### 4.5.5 결과

```bash
150000
inheritance.TwinRoom 예약 포인트1500.0지급
200000
inheritance.VIPRoom 예약 포인트2000.0지급
```



#### 4.5.6 bytecode

```bash
void doubleDispatchTest();
    Code:
       0: new           #22                 // class inheritance/EventVisitorImpl
       3: dup
       4: invokespecial #23                 // Method inheritance/EventVisitorImpl."<init>":()V
       7: astore_1
       8: new           #2                  // class inheritance/TwinRoom
      11: dup
      12: invokespecial #3                  // Method inheritance/TwinRoom."<init>":()V
      15: astore_2
      16: new           #5                  // class inheritance/VIPRoom
      19: dup
      20: invokespecial #6                  // Method inheritance/VIPRoom."<init>":()V
      23: astore_3
      24: aload_2
      25: aload_1
      26: invokevirtual #24                 // Method inheritance/Room.accumulatePoint:(Linheritance/EventVisitor;)V
      29: aload_3
      30: aload_1
      31: invokevirtual #24                 // Method inheritance/Room.accumulatePoint:(Linheritance/EventVisitor;)V
      34: return
```



## 5. 추상 클래스(abstract class)

### 5.1 추상 클래스란?

추상 클래스는 메소드를 구현하지 않고 정의(?)만 해두는 클래스이다. (abstract 메소드가 하나라도 있다면 class를 추상 클래스로 지정해야한다.)



### 5.2 특징

* abstract를 지정한 추상 메소드는 접근 제어자로 private를 사용할 수 없다.
* abstract는 메소드는 반드시 자식 클래스 에서 구현해야 한다. (미구현시 자식 클래스도 추상 클래스로 작성해야 한다.)

### 5.3 문법

사용하려면 abstract를 아래와 같이 지정하면 되며, 부모 클래스를 상속 받는 추상 클래스가 아닌 자식 클래스는 반드시 해당 내용을 구현해야 한다.
~~java 8부터 default 메서드 지원등 으로 인해 이왕이면 interface를 쓰자~~

```java
public abstract class Room {

   ... 중략 ...

    protected abstract void abstractPoint();

}

public class VIPRoom extends Room {
   ... 중략 ...

    @Override
    protected void abstractPoint() {
        
    }
}

```



## 6. final 키워드

### 6.1 final?

final 키워드는 엔티티를 한번만 할당한다. 재할당을 시도할시 아래와 같이 컴파일 단계에서 오류가 발생한다. (read-only)

> 단 Collections 과 같이 재할당이 아니라, add, remove와 같은 기능은 동작한다.

<img width="455" alt="스크린샷 2020-12-26 오후 8 35 42" src="https://user-images.githubusercontent.com/37217320/103150720-f03d4080-47b9-11eb-8ec3-9eec3bb66458.png">



###  6.2 사용 구분

* 클래스(class): 클래스의 상속을 제한할때 사용
* 메소드(method): 메소드 오버라이딩을 제한할때 사용
* 변수(variable): 변수에 값 재할당을 막을때 사용







## 7. Object 클래스

###  7.1 Object class?

 Object  클래스는 모든 클래스의 최상위 클래스이다. 모든 클래스는 구현시 Object 클래스를 상속하고 있으므로 Object 클래스의 메소드를 사용 및 재정의 가능하다.

> 단 함부로 재정의 하지는 말자. 다양한 함정이 도사리고 있을 수도 있다. (Effective Java 3/E - 아이템 10 Equals는 일반 규약을 지켜 재정의하라)

### 7.2 Object 메소드

| **Modifier and Type and Method**   | **Description**                                              |
| ---------------------------------- | ------------------------------------------------------------ |
| protected Object clone()           | 객체를 복사해서 리턴한다.                                    |
| boolean equals(Object obj)         | 객체간의 동일여부를 boolean 값으로 리턴한다.                 |
| protected void finalize()          | 지정 객체가 GC 처리되기 전에 호출 된다.                      |
| Class<?> getClass()                | 객체의 runtime class를 리턴한다.                             |
| int hashCode()                     | 객체의 해쉬값을 리턴한다.                                    |
| void notify()                      | wait() 된 스레드를 다시 진행할때 호출한다.                   |
| void notifyAll()                   | wait() 된 모든 스레드를 다시 진행할때 호출한다.              |
| String toString()                  | 객체를 String으로 나타낸다. default는 클래스명@16진수 hashcode로 반환한다. |
| void wait()                        | 해당 싱글 스레드를 일시적으로 중지할 때 호출한다.            |
| void wait(long timeout)            | 매개변수의 시간동안 스레드를 일시적 중단할때 호출한다.       |
| void wait(long timeout, int nanos) | 매개변수의 시간동안 스레드를 일시적 중단할때 호출한다.       |



## Reference

> [[JAVA] Static Method Dispatch, Dynamic Method Dispatch, Double Dispatch](https://defacto-standard.tistory.com/413)
>
> [방문자 패턴 - Visitor pattern](https://thecodinglog.github.io/design/2019/10/29/visitor-pattern.html)
>
> [자바 Object 클래스에 대한 이야기](https://www.holaxprogramming.com/2012/06/30/java-basic-object-class/)