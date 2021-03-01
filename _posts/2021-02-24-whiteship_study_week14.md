---
title: "[WHITESHIP #1] 14. 제네릭"
date: 2021-02-24 17:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[`#Season1 백기선님과 함께하는 자바 온라인 스터디`](https://github.com/whiteship/live-study)



# 목표

자바의 제네릭에 대해 학습하세요.

# 학습 내용
* 제네릭 사용법
* 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
* 제네릭 메소드 만들기
* Erasure



## 제네릭이란?

Java 1.5에서 부터 추가되었으며, 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 `컴파일 시의 타입체크 (compile-time type check)`를 해주는 기능이다.

객체 타입을 컴파일 시점에서 체크하기에, Type 안정성을 높이고, 형변환의 번거로움이 줄어둔다.



## 제네릭의 특징

* Compile 시점 타입체크를 진행하므로 Runtime 시점에서 타입 안정성을 제공해준다.
* 타입 체크 및 형변환 코드를 생략할 수 있으므로 코드가 간결해진다.



## 제네릭 사용법

### 제네릭 정의

제네릭에서는 참조형 타입(reference type)을 의미하는 기호로 `T`를 사용한다. T 외에도 다른기호도 사용이 가능하며, 여러 타입 변수가 필요하면 `<K, V>` 와 같이 쉼표(,)로 구분하여 명시한다.

```java
public class GenericExample<T> {
    T element;
    
    void setElement(T element) {
        this.element = element;
    }
    
    T getElement() {
        return element;
    }

}
```



### 제네릭 명명 규칙

제네릭의 참조형 타입은 어떠한 기호도 가능하나, 아래 명명 규칙을 따르도록 하자

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types

[REF: https://docs.oracle.com/javase/tutorial/java/generics/types.html](https://docs.oracle.com/javase/tutorial/java/generics/types.html)



### 제네릭 사용하기

제네릭 클래스를 생성시 `<T>`에 객체 타입을 지정하여 사용한다.

```java
public class GenericMain {

    public static void main(String[] args) {
        GenericExample<String> stringElement = new GenericExample<>();
        stringElement.setElement("String Element");
        GenericExample<Integer> integerElenemt = new GenericExample<>();
        integerElenemt.setElement(100);

        System.out.println(stringElement.getElement()); // String Element
        System.out.println(integerElenemt.getElement()); // 100

    }
}
```



### 제네릭의 바이트코드

제네릭이 컴파일 시점에서 type check를 통해 Runtime 시점에서는 타입 안정성을 제공해준다고 하였다. 

즉 컴파일 시점에서 type check 후 byte-code에는 제네릭 객체를 Object 타입으로 사용하는 것을 확인할 수 있었다. 

아래는 위 코드의 byte-code이다.

![image](https://user-images.githubusercontent.com/37217320/109502030-cc5cea80-7adb-11eb-9c22-5211d5519456.png)



## 제네릭 주요 개념 (바운디드 타입, 와일드 카드)

### 바운디드 타입 매개변수 (Bounded Type Parameters)

매개변수화 된 타입에 타입인자를 넣을때 `제한`을 걸고싶을때 `Bounded Type Parameters`을 사용한다.

> 아래는 Number 또는 Number의 하위 타입만 사용할 수 있다. 

```java
public class GenericExample<T extends Number> {}
```

![스크린샷 2021-03-01 오후 10 28 19](https://user-images.githubusercontent.com/37217320/109503313-7426e800-7add-11eb-954f-dfc1ab1a8c73.png)



### 와일드 카드 매개변수 (Wild Card Parameters)

와일드 카드는 대게 아스테리스크(*) 또는 물음표(?)를 사용한다. Java에서는 ?를 사용한다. 

WildCard에는 아래와 같이 3가지 종류가 있다.



#### 1. Unbounded WildCard

List<?>와 같은 타입을 언바운디드 와일드 타입이라한다. 위의 타입은 어떤 타입이 오든 관계가 없을때 주로 사용한다.

```java
private static void printList(List<?> list) {
    list.forEach(System.out::println);
}
```



#### 2. Upper Bounded WildCard

Java에서 매개변수화 타입은 무공변(invariant)이기 때문에, List<Number>은 List<Integer>의 하위타입이거나, 상위 타입이 아니다. 

> 무공변(invariant): 제네릭 타입에서 인스턴스화 할때 다른 인자가 들어가는 경우 인스턴스 간의 하위 및 상위 관계가 성립하지 않는 것 

즉 아래와 같은 코드에서, Integer은 Number의 하위 타입이라 가능할 것처럼 보이나 무공변 이기에 컴파일 에러를 발생시킨다. 

```java
public class GenericMain {

    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        printList(list);

    }

    private static void printList(List<Number> list) {
        list.forEach(System.out::println);
    }
}

```

![스크린샷 2021-03-01 오후 11 31 00](https://user-images.githubusercontent.com/37217320/109511248-324e6f80-7ae6-11eb-85f2-db5f54c47c74.png)

이와 같은 문제를 해결하기 위해 와일드 카드 타입을 사용한다. (다른 말로는 공변(convariant) 라고도 한다.)

아래와 같이 `<? extends Number>`를 사용하면, Number 또는 Number의 하위 타입을 제너릭 타입으로 가진 List를 매개변수로 받을 수 있다. 

> **공변(convariant)**:*구체적인 방향으로 타입 변환을 허용하는 것 (자기 자신과 자식 객체만 허용) <? extends T>*

```java
    private static void printList(List<? extends Number> list) {
        list.forEach(System.out::println);
    }
```



#### 3. Lower Bounded WildCard

Upper Bounded WildCard와는 다르게 extends 대신에 super 키워드를 사용하며 상위 타입으로의 타입 변환을 허용하는 것을 이야기한다.

즉 아래코드에서는 Integer와, Integer의 상위 타입 만 허용한다.

> **반공변 (contravariant)** *: 추상적인 방향으로의 타입 변환을 허용하는 것(자기 자신과 부모 객체만 허용) <? super T>*

```java
  private static void printList(List<? super Integer> list) {
        list.forEach(System.out::println);
    }
```



[REF: [ Java] Java의 Generics](https://medium.com/@joongwon/java-java의-generics-604b562530b3)



## 제네릭 메소드

매개타입과 리턴타입으로 타입 파라미터를 갖는 메소드를 제네릭 메소드라 한다.

사용 방법은 다음과 같다.

```java
public class Box<T> {
    T t;

    public void setT(T t) {
        this.t = t;
    }

    public T getT() {
        return t;
    }
}

public class Util {

    public static <T> Box<T>  boxing(T t) {
        Box<T> box = new Box<>();
        box.setT(t);
        return box;
    }
}
```

위 box 메소드를 호출하는 방법은 두가지가 있다.

```java
Util.<Integer>boxing(100); // 구체적 타입 명시
Util.boxing("암묵적 호출"); // 암묵적 호출
```

[REF: [Java] 제네릭 메서드(Generic Method)](https://limkydev.tistory.com/59)



## Erasure

위에서 잠깐 제네릭의 바이트코드를 보면 Object로 매핑이 되어있는 것을 볼 수 있다. 컴파일러가 정하는 타입으로 대체하는 Type Erasure을 실행하게 되며, 그 결과로 Object로 대체되었다고 볼 수 있다.

> 아래 메소드는 타입 파라미터가 바인드 되지 않았기에 Ljava/lang/Object로 대체가 되었으며, 타입 파라미터가 바인드 된 경우에는 바인드 된 타입 파라미터로 대체된다.

![image](https://user-images.githubusercontent.com/37217320/109502030-cc5cea80-7adb-11eb-9c22-5211d5519456.png)



--- Erasure 추가 예정 ---



## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [[Java] 제네릭 메서드(Generic Method)](https://limkydev.tistory.com/59)
>
> [자바 제네릭스(6) Java Generics: 제한된 타입 매개변수 (Bounded Type Parameters)](https://durtchrt.github.io/blog/java/generics/6/)
>
> [[ Java] Java의 Generics](https://medium.com/@joongwon/java-java의-generics-604b562530b3)
>
> [[kotlin] 제너릭 변성(variance) 정리](https://namget.tistory.com/entry/kotlin-제너릭타입-정리)