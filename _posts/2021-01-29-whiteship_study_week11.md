---
title: "[WHITESHIP #1] 11. Enum"
date: 2021-01-29 20:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[`#Season1 백기선님과 함께하는 자바 온라인 스터디`](https://github.com/whiteship/live-study)

# 목표

자바의 열거형에 대해 학습하세요.

# 학습 내용
* enum 정의하는 방법

* enum이 제공하는 메소드 (values()와 valueOf())

* java.lang.Enum

* EnumSet

  



## enum

관련 상수를 모아 사용할 수 있는 상수들의 그룹을 표현할 수 있다. 

java 에서는 JDK 1.5 이상부터 지원하며, C/C++과는 다르게 변수, 메소드 그리고 생성자를 추가할 수 있다.



## enum 정의하는 방법

아래와 같이 Role을 정의하고, 다음과 같이 사용할 수 있다.

> Java Naming Convention에 따르면, 대문자 및 상수로 정의하는 것을 권장한다. [9 - Naming Conventions](https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html)

```java
public enum Role {
    ADMIN, GENERAL
}

public class EnumMain {

    public static void main(String[] args) {
        Role role = Role.ADMIN;
        System.out.println(role); // java.lang.Enum에서 toString() 메서드가 오버라이드 되어짐
    }
}
```



## enum의 특징

* enum 자체는 클래스이며, 인스턴스 통제된다. (Factory Method & Singleton을 이용해 인스턴스가 하나만 만들어짐을 보장한다.)
* enum은 java.lang.Enum 클래스에 의해 상속된다. (타 클래스를 상속할 수 없다.)
* toString() 메소드는 java.lang.Enum 클래스에서 오버라이드 되어진다.
* 지원 메소드에는 java.lang.Enum 클래스의 static 메소드인 values(), valueof() 와, static 메소드가 아닌, name(), ordinal(), compareTo(), equals() 를 지원한다.



## enum이 제공하는 메소드

### 1. values()

enum에 정의된 열거타입을 배열로 반환해 준다.

```java
@Test
void printValues() {
   Role[] values = Role.values();
  
    Stream.of(values)
        .forEach(System.out::println);
}
```

```bash
ADMIN
GENERAL
```



### 2. valueOf()

enum에서 이름을 매개변수로 받고, 열거 타입을 반환한다. 또한 정의되어 있는 열거 타입을 찾을 수 없다면, IllegalArgumentException을 반환한다.

> Role.valueOf("ADMIN") 은 Role.ADMIN과 같다.

```java
    @Test
    void isEqualValueOf() {
        assertEquals(Role.valueOf("ADMIN"), Role.ADMIN);
        assertThrows(IllegalArgumentException.class, () -> Role.valueOf("EMPTY"));
    }
```



### 3. name()

enum에 정의된 열거 타입 이름을 반환해준다.

```java
    @Test
    void isEqualNameString() {
        Role adminRole = Role.ADMIN;
        String name = adminRole.name();

        assertEquals(name, "ADMIN");
    }
```



### 4. ordinal()

enum에 정의된 열거 타압의 순서를 정수 값으로 리턴한다.

```java
   @Test
    void isEqualsOrdinal() {
        Role adminRole = Role.ADMIN;
        int adminOrdinal = adminRole.ordinal();
        
        assertEquals(adminOrdinal, 0);
    }
```

<img width="1243" alt="스크린샷 2021-01-30 오후 2 40 09" src="https://user-images.githubusercontent.com/37217320/106348360-0edaae00-6309-11eb-81c6-7f4b1af5eaaa.png">





만약 enum 클래스 내의 순서를 변경하면 위 테스트는 어떻게 동작할까?

```java
public enum Role { 
    ADMIN, GENERAL; // AS-IS
}

public enum Role {
    GENERAL, ADMIN; // TO-BE
} 
```

<img width="1242" alt="스크린샷 2021-01-30 오후 2 39 08" src="https://user-images.githubusercontent.com/37217320/106348341-e9e63b00-6308-11eb-8e70-2dff80023ea7.png">

enum 클래스 내부 열거 형들의 순서가 바뀜에 따라, 앞서 작성된 테스트 코드가 실패함을 볼 수 있다.

따라서 ordinal 메소드를 사용하는 것은 가능하면 지향하자. (아래 Effective Java에서 예제를 자세히 설명해준다.)

[Effective Java 3/E Item 35. ordinal 메서드 대신 인스턴스 필드를 사용하라]



## EnumSet

EnumSet은 AbstractSet을 상속받고 있는 추상 클래스이다.

EnumSet은 다른 컬렉션들과 다르게,  new 연산자를 사용하지 않고(abstract 키워드가 사용됨), 정적 팩토리 메소드 (static factory method)로 구현 객체를 반환받을 수 있다.

```java
    @Test
    void enumSetEmptyTest() {
        EnumSet<Role> noneOfEnums = EnumSet.noneOf(Role.class);
        EnumSet<Role> allOfEnums = EnumSet.allOf(Role.class);

        Stream.of(noneOfEnums).forEach(System.out::println);
        Stream.of(allOfEnums).forEach(System.out::println);
    }
```



<img width="1245" alt="스크린샷 2021-01-30 오후 3 06 48" src="https://user-images.githubusercontent.com/37217320/106348850-c7eeb780-630c-11eb-9866-932074141d7d.png">



## Reference

> [자바의 enum](https://velog.io/@pop8682/Enum-27k067ns4a)
>
> [EnumSet을 사용하는 방법](https://siyoon210.tistory.com/152)
>
> [이펙티브 자바 Effective Java 3/E](http://www.yes24.com/Product/Goods/65551284)

