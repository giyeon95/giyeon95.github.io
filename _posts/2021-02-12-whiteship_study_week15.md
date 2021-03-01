---
title: "[#15 백기선님 라이브 스터디] 람다식"
date: 2021-02-11 19:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

# 목표

자바의 람다식에 대해 학습하세요.

# 학습 내용
* 람다식 사용법

* 함수형 인터페이스

* Variable Capture

* 메소드, 생성자 레퍼런스

  

## 람다식이란?

Java8 부터 지원하며, 메소드를 간결한 식(expression)으로 표현한 것이다.

익명 클래스의 메소드가 하나인 경우, Lambda 표현식을 사용해서 메소드 인수 또는 코드로 데이터를 처리할 수 있으며, 메소드를 직접 정의하지 않고 간결한 식(expression)으로 표현할 수 있다.

단 Abstract class로도 익명 클래스를 사용할 수 있는것으로 알고 있는데, 람다식 사용이 불가하였다, 즉 람다식은 `FunctionalInterface의 조건을 충족`해야 람다식을 쓸 수 있다.

> FunctionalInterface: 오직 하나의 메소드 선언을 갖는 인터페이스

[참고: 람다식(Lambda Expression)](https://atoz-develop.tistory.com/entry/JAVA-람다식Lambda-Expression)



## 람다식 사용법

Thread 시작시 Thead 이름을 출력하는 기능을 정의하였다.

```java
public class LambdaDemo {
    
    public static void main(String[] args) {

        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        });

        thread.start();
    }
}
```

위 코드를 람다식을 사용하면 아래와 같이 변경할 수 있다.

```java
public class LambdaDemo {

    public static void main(String[] args) {

        Thread thread = new Thread(() -> System.out.println(Thread.currentThread().getName()));
        thread.start();
    }

}
```

> Runnable인터페이스는 FunctionalInterface 이다.



### 람다식 특징

```java
// return void
1. (Integer a) -> System.out.println(a);
2. (a) -> { System.out.println(a) };
3. a -> System.out.println(a);

// return value
4. (a, b) -> a + b; // return a;
5. (a, b) -> {System.out.println(a+b); return a+b;}
```

1. 매개변수 타입도 표기가 가능하나 일반적으로 언급하지 않는다.
2. 하나의 매개변수 일경우 `()` 생략이 가능하다.
3. 실행문(Body) 가 하나일 경우 {} 생략이 가능하다.
4. return type이 있을때, 실행문(Body)가 하나일 경우 return 예약어 생략이 가능하다.



## 함수형 인터페이스 (Functional Interface)

Functional Interface는 오직 하나의 메소드 선언을 갖는 인터페이스이다.

Java8에서부터 아래와 같은 Functional Interface를 지원해준다.

-- 추가예정 -- 



## Reference

> 