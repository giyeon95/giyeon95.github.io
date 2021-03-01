---
title: "[WHITESHIP #1] 2. 자바 데이터 타입, 변수 그리고 배열"
date: 2020-11-17 22:05:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[` #Season1 백기선님과 함께하는 자바 온라인 스터디`](https://github.com/whiteship/live-study)

# 목표

자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익힙니다.

# 학습 내용

- 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- 프리미티브 타입과 레퍼런스 타입
- 리터럴
- 변수 선언 및 초기화하는 방법
- 변수의 스코프와 라이프타임
- 타입 변환, 캐스팅 그리고 타입 프로모션
- 1차 및 2차 배열 선언하기
- 타입 추론, var



## 프리미티브 타입(Primitive type)
메모리에 직접 데이터를 넣는 타입

**[종류]**
  - 실수타입
    * byte: 8bit
    * short: 16bit
    * int: 32bit
    * long: 64bit
    * char: 16bit
  - 소수타입
    * float: 32bit
    * double: 64bit

**[특징]**

 * 표현 범위: (2^bit수 -1) ~ (2^bit수-1) -1
 * nonNull (null이 들어갈 수 없음)
 * 산술 연산이 가능
 * JVM의 stack 영역에서 관리되어짐

## 레퍼런스 타입(Reference Type)
메모리에 데이터가 있는 주소값을 넣는 타입(Wrapper)

**[종류]**
 - Class
 - Enumeration
 - Array
 - Interface

**[특징]**
 - nullable
 - Wrapper class로 객체를 사용하기 위해 지원 될 수 있음
 - Heap 영역에서 원시적인 값은 관리 되어짐
 - Generic Type으로 Type Safety 하게 사용 가능 (표현이 맞는지 확실하지 않음..)

##### Byte, Short, Integer, Long, Character, Float, Double 이런 타입은 Wrapper 클래스인데, 어떻게 계산이 될까?
```java
class WrapperClassTest { 
    public static void main(String[] args) {
        Integer boxingNum1 = 15;
        boxingNum1 += 1;
        System.out.println("boxingNum1 = " + boxingNum1);
    }
}
```

```bash
> boxingNum1 = 16
```

 - *autoboxing, unboxing?*
     + primitive data type을 그에 상응하는 wrapper class로 자동 변환 시켜주는 것

 - JDK 1.5 이상부터는 Autoboxing, Unboxing을 지원한다.
   

 *Autoboxing and unboxing are introduced in Java 1.5 to automatically convert the primitive type into boxed primitive( Object or Wrapper class). ... With the introduction of autoboxing and unboxing in Java, this primitive to object conversion happens automatically by Java compiler which makes the code more readable.*

[참고: What is Autoboxing and Unboxing in Java](https://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html#axzz6dxWlCrdt)


## 리터럴(Literal)이란?
 - Java 언어가 실제로 처리하는 데이터
 - 변수의 값이 변하지 않는 데이터(메모리 주소가 바라보는 실제 값)

 **[종류]**
  - 정수형 리터럴
    * 10: 정수 (int)
    * 10L or 10l: 정수 (long)
    * 0b1010: 2진수 
    * 012: 8진수
    * 0x0A: 16진수

 - 실수형 리터럴
    * 3.14: 실수 (double)
    * 3.14f: 실수 (float)
    * 314e -2: 지수를 이용한 표현법 (314 * 10^-2)

 - 논리형 리터럴 
    * true, false
    
 - 문자형 리터럴
    * '', '\u[unicode값]'
    

<img src="/assets/images/whiteship_java_2.1.png">

[참고: 상수(Constant)와 리터럴(literal)이란?](https://mommoo.tistory.com/14)
## 변수 선언 방법

```java
public class Main {
    
    int a; // 0 으로 초기화
    String b; // null

    public static void main(String[] args){
        System.out.println(a); // 0
    }
}
```

## 변수의 스코프와 라이프 타임
[변수 종류]
 - 지역 변수: 선언한 Method 안에서만 사용된다. Method가 끝나면 해당 변수도 역할을 다하고 사라진다.
    * 함수 실행 시간동안만 유지
 - 전역 변수: 클래스에 선언되는 변수를 의미하며 객체 변수와 클래스 변수(static)으로 나뉘어진다.
    * 객체 변수: 객체를 사용하는 동안에는 변수가 유지된다.
    * 클래스 변수: 객체의 생성과 관계없이, 클래스가 로딩되는 시점부터 프로그램이 종료되기 전까지 쭉 유지된다.

## 타입 변환, 캐스팅 그리고 타입 프로모션
 변수나 리터럴 타입을 다른 타입으로 변환하는것을 타입 변환이라 한다.
 타입 변환에는 자동 형변환과, 강제 형변환으로 나뉘어 진다.

###### 자동 형변환(Promotion)
자동 형변환은 작은 메모리 크기의 데이터 타입을 큰 메모리 크기의 데이터 타입으로 변환하는 행위
```java
int a = 100; 
double b = a; // 자동 형변환 int(4) -> double(8)
```

###### 강제 형변환(Casting)
자동 형변환의 조건을 갖추지 못했을때, 형변환을 명시적으로 지정해서 할 수 있는 방법이다.
```java
int intVal = 1;
byte byteVal = intVal; // 컴파일 에러 발생
```
위 코드가 컴파일 에러가 발생하는 이유는, int(4byte)의 크기를 잡고 있는 변수를, byte(1byte)로 변환하려 했기 때문이다.
그러나, 1의 값은 byte의 범위에 포함될 수 있으므로 변환하기 위해 아래와 같이 명시할 수 있다.
```java
int intVal = 1;
byte byteVal = (byte)intVal;
```

단, byte(1byte)의 범위를 넘어서는 int value를 byte에 넣을시 전혀 다른 엉뚱한 값이 나올 수 있으므로 주의가 필요하다.

[참고: 6. Java 데이터 타입 자동 형변환(Promotion)과 강제 형변환(Casting)](https://stage-loving-developers.tistory.com/8)


## 1차 및 2차 배열 선언하기
```java
int[] array = new int[10]; // 1차원 배열 설정
int[][] array = new int[10][10]; // 2차원 배열 선언
```

## 타입 추론, var
타입 추론이 정적 타이핑을 지원하는 언어에서, 타입이 정해지지 않은 변수에 대해서 컴파일러가 변수의 타입을 스스로 찾아낼수 있도록 하는 기능
java에서는 일반변수에 대한 타입추론은 var 문법을 9버전 이상에서 지원하며, *제네릭*과 *람다*에 대한 타입추론을 보통 지원한다.


[참고: 자바 타입 추론에 대한 논의 (Type Inference for Java)](https://m.blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220655685560&proxyReferer=https:%2F%2Fwww.google.com%2F)

[참고: Java Lambda (2) 타입 추론과 함수형 인터페이스](https://futurecreator.github.io/2018/07/20/java-lambda-type-inference-functional-interface/)