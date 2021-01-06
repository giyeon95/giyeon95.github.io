---
title: "[#8 백기선님 라이브 스터디] 인터페이스"
date: 2021-01-06 22:05:10 -0900
categories: whiteship
---

# 목표
자바의 인터페이스에 대해 학습하세요.

# 학습 내용
* 인터페이스 정의하는 방법
* 인터페이스 구현하는 방법
* 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
* 인터페이스 상속
* 인터페이스의 기본 메소드 (Default Method), 자바 8
* 인터페이스의 static 메소드, 자바 8
* 인터페이스의 private 메소드, 자바 9

## 

## 인터페이스란?

인터페이스는 일종의 추상클래스이다. 인터페이스는 추상클래스처럼 추상메서드를 갖지만 추상클래스보다 추상화 정도가 높다고 보면 된다.



## 인터페이스 정의하는 방법

MyService라는 Interface를 정의해보자.

```java
public interface MyService {

    int usePort = 1111;

    void initService();
}
```

위 인터페이스는 다음과 같은 특징이 있다.

### 멤버변수

위 MyService에는 usePort 멤버변수를 정의하였다. 사실 인터페이스의 모든 멤버변수는 아래와 같은 특징이 있다.

> 모든 멤버변수는 public static final 이어야 하며, 생략할 수 있다.

```java
int usePort = 1111; 
// public static final usePort = 1111;
```

즉 위 interface에서 정의된 멤버변수를 구현하는 클래스에서는 readOnly다. 



### 메소드

위의 initService라는 메소드 역시 아래와 같은 특징이 있다.

> 모든 메소드는 public abstract이어야하며 이를 생략할 수 있다.

```java
void initService();
// public abstract void initService();
```

즉 interface에서 정의된 메소드를 구현하는 클래스에서는 반드시 정의해야 한다.



또한 멤버변수와 메소드는 동일하게 public외 다른 접근제어자는 사용할 수 없다.



## 인터페이스 구현하는 방법

인터페이스는 구현시 implements 라는 명령어를 사용한다. 또한 일반적인 상속과 다중 구현(상속)이 가능하다.

또한 인터페이스에 정의된 메소드는 *반드시 모두 정의* 해야한다.

```java
public class MyServiceImpl implements MyService {

    @Override
    public void initService() {
        System.out.println(usePort);
    }
}

```







### 테스트

테스트 방법은 다음과 같다. (springBoot환경에서 테스트해서 몇가지 어노테이션이 추가로 존재한다.)

1. reflection을 이용해서, 인터페이스를 구현클래스의 멤버변수와, 메소드의 접근자를 가져온다.
2. 멤버변수는 static, public 이 맞는지 확인한다.
3. 메소드는 public 이 맞는지 확인한다.

```java
package com.example.demospringboot.example;

import static org.junit.jupiter.api.Assertions.assertTrue;

import java.lang.reflect.Modifier;
import java.util.stream.Stream;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class MyServiceImplTest {

    @Autowired
    MyService myService;

    @Test
    @DisplayName("멤버변수 접근자 테스트")
    void variableAccessModifierTest() throws NoSuchFieldException {

        int modifiers = myService.getClass().getField("usePort").getModifiers();

        assertTrue(Modifier.isPublic(modifiers));
        assertTrue(Modifier.isStatic(modifiers));


        Stream.of(myService.getClass().getInterfaces().getClass().getDeclaredFields()).forEach(System.out::println);
    }

    @Test
    @DisplayName("메소드 접근자 테스트")
    void methodAccessModifierTest() throws NoSuchMethodException {
        int modifiers = myService.getClass().getMethod("initService").getModifiers();

        assertTrue(Modifier.isPublic(modifiers));

        Stream.of(myService.getClass().getInterfaces().getClass().getDeclaredFields()).forEach(System.out::println);
    }
}
```



![스크린샷 2021-01-06 오후 11 19 52](https://user-images.githubusercontent.com/37217320/103778630-dcc18d80-5075-11eb-9cc6-b86b49396c7e.png)



## 

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

위 인터페이스는 다음과 같이 정의할 수 있다.

```java
MyInterface myInterface = new MyInterfaceImpl();
```

상속과 동일하게 다형성에 대해 지원하며, 인터페이스 타입의 참조변수로 인터페이스를 구현한 클래스의 인스턴스를 참조 할 수도 있다.

즉 아래처럼 작성할 수 있다.

```java
  @Test
    void polymorphismExample() {
        MyService myService = new MyServiceImpl();
        MyService secondMyService = new MyServiceImpl2();
    }
```

자세한 내용은 라이브 스터디의 6주차 내용에 나와있으며, 6주차 (상속) 에서 학습하였던 메소드 디스패치 또한 동작을 한다.

[백기선님 라이브 스터디 6주차 상속- 메소드 디스패치(Method Dispatch)](https://giyeon95.github.io/whiteship/whiteship_study_week06/#4-메소드-디스패치method-dispatch)





## 인터페이스 상속

인터페이스는 다른 인터페이스를 상속 받을 수 있으며 다중 상속도 가능하다.

 예를들어 아래와 같이 ServiceA, ServiceB 인터페이스를 정의해보자

```java
public interface ServiceA {

    void runServiceA();
}

public interface ServiceB {

  	void runServiceB();
}
```



그리고 위에서 정의한 ServiceA, ServiceB를 다중상속한 CommonService의 정의는 다음과 같다.

```java
public interface CommonService extends ServiceA, ServiceB {

}
```



그리고 위에서 정의한 CommonService를 구현한 CommonServiceImpl을 정의해보았다.

> 이렇게 되면 ServiceA, ServiceB를 구현해야할까? 라는 의구심이 들었는데 아래와 같이 해결되었다.

```java
public class CommonServiceImpl implements CommonService {

    @Override
    public void runServiceA() {

    }

    @Override
    public void runServiceB() {

    }
}
```

위와 같이 CommonService의 부모 인터페이스(?)의 정의된 메소드는 전부 구현을 해야한다. (미구현 시 컴파일 에러가 뜬다.)



즉 CommonService는 명시적으로 이렇게 정의할 수도 있다.

```java
public interface CommonService extends ServiceA, ServiceB {

    @Override
    void runServiceA();

    @Override
    void runServiceB();
}

```



## 인터페이스의 기본 메소드 (Default Method), 자바 8



## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> 