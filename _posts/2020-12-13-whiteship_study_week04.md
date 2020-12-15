---
title: "[#4 백기선님 라이브 스터디] 제어문"
date: 2020-12-13 13:48:10 -0900
categories: whiteship
---

이번주는 프로젝트 일정으로 인해 스터디를 참여할 시간이 없었다..
프로젝트가 마무리되고 조금 늦게라도 4주차 스터디에 참여해본다.

# 목표
자바가 제공하는 제어문을 학습해보자.

# 학습 내용
* 선택문
* 반복문
* 과제 (옵션이지만 무조건 참여 해보자.)

## 선택문 (switch)
 - 선택문은 주어진 조건에 따라 분기 처리를 할 수 있도록 일종의 조건문이다.
 - if문(조건문)은 조건식이 많아질수록 else-if를 추가해야하므로 조건식이 많아져 복잡하고, 계산해야 하므로 시간이 오래걸린다. 그러나 switch 문의 조건식은 결과값으로 int형 범위의 정수값을 허용하므로, 하나의 조건식만 계산하면 if문보다 속도가 빠르다. [switch vs if 어떤 때 어는게 효율적인가요?](https://kldp.org/node/62262)
 - 조건식은 연산결과가 int 형 범위의 정수값 이어야 한다. 또한 case문에는 오로지 ~~*[리터럴](https://giyeon95.github.io/whiteship/whiteship_study_week02/)* 이나, *상수* 만을 허용한다.~~ (JDK 1.7 이상부터 String 변수도 허용 된다.)
 - case중간에, break문을 만나게 되면 case문을 빠져나가게 되며, break문이 없다면 모든 문장을 수행하게 된다.
 
 
```java
public class SwitchExample {

    public static void main(String[] args) {
        int n = (int) (Math.random() * 10) + 1;

        System.out.println("n = "+ n);

        switch (n) {
            case 1: {
                System.out.println("나온 숫자는 1 입니다.");
                break;
            }
            case 2: {
                System.out.println("나온 숫자는 2 입니다.");
                break;
            }
            case 3:{
                System.out.println("나온 숫자는 3입니다.");
                break;
            } default: {
                System.out.println("나온숫자는 1 또는 2 또는 3 이 아닙니다.");
            }
        }

    }
}
```


```bash
n = 8
나온숫자는 1 또는 2 또는 3 이 아닙니다.

---

n = 2
나온 숫자는 2 입니다.

---

n = 6
나온숫자는 1 또는 2 또는 3 이 아닙니다.

```

[참고: java의 정석 2nd Edition]

 

## 반복문
* 어떤 작업을 반복적으로 수행되도록 할 떄 사용하는 문법이다.
* for / while / do-while 문으로 구분된다.

*for*
```
for ((1)초기화 ; (2)조건식 ; (4)증감식) {
 (3) 수행될 문장
}
```
(1) 초기화는 처음에만 한번 수행되고, (2)가 true 일때 (3) 실행 (4) 증감식 이렇게 반복된다.

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            System.out.println("i = "+ i + ", i < 3 ?" + (i < 3));
        }
    }
}
```

```bash
i = 0, i < 3 ?true
i = 1, i < 3 ?true
i = 2, i < 3 ?true
```

*foreach(향상된? for문)*
JDK 1.5 이상부터 추가되었으며, for문과 동일하나, 조건식 부분이 다르게 표현된다.
list, 혹은 array를 돌릴때 직관적이나, index값을 사용하려면 별도의 선언이 필요하다.

```java
/** for문 사용*/
public class ForLoopExample {

    public static void main(String[] args) {

        List<String> progressWeeks = List.of("week1", "week2", "week3", "week4");

        for (int i = 0; i < progressWeeks.size(); i++) {
            String progressWeek = progressWeeks.get(i);
            System.out.println(progressWeek);
        }
    }
}
/** foreach 사용 */
public class ForEachLoopExample {

    public static void main(String[] args) {

        List<String> progressWeeks = List.of("week1", "week2", "week3", "week4");

        for (String progressWeek : progressWeeks) {
                   System.out.println(progressWeek);
        }
    }
}
```

*while*
```
while((1)조건식) {
    (2) 수행될 문장
}
```
(1) 조건식이 true일 때, (2) 를 실행 후 다시 (1)이 true인지 반복 과정이 이뤄진다.

```java
public class Main {

    public static void main(String[] args) {
        int i = 0;
        while (i < 3) {
            System.out.println("i = " + i + ", i < 3 ?" + (i < 3));
            i++;
        }
    }
}
```
위에 작성한 *for*과 결과는 동일하다.
while의 조건식을 잘못 작성시(조건식이 계속 true 일때) 무한루프에 빠질 수 있어 주의가 필요하다.


*do-while*
while문의 변형으로 기본적인 구조는 while과 같으나,(1) 수행될 문장 먼저 실행 후 (2) 조건식을 비교한다.
```java
do {
 (1) 수행될 문장
} while((2)조건식);
```

```java
public class Main {

    public static void main(String[] args) {
        
        do {
            System.out.println("do-while 실행입니다.");
        } while (false);
    }
}
```

```bash
do-while 실행입니다. // 조건식은 false 이지만, 한번을 실행된다.
```

do-while 실행입니다.



