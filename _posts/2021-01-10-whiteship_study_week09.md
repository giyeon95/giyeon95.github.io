---
title: "[#9 백기선님 라이브 스터디] 예외 처리"
date: 2021-01-06 22:05:10 -0900
categories: whiteship
---

# 목표
자바의 예외 처리에 대해 학습하세요.

# 학습 내용
* 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
* 자바가 제공하는 예외 계층 구조
* Exception과 Error의 차이는?
* RuntimeException과 RE가 아닌 것의 차이는?
* 커스텀한 예외 만드는 방법



## 예외 처리란?

프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이다.

예외 처리를 통해 특정 에러가 날때 걸맞는 적절한 처리를 할 수 있도록 프로그램을 구성할 수 있다.



## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

아래와 같이 RuntimeException을 강제로 발생시켰다.

```java
public class ExceptionMain {

    public static void main(String[] args) {
        callRunTimeException();

        System.out.println("after Runtime Exception!");
    }

    public static void callRunTimeException() {
        throw new RuntimeException("runtime exception call");
    }
}

```



```bash
Exception in thread "main" java.lang.RuntimeException: test
	at ExceptionMain.callRunTimeException(ExceptionMain.java:13)
	at ExceptionMain.main(ExceptionMain.java:9)

```

위의 코드는 중간에 RuntimeException이라는 예외를 발생시키므로, "after Runtime Exception" 을 출력하지 않는다.

그렇다면 위의 Exception이 발생하더라도 다음 로직을 실행시키려면 어떻게 해야할까?



### try - catch

try-catch 문은 java에서 예외처리를 할때 사용할수 있는 문법이다.

```java
try {
	// 예외가 발생할 수 있는 구문
} catch(RuntimeException e) {
	// 예외 발생시 예외 처리 구문
} finally {
	// 예외 발생 유무와 관계없이 실행 (최종 처리를 위함)
}
```

즉 위에서 예외를 발생한 구문을 try-catch 블록을 이용해서, 다음과 같이 코드를 변경해보자

```java
public class ExceptionMain {

    public static void main(String[] args) {
        try {
            callRunTimeException();
        } catch (RuntimeException e) {
            System.out.println(e); // 1. java.lang.RuntimeException: runtime exception call
        } finally {
            System.out.println("try-catch out"); // 2. try-catch out
        }

        System.out.println("after Runtime Exception!"); // 3. after Runtime Exception!
    }

    public static void callRunTimeException() {
        throw new RuntimeException("runtime exception call");
    }
}
```



```java
java.lang.RuntimeException: runtime exception call
try-catch out
after Runtime Exception!
```

즉 아래와 같이 try-catch 구문으로 예외 처리를 진행할 수 있다.







## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [Inflearn - 더 자바, Java 8 (백기선님 강의)](https://www.inflearn.com/course/the-java-java8?inst=6fcc1e30)
>
> [자바8 #인터페이스 디폴트 메서드와 정적메서드](https://frontierdev.tistory.com/67)
>
> [정적 메소드는 언제 써야 하는가? ::마이구미](https://mygumi.tistory.com/253)
>
> [[Java] 인터페이스 - 인터페이스의 요소들](https://velog.io/@foeverna/Java-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%9A%94%EC%86%8C%EB%93%A4)

