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
 - 조건식은 연산결과가 int 형 범위의 정수값 이어야 한다. 또한 case문에는 오로지 ~~*[리터럴](https://giyeon95.github.io/whiteship/whiteship_study_week02/)* 이나, *상수* 만을 허용한다.~~ (JDK 7 이상부터 String 변수도 허용 된다.)
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
 // TODO 작성 예정
