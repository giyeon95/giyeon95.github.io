---
title: "[#3 백기선님 라이브 스터디] 자바가 제공하는 다양한 연산자"
date: 2020-11-22 22:45:10 -0900
categories: whiteship
---

# 목표
자바가 제공하는 다양한 연산자를 학습해보자

# 학습 내용
* 산술 연산자
* 비트 연산자
* 관계 연산자
* 논리 연산자
* instanceof
* assignment(=) operator
* 화살표(->) 연산자
* 3항 연산자
* 연산자 우선 순위
* (optional) Java 13. switch 연산자

## 산술 연산자
산술 연산자는 수학에서 사칙연산을 다루는 기본적인 연산자이다.
산술 연산자에 사용되는 종류는 다음과 같다.
* +(더하기), -(빼기), *(곱하기), /(나누기), %(나머지)

```java
int firstNum = 10;
int secoundNum = 3;

System.out.println(firstNum + secoundNum); // 13
System.out.println(firstNum - secoundNum); // 7
System.out.println(firstNum * secoundNum); // 30
System.out.println(firstNum / secoundNum); // 3
System.out.println(firstNum % secoundNum); // 1
```

위 예제에서 보면, 4번째, (firstNum / secoundNum) 을 보면 3으로 출력이 된다.
이는 별도의 [강제 형변환(Casting)](https://giyeon95.github.io/whiteship/whiteship_study_week02/) 을 하지 않는다면, 같은 타입으로 반환되어 진다고 생각된다.

단 다른 형타입으로 연산시, 큰타입에서 작은타입으로 변환한다면, 값이 손실 될 수 있다.

또한 형변환의 기준은 정수형과 실수형을 계산시 실수형으로 표현하는 것으로 보여진다.

```java
class OperatorExample {

    public static void main(String[] args) {
        int a = 10;
        int intNum = 3;
        double doubleNum = 3;

        long b = 10;
        float floatNum = 3f;

        System.out.println("int/int = " + a / intNum);
        System.out.println("int/double = " + a / doubleNum);
        System.out.println("(double)int/int = " + (double) a / intNum);
        System.out.println("long/float = " + b / floatNum);
    }
}
```

```bash
int/int = 3
int/double = 3.3333333333333335
(double)int/int = 3.3333333333333335
long/float = 3.3333333
```

[참고: 4.2.2. Integer Operations](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.2)

## 비트 연산자
비트연산자는 데이터를 비트단위로 계산을 하는 연산자이다. 계산 방법은 다음과 같다.
1. 10진수를 2진수로 변경
2. 2진수를 연산자 수식에 맞게 계산
3. 2진수를 10진수로 변환 및 출력

| 연산자 | 이름 | 표현 |설명 |
|:------:|:----:|:----:|:------:|
| & | AND(논리곱) | a & b | 두비트가 모두 1이면 1 |
| &#124; | OR(논리합) | a &#124; b | 두 비트가 모두 0이 아니면 1|
| ^ |  XOR(배타적논리합) | a^b | 두 비트가 다르면 1, 같으면 0 |
| ~ | NOT(논리부정) | ~a | 대상 비트의 1의 보수 (1이면 0으로, 0이면 1) |
| &#62;&#62; | 우쉬프트 |  a >> n | n 비트만큼 오른쪽 이동 |
| &#60;&#60; | 좌쉬프트 | a << n | n 비트만큼 왼쪽 이동 | 


```java
public class BitOperatorExample {

    public static void main(String[] args) {
        int a = 3;
        int b = 5;

        System.out.println(a & b); // 0000 0011 & 0000 0101 -> 0000 0001
        System.out.println(a | b); // 0000 0011 | 0000 0101 -> 0000 0111
        System.out.println(a ^ b); // 0000 0011 ^ 0000 0101 -> 0000 0110
        System.out.println(~a); // 0000 0011 -> 1111 1100
        System.out.println(a >> 1); // 0000 0011 -> 0000 0001
        System.out.println(a << 1); // 0000 0011 -> 0000 0110

    }
}
```











