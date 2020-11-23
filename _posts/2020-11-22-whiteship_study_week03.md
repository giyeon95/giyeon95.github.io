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
| &#62;&#62; | 우쉬프트 |  a >> n | n 비트만큼 오른쪽 이동 (부호를 유지) |
| &#60;&#60; | 좌쉬프트 | a << n | n 비트만큼 왼쪽 이동 (0으로 채움)|


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

## 관계 연산자
관계 연산자는 프로그램을 구현하면서 가장 흔히 볼 수 있는 연산자로, boolean 자료형으로 반환되는 연산자 이다.

| 연산자 | 표현 | 설명 |
|:-----:|:-----:|:-----:|
| &#62; | a > b | a가 b보다 크면 true, 작거나 같으면 false를 반환 |
| < | a < b | a가 b보다 작으면 true, 크거나 같으면 false를 반환 |
| >= | a >= b | a가 b보다 크거나 같으면 ture, 작으면 false를 반환 |
| <= | a <= b | a가 b보다 작거나 같으면 true, 크면 false를 반환 |
| == | a == b | a가 b와 같으면 ture, 다르면 false를 반환 |
| != | a != b | a가 b와 다르면 ture, 같으면 false를 반환 |

* *==* , *!=* 연산자는, Reference Type 에서도 사용이 가능한데, 해당 연산을 사용하면 주소값을 비교하기 때문에, 객체가 다르면 false가 반환된다.
* 따라서 .equals() 함수를 쓰면 값을 비교할 수 있다.
* Integer, Long과 같은 타입은 신기하게도 '같음'이 출력되어, 자동으로 변환해주는 것 같다. Intellij 에서는 '=='을 사용했을때 다음과 같은 경고문구는 출력되었다.



```java
public class Operator {

    public static void main(String[] args) {
        Integer a = 3;
        Integer b = 3;

        if(a == b) { // Number objects are compared using '==', not 'equals()' 
            System.out.println("같음");
        } else {
            System.out.println("다름");
        }

    }
}

```

```bash
같음
```

## 논리 연산자
논리 연산자또한 관계 연산자와 같이 자주 사용되며, 역시 boolean 자료형으로 반환된다.
논리연산자는 boolean 타입을 가지고 비교하는것이 특징이다. (a, b는 boolean)

| 연산자 | 표현 | 설명 |
|:-----:|:-----:|:-----:| 
| && | a && b | a와 b 둘다 true라면 true 그 외에는 false 반환 |
| &#124;&#124; | a &#124;&#124; b | a 또는 b 중 하나라도 true라면 true, 두개다 false일때 false 반환 |
| ! |  !a | a가 true이면 false, false이면 true 반환 |

```java
public class LogicalOperator {

    public static void main(String[] args) {
        boolean a = true;
        boolean b = false;

        System.out.println(a && b);
        System.out.println(a || b);
        System.out.println(!a);

    }
}
```

```bash
false
true
false
```

## instanceof
instanceof 연산자란 참조변수가 참조하고 있는 인스턴스의 실제 타입을 확인하여 boolean 자료형으로 반환 해준다.
확인하려는 인스턴스의 클래스가 특정 클래스를 상속, 구현 하고 있다면 역시 true를 반환한다.

```java

public class Player {

}

public interface Charactor {
    void attack();
}

public class Magician extends Player implements Charactor {

    @Override
    public void attack() {
        System.out.println("magic attack!");
    }
}

public class Warrior extends Player implements Charactor{

    @Override
    public void attack() {
        System.out.println("sward attack!");
    }
}

```
위와 같이 클래스 및 인터페이스가 정의되어있다고 가정해보자.

```java
class Main {

    public static void main(String[] args) {


        Object charactor = new Magician();

        System.out.println(charactor instanceof Object); // 1.
        System.out.println(charactor instanceof Charactor); // 2.
        System.out.println(charactor instanceof Magician); // 3.
        System.out.println(charactor instanceof Warrior); // 4.
        System.out.println(charactor instanceof Player); // 5.

    }
}
```

```bash
true
true
true
false
true
```
1. Object는 모든 클래스의 최상위 클래스 이기에 true를 반환한다.
2. Charactor 클래스는 Magician 클래스를 구현 하고 있기에, true를 반환한다.
3. Magion 클래스는 Magician 으로 초기화 하였기에, true를 반환한다.
4. Warrior 클래스는 Magician 클래스와 상속, 구현은 동일하나 실질적으로 다른 클래스 이기에 false를 반환한다.
5. Player 클래스는 Magician의 부모 클래스 이기에 true를 반환한다.


## assignment(=) operator


## 화살표(->) 연산자


## 3항 연산자


## 연산자 우선 순위


## (optional) Java 13. switch 연산자










