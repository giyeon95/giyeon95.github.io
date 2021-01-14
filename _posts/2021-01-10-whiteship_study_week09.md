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



### throw

throw는 아래와 같이 Exception을 강제로 발생시킬때 사용한다.

```java
    public static void callRunTimeException() {
        throw new RuntimeException("runtime exception call"); // 예외 발생
    }
```



### throws

throws는 예외가 발생했을때, 해당 method를 호출한 상위 메소드로 Exception을 던져준다.

```java
  public static void callRunTimeException() throws RuntimeException {
        throw new RuntimeException("runtime exception call");
    }
```

그런데 아래와 위와 같이, throws를 추가하여도 해당 메소드를 호출한 main 메소드에서는 별다른 변화가 없다. 왜 그럴까? (아래 Checked Exception, Unchecked Exception을 보자.)





## 자바가 제공하는 예외 계층 구조

![IMG_22294A635DD9-1](https://user-images.githubusercontent.com/37217320/104128180-5ffe1e80-53a9-11eb-9c12-d0de5cff4cd1.jpeg)

위 그림은 자바가 제공하는 계층 구조를 그림으로 표현한 것이다. Throwable 클래스 역시 Object 클래스를 상속한다. 그리고 Error 클래스와 Exception 클래스로 나뉘어 진다.



## Exception과 Error의 차이는?

Error 클래스와 Exception 클래스에 표기된 javaDoc은 아래와 같다.

**Error**

> An Error is a subclass of Throwable that indicates serious problems that a reasonable application should not try to catch. Most such errors are abnormal conditions. The ThreadDeath error, though a "normal" condition, is also a subclass of Error because most applications should not try to catch it.
> A method is not required to declare in its throws clause any subclasses of Error that might be thrown during the execution of the method but not caught, since these errors are abnormal conditions that should never occur. That is, Error and its subclasses are regarded as unchecked exceptions for the purposes of compile-time checking of exceptions.

**Exception**

>The class Exception and its subclasses are a form of Throwable that indicates conditions that a reasonable application might want to catch.
>The class Exception and any subclasses that are not also subclasses of RuntimeException are checked exceptions. Checked exceptions need to be declared in a method or constructor's throws clause if they can be thrown by the execution of the method or constructor and propagate outside the method or constructor boundary.



Error 클래스는 *try-catch로 잡을 수 없는 심각한 문제를 나타내는 Throwable의 하위 클래스* 이며 분류하자면 **UncheckedException**을 발생시킨다.

Exception 클래스는 *합리적인 응용 프로그램이 포착하려는 조건을 나타내는 Throwable의 하위 클래스* 이며 RuntimeException의 하위 클래스 외에는 **CheckedException** 을, RuntimeException은 **UncheckedException**을 발생시킨다.



즉 Error 클래스는 System에서의 심각한 오류를 야기시킬 수 있는 에러이며, Exception 클래스는 프로그래머가 *예외 처리*를 통하여 적절한 처리를 진행할 수 있는 클래스이다.



## RuntimeException과 RE가 아닌 것의 차이는?

위의 글에서 보면 CheckedException과 UncheckedException이란 말이 자주 등장한다. 이는 RuntimeException과 RuntimeException이 아닌 것의 차이와도 관련이 있다.



RuntimeException을 상속 받는 모든 Exception은 UncheckedException이며, 그 외의 것들은 CheckedException을 발생시킨다.

### Checked Exception vs Unchecked Exception

|           | Checked Exception                               | Unchecked Exception                                      |
| --------- | ----------------------------------------------- | -------------------------------------------------------- |
| 구분      | UncheckedException을 제외한 모든 클래스         | RuntimeException 클래스 / RuntumeException의 자손 클래스 |
| 처리 여부 | 반드시 예외 처리를 해줘야 함(try-catch, throws) | 명시적 처리를 강제하지 않음                              |
| 확인 시점 | Compile Level                                   | Runtime Level                                            |
| 트랜잭션  | Rollback 하지 않음                              | Rollback 진행                                            |



위의 코드를 다시한번 살펴보자

```java
 public static void callRunTimeException() throws RuntimeException {
        throw new RuntimeException("runtime exception call");
    }
```

callRunTimeException 메소드는 RuntimeException을 발생시키며 *명시적인 처리를 강제하지 않기에* try-catch, throws를 사용 하던지, 안하던지 컴파일 단계에서 에러가 나지 않는다.

> throws는 해당 method를 호출한 상위 메소드로 Exception을 던져주는데, RuntimeException은 상위 코드에서도 반드시 예외처리를 해주지 않아도 되기에, 의미가 없지 않나 싶다.



RuntimeException을 CheckedException인 IOException으로 변경하면 어떻게 될까?

![스크린샷 2021-01-11 오전 1 48 02](https://user-images.githubusercontent.com/37217320/104129190-0bf63880-53af-11eb-9b4b-05853d87c26b.png)

![스크린샷 2021-01-11 오전 1 49 49](https://user-images.githubusercontent.com/37217320/104129258-4bbd2000-53af-11eb-8cc3-f10049766cf2.png)

반드시 예외 처리를 해줘야 하기 때문에 컴파일단계에서 에러를 뱉는다. (try-catch로 감싸주거나, throws로 예외를 상위로 던져야 한다.)

> main 문에서 throws 구문을 사용하면 main 메소드를 호출하는 JVM이 예외 처리를 진행한다.



## 커스텀한 예외 만드는 방법

커스텀한 예외를 만드려면 예외로 사용할 클래스를 정의하고 Exception 또는 RuntimeException을 상속해서 만들 수 있다.

### Exception vs RuntimeException

> Exception중 RuntimeException의 *자손 클래스는 UncheckedException*, 그 외에는 *CheckedException* 이라고 확인하였다. 
> 예외를 컴파일 단계에서 처리해야 하는 경우 Exception을 상속하여 CheckedException을 발생시키고, 컴파일 단계에서 예외처리를 할 필요가 없는 경우 또는 트랜잭션 롤백이 필요한 경우 RuntimeException을 상속해 UncheckedException을 발생시키자



### Custom CheckedException

예외 클래스 정의 방법은 다음과 같다.

```java
public class MyCheckedException extends Exception {

}
```

여기에서 사용자에게 보여줄 에러메시지를 정의 및 사용하고 싶다 다음과 같이 message를 매개변수로 받고 super 키워드로 넘겨주면 된다.

```java
public class MyCheckedException extends Exception {

    public MyCheckedException(String message) {
        super(message);
    }
}
```



```java

public class ExceptionMain {

    public static void main(String[] args) throws Exception {

        throw new MyCheckedException("My CheckedException 입니다!");
    }
}
```



```bash
Exception in thread "main" MyCustomException: My CheckedException 입니다!
```



### Custom UncheckedException

예외 클래스 정의 방법은 CheckedException과 유사하나 다른점은 **RuntimeException**을 상속해야 한다는 것이다.

```java
public class MyUncheckedException extends RuntimeException {

}

```



또한 CheckedException과 비교했을때, throws 키워드를 붙이지 않아도 컴파일 단계에서는 에러를 뱉지 않는다.

```java

public class ExceptionMain {

    public static void main(String[] args) {

        throw new MyUncheckedException();
    }
}
```





## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [[자바] 예외처리 (2) - 예외클래스의 구조](https://itmining.tistory.com/9)
