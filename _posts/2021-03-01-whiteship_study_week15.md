---
title: "[WHITESHIP #1] 15. 람다식"
date: 2021-03-01 15:27:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[`백기선님과 함께하는 자바 온라인 스터디 #Session1`](https://github.com/whiteship/live-study)

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

[REF: 람다식(Lambda Expression)](https://atoz-develop.tistory.com/entry/JAVA-람다식Lambda-Expression)



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

위 코드를 람다식을 사용하면 아래와 같이 한줄로도 사용할 수 있다.

```java
public class LambdaDemo {

    public static void main(String[] args) {

        Thread thread = new Thread(() -> System.out.println(Thread.currentThread().getName()));
        thread.start();
    }

}
```

> Runnable인터페이스는 FunctionalInterface 이다.



## 람다식 특징

```java
// return void
1. (Integer a) -> System.out.println(a);
2. (a) -> { System.out.println(a) };
3. a -> System.out.println(a);

// return value
4. (a, b) -> a + b; // return a + b;
5. (a, b) -> {System.out.println(a+b); return a+b;}
```

1. 매개변수 타입도 표기가 가능하나 일반적으로 언급하지 않는다.
2. 하나의 매개변수 일경우 `()` 생략이 가능하다.
3. 실행문(Body) 가 하나일 경우 `{}` 생략이 가능하다.
4. return type이 있을때, 실행문(Body)가 하나일 경우 return 예약어 생략이 가능하다.



## java.util.function

Functional Interface는 오직 하나의 메소드 선언을 갖는 인터페이스이다.

Java8부터는 다음과 같은 Functional Interface기반의 java.util.function 패키지를 지원한다.

### 1. Predicate<T>

Predicate <T> 인터페이스는 test라는 추상 메소드를 정의하고 test는 제너릭 형식의 T의 객체를 인수로 받아 boolean을 반환 한다.

```java
@FunctionalInterface
interface Predicate<T> {
    boolean test(T t);
}
```

```java
Predicate<String> isTestPredicate = a -> a.equals("test");
boolean isTest = isTestPredicate.test("test"); // true
```

Predicate 인터페이스에는 아래와 같은 default 메소드가 추가로 존재한다.

##### 1.1 and(Predicate<? super T> other)

Predicate<T>를 인수로 받아 기존 Predicate 와 `and` 조건으로 결합된 Predicate를 반환한다.

```java
Predicate<String> predicate = a -> a.startsWith("t");
Predicate<String> predicateAnd = predicate.and(a -> a.endsWith("i"));

boolean check2 = predicateAnd.test("test"); // false
```

##### 1.2 negate()

Predicate의 `부정을` 반환하는 Predicate 반환한다.

```java
Predicate<String> predicate = a -> a.startsWith("t");
Predicate<String> predicateNegate = predicate.negate();

boolean check2 = predicateNegate.test("test"); // false 
```

##### 1.3 or(Predicate<? super T> other)

Predicate<T>를 인수로 받아 기존 Predicate 와 `or` 조건으로 결합 Predicate 반환한다.

```java
Predicate<String> predicate = a -> a.startsWith("t");
Predicate<String> predicateOr = predicate.or(a -> a.endsWith("i"));

boolean check2 = predicateOr.test("test"); // true 
```

##### 1.4 not(Predicate<? super T> target) 

java 11 부터 지원하는 static 메소드이며 인수로 전달된 Predicate의 부정 Predicate를 반환한다.



### 2. Consumer<T>

Consumer<T> 인터페이스는 제너릭 형식의 T 객체를 받아, void를 반환하는 accept 추상메소드를 정의한다.

```java
@FunctionalInterface
public interface Consumer<T> {

    void accept(T t);
 }
```

```java
Consumer<String> consumer = a -> System.out.println(a);
consumer.accept("test!!"); // test!!
```

##### 2.1 andThen(Consumer<? super T> after)

Consumer<T>의 default메소드로서, accept 메소드를 실행하고, 인수로 받은 Consumer<T>의 accept 메소드를 호출하도록 정의한다.

* accept() 메소드를 실행하기 전에는, 아무런 동작이 일어나지 않는다.

  ```java
  Consumer<String> firstConsumer = a -> System.out.println("first: "+ a);
  Consumer<String> secondConsumer = b -> System.out.println("second: "+b);
  Consumer<String> combineConsumer = firstConsumer.andThen(secondConsumer);
  
  combineConsumer.accept("test");
  
  // first: test
  // second: test
  ```



### 3. Function<T, R>

Function<T, R> 인터페이스는 제너릭 형식의 T 객체를 받아, R 객체를 반환하는 apply 추상메소드를 정의한다.

```java
@FunctionalInterface
public interface Function<T, R> {

    R apply(T t);
}
```

```java
Function<Integer, Integer> function = a -> a * 100;
Integer apply = function.apply(3); // 300
```



##### 3.1 andThen(Function<? super R, ? extends V> after)

Function<T>의 default메소드로서, apply 메소드를 실행후 반환 값을 인수로 받은 Function<T>의 apply 메소드의 인수로 전달하고 결과를 반환 한다.

```java
Function<Integer, Integer> function = a -> a * 100;
Function<Integer, Integer> function1 = function.andThen(b -> b / 2);

Integer apply1 = function1.apply(3); // 150
// 1. funciton에 3을 인수로 넣어 300 이 반환된다.
// 2. function에서 반환된 300을 function1의 b에 넣고, 150을 반환한다.
```



##### 3.2 Function<V, R> compose(Function<? super V, ? extends T> before) 

Function<T>의 default 메소드로서, 인수로 받은 Function<T>의 apply 메소드를 먼저 실행 및 반환 후 apply 메소드를 실행하여 결과를 반환 한다.

`andThen 메소드와 반대 순서로 동작한다.`

```java
Function<Integer, Integer> function = a -> a * 100;
Function<Integer, Integer> function1 = function.compose(b -> b / 2);

Integer apply1 = function1.apply(3); // 100
// 1. function1에 3을 인수로 넣어, 1(1.5 이나 Integer이므로) 이 반환된다.
// 2. function1에서 반환된 1을 function의 a에 넣고, 100을 반환한다.
```



### 4. Supplier<T>

Supplier<T> 인터페이스는 매개변수는 없으며 T 객체를 반환하는 get 추상메소드를 정의한다.

```java
@FunctionalInterface
public interface Supplier<T> {

    T get();
}
```

```java
Supplier<String> supplier = () -> "test";
String s = supplier.get(); // test
```



[REF: https://multifrontgarden.tistory.com/125](https://multifrontgarden.tistory.com/125)

## Variable Capture

### Variable Scope

익명클래스는 새로운 scope를 가지기에 아래와 같이 변수를 로컬변수로 재정의 하여 사용할 수 있다.

```java
public class LambdaDemo {

    public static void main(String[] args) {
        int a = 100;

        Consumer<Integer> anonymosClass = new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                int a = 10; 
                System.out.println(integer * a); // 10
            }
        };

        anonymosClass.accept(100);
    }
}
```

그러나 람다식에서는, 변수의 scope가 람다를 감싸고 있는 scope를 공유한다, 로컬변수를 재정의 할 수 없다.

```java
public class LambdaDemo {

    public static void main(String[] args) {
        int a = 100;

        Consumer<Integer> lambdaExpression = (integer) -> {
            int a = 10; // Error. Variable 'a' is already defined in the scope
            System.out.println(integer * a);
        };
        
        lambdaExpression.accept(100);
    }
}
```

위 내용을 유식하게, 람다식에서는 변수 캡처를 `쉐도윙` 하지 않는다고 표현한다.



### final / effective final

람다식에서 scope를 공유하기에, 위 변수의 값을 사용할 수 있으나, 해당 변수는 final 혹은 effective final이어야 한다.

> effective fianl: final이 붙지 않았으나, 변수의 값이 변경되지 않는 것

<img width="888" alt="스크린샷 2021-03-01 오후 5 09 06" src="https://user-images.githubusercontent.com/37217320/109469080-e46b4480-7ab0-11eb-8d9c-57e8470e7217.png">

왜 위와 같이 불변의 값만 lambda식 내에서 사용할 수 있을까?

* 람다식에서 사용되는 외부 지역 변수는 사실 복사본이다.

  > 지역변수는 stack 영역에서 생성되며, 지역변수가 선언된 block이 끝나면 stack에서 제거된다.
  >
  > 메소드 내 지역변수를 참조하는 람다식을 리턴하는 메소드가 있을 경우, 메소드 block이 끝나면 추후에 람다식이 수행될때 참조할 수 없다.

```java
public class LambdaDemo {

    public static void main(String[] args) {
        Consumer<Integer> consumer = getConsumer();
      // getConsumer()의 메소드 block을 나오면서 num은 stack에서 제거된다, 그러나 Consumer 메소드는 원하는 기능으로 정상 동작한다.
        consumer.accept(3);

    }

    public static Consumer<Integer> getConsumer() {
        int num = 100;

        Consumer<Integer> consumer = a -> System.out.println(a * num);
        return consumer;
    }
}
```

* 지역 변수를 관리하는 쓰레드와 람다식이 실행되는 쓰레드가 다를 수 있다.

  >  스택은 각 쓰레드의 고유의 공간이고, 쓰레드끼리 공유되지 않기 때문에 마찬가지로 람다식이 수행될 때 값을 참조할 수 없다.

위와 같은 이유로, 람다식이 어떤 쓰레드에서 수행될지는 미리 알 수 없으며, 지역 변수값이 변하게 되면 가장 최신 값이 복사되어 전달되었는지 확신 할 수 없다는 것이다. 값이 변경된다면, 어떤 값이 최신 복사본인지 판단하기 어렵기에, 최신 값을 보장하기 위해 final / effective final 이어야 한다.



[REF: https://vagabond95.me/posts/lambda-with-final](https://vagabond95.me/posts/lambda-with-final/)

[REF: https://www.inflearn.com/course/the-java-java8](https://www.inflearn.com/course/the-java-java8)



## 메소드, 생성자 레퍼런스

메소드 레퍼런스란 람다식을 더 간단하게 표현할 수 있는 방법이다.

전달하는 인수와 사용하려는 메소드의 인수 형태가 같다면 메소드 레퍼런스를 사용해서 매우 간결하게 표현할 수 있다. 

메소드 레퍼런스의 종류는 다음과 같다.

1. Static Method Reference
2. Instance Method Reference
3. Constructor Method Reference 

### 1. Static Method Reference

> 타입::(Static Method)

```java
Consumer<Integer> consumer = a -> System.out.println(a);
Consumer<Integer> refConsumer = System.out::println;
```



### 2. Instance Method Reference

> (Object Reference)::(Instance Method)

```java
UnaryOperator<String> operator = str -> str.toLowerCase();
UnaryOperator<String> refOperator = String::toLowerCase;
```



### 3. Constructor Method Reference

> 타입::new

```java
UnaryOperator<String> stringOperator = str -> new String(str);
UnaryOperator<String> refStringOperator = String::new;
```



[REF: https://www.inflearn.com/course/the-java-java8](https://www.inflearn.com/course/the-java-java8)

## Reference

> [Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
>
> [[[JAVA8\] 함수형 인터페이스 정리](https://digitalbourgeois.tistory.com/66)](https://digitalbourgeois.tistory.com/66)
>
> [Java 8 Lambda Expression - 람다식 #3](https://tourspace.tistory.com/6)
>
> [Inflearn - 더 자바, Java 8](https://www.inflearn.com/course/the-java-java8)
>
> [[Java] lambda 와 effectively final](https://vagabond95.me/posts/lambda-with-final)

