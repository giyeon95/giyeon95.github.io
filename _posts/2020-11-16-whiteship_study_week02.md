---
title: "[#2 백기선님 라이브 스터디] 자바 데이터 타입, 변수 그리고 배열"
date: 2020-11-17 22:05:10 -0900
categories: whiteship
---

백기선님 온라인 라이브 스터디를 기회로, JAVA 기본기를 정리해보자

# 목표
* 자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익히자

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
 
[Ref: What is Autoboxing and Unboxing in Java](https://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html#axzz6dxWlCrdt)


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
    
<img src="/assets/images/whiteship_java_2.1.png"></img>

-- 추가 작성 예정

### References
> [상수(Constant)와 리터럴(literal)이란?](https://mommoo.tistory.com/14)
> 