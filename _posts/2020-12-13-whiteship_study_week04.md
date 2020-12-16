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
do-while 실행입니다.
```


---
## 과제(옵션)을 진행해보자

### 과제 0. JUnit 5 학습하세요.
* 인텔리J, 이클립스, VS Code에서 JUnit 5로 테스트 코드 작성하는 방법에 익숙해 질 것.
* 이미 JUnit 알고 계신분들은 다른 것 아무거나!
* 더 자바, 테스트 강의도 있으니 참고하세요~

### 과제 1. live-study 대시 보드를 만드는 코드를 작성하세요.

> 깃헙 이슈 1번부터 18번까지 댓글을 순회하며 댓글을 남긴 사용자를 체크 할 것.
> 참여율을 계산하세요. 총 18회에 중에 몇 %를 참여했는지 소숫점 두자리가지 보여줄 것.
> Github 자바 라이브러리를 사용하면 편리합니다.
> 깃헙 API를 익명으로 호출하는데 제한이 있기 때문에 본인의 깃헙 프로젝트에 이슈를 만들고 테스트를 하시면 더 자주 테스트할 수 있습니다.

1. **github API를 사용하기 위해 필요한 token key 사전 발급**

- 발급 방법: github 접속 -> Settings -> Developer settings -> Personal access tokens -> Generate new token
- 권한은 repo만 허용을 하였다. (issue에 접근하기 위함)

<img src="https://user-images.githubusercontent.com/37217320/102365242-ad7daa80-3ffa-11eb-9a30-1a2e9d3641c6.png" alt="git-01" style="zoom: 33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102365671-267d0200-3ffb-11eb-90d1-50d97f421c3a.png" alt="스크린샷 2020-12-16 오후 11 54 03" style="zoom:33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102365956-765bc900-3ffb-11eb-9403-c83742d10746.png" alt="스크린샷 2020-12-16 오후 11 54 06" style="zoom:33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102366452-03068700-3ffc-11eb-98d0-9732a1de01f9.png" alt="스크린샷 2020-12-17 오전 12 06 56" style="zoom:33%;" />



2. 발급받은 access token이 유효한지 사전 테스트 - curl

<img width="1247" alt="스크린샷 2020-12-17 오전 12 28 48" src="https://user-images.githubusercontent.com/37217320/102369129-f172ae80-3ffe-11eb-981e-1da6110ce786.png" style="zoom: 50%;" >

[참고: github api 사용방법](https://taetaetae.github.io/2017/03/02/github-api/)



https://api.github.com/ API 호출시 어떤  API를 호출할 수 있는지 상세하게 나온다. (Hateos restful 성숙도 모델(?)에서 3단계인가 기억이 잘 안난다..)

그중 우리는 특정 레포지토리(online-study)에 관해 알아야 하므로, repo를 우선적으로 조회 해보자

 "repository_url": "https://api.github.com/repos/{owner}/{repo}",



<img width="1247" alt="스크린샷 2020-12-17 오전 12 31 43" src="https://user-images.githubusercontent.com/37217320/102369424-41ea0c00-3fff-11eb-9190-f9ab33a49fcb.png" style="zoom:50%;" >







repository_url을 조회해보니, 관련있을것 같은 issues_url 을 찾았다.

"issues_url": "https://api.github.com/repos/giyeon95/giyeon95.github.io/issues{/number}",

<img src="/Users/giyeon/Library/Application Support/typora-user-images/스크린샷 2020-12-17 오전 12.44.00.png" alt="스크린샷 2020-12-17 오전 12.44.00" style="zoom:50%;" />



위에 나온 정보를 잘 조합해서 구현을 하면 되지 않을까 싶다.



-- 추가구현 필요 --









### 과제 2. LinkedList를 구현하세요.

* LinkedList에 대해 공부하세요.
* 정수를 저장하는 ListNode 클래스를 구현하세요.
* ListNode add(ListNode head, ListNode nodeToAdd, int position)를 구현하세요.
* ListNode remove(ListNode head, int positionToRemove)를 구현하세요.
* boolean contains(ListNode head, ListNode nodeTocheck)를 구현하세요.

### 과제 3. Stack을 구현하세요.
* int 배열을 사용해서 정수를 저장하는 Stack을 구현하세요.
* void push(int data)를 구현하세요.
* int pop()을 구현하세요.

### 과제 4. 앞서 만든 ListNode를 사용해서 Stack을 구현하세요.
* ListNode head를 가지고 있는 ListNodeStack 클래스를 구현하세요.
* void push(int data)를 구현하세요.
* int pop()을 구현하세요.

### 과제 5. Queue를 구현하세요.
배열을 사용해서 한번
ListNode를 사용해서 한번.
