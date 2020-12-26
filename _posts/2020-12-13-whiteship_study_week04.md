---
title: "[#4 백기선님 라이브 스터디] 제어문"
date: 2020-12-25 23:57:10 -0900
categories: whiteship
---

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



## 과제 1. live-study 대시 보드를 만드는 코드를 작성하세요.

### 1. GitHub API 발급하기

#### 1.1 github API를 사용하기 위해 필요한 token key 사전 발급

- 발급 방법: github 접속 -> Settings -> Developer settings -> Personal access tokens -> Generate new token
- 권한은 repo만 허용을 하였다. (issue에 접근하기 위함)

<img src="https://user-images.githubusercontent.com/37217320/102365242-ad7daa80-3ffa-11eb-9a30-1a2e9d3641c6.png" alt="git-01" style="zoom: 33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102365671-267d0200-3ffb-11eb-90d1-50d97f421c3a.png" alt="스크린샷 2020-12-16 오후 11 54 03" style="zoom:33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102365956-765bc900-3ffb-11eb-9403-c83742d10746.png" alt="스크린샷 2020-12-16 오후 11 54 06" style="zoom:33%;" />

<img src="https://user-images.githubusercontent.com/37217320/102366452-03068700-3ffc-11eb-98d0-9732a1de01f9.png" alt="스크린샷 2020-12-17 오전 12 06 56" style="zoom:33%;" />



#### 1.2 발급받은 access token이 유효한지 사전 테스트 - curl

<img width="1247" alt="스크린샷 2020-12-17 오전 12 28 48" src="https://user-images.githubusercontent.com/37217320/102369129-f172ae80-3ffe-11eb-981e-1da6110ce786.png" style="zoom: 50%;" >

[참고: github api 사용방법](https://taetaetae.github.io/2017/03/02/github-api/)



https://api.github.com/ API 호출시 어떤 

 API를 호출할 수 있는지 상세하게 나온다. (Hateos restful 성숙도 모델(?)에서 3단계인가 기억이 잘 안난다..)

그중 우리는 특정 레포지토리(online-study)에 관해 알아야 하므로, repo를 우선적으로 조회 해보자

 "repository_url": "https://api.github.com/repos/{owner}/{repo}",



<img width="1247" alt="스크린샷 2020-12-17 오전 12 31 43" src="https://user-images.githubusercontent.com/37217320/102369424-41ea0c00-3fff-11eb-9190-f9ab33a49fcb.png" style="zoom:50%;" >







repository_url을 조회해보니, 관련있을것 같은 issues_url 을 찾았다.

"issues_url": "https://api.github.com/repos/giyeon95/giyeon95.github.io/issues{/number}",

<img src="/Users/giyeon/Library/Application Support/typora-user-images/스크린샷 2020-12-17 오전 12.44.00.png" alt="스크린샷 2020-12-17 오전 12.44.00" style="zoom:50%;" />



~~위에 나온 정보를 잘 조합해서 구현을 하면 되지 않을까 싶다.~~



**[Github 자바라이브러리](https://github-api.kohsuke.org)를 사용하면 편리합니다.**

문제를 잘 읽자.. (위의 과정을 사용하기 쉽게 API단에서 제공해준다.)



### 2. GitHubBuilder 을 이용해 Repository 가져오기

```java
public static GitIssueBoard create(String token, String repoContext) throws IOException {
    return new GitIssueBoard(token, repoContext);
}

private GitIssueBoard(String token, String repoContext) throws IOException {
    repo = GitHubBuilder
        .fromEnvironment().withOAuthToken(token)
        .build().getRepository(repoContext);

    List<GHIssue> assignments = initAssignments(); // 2.
    this.participants = getParticipants(assignments); //3. 
}
```

Github API를 gradle(혹은 maven)에 추가해준 다음, API의 기능을 사용하기 위해 초기 생성해주는 코드이다. token은 위 방식으로 생성한 값을 넣어주었고,

repoContext에는 url 뒤의 context를 넣어주면 된다. ("whiteship/live-study")



### 3. initAssignments()

```java
private List<GHIssue> initAssignments() throws IOException {
    return repo.getIssues(GHIssueState.ALL).stream()
        .filter(issue -> issue.getAssignees() != null)
        .filter(issue -> issue.getAssignees().stream()
            .anyMatch(assignee -> assignee.getLogin().equals("whiteship")))
        .sorted(Comparator.comparing(GHIssue::getNumber))
        .collect(Collectors.toList());
}
```

Repository에 지정한 repository에서 getIssues()를 통해 이슈 목록을 가져왔다.

> Issue에서 assignee 을 할당해줄 수 있는데, 지정하지 않았을 경우 filter을 통해서 스터디 주차 목록에서 제외한다.
>
> assignee의 할당자가 whiteship 이 아닌경우 제외한다.
>
> issue의 지정된 number 순서대로 오름차순으로 정렬해준후 List<GHIssue> 타입으로 리턴

 

### 4. getParticipants(List<GHIssue> assignments)

```java
private List<Participant> getParticipants(List<GHIssue> assignments) throws IOException {

    List<Participant> participants = new ArrayList<>();

    int size = assignments.size();
    for (int week = 0; week < size; week++) {
        GHIssue assignment = assignments.get(week);

        Set<String> ids = getPerformComments(assignment); // 4.

        for (String id : ids) {
            int pos = findListIndex(participants, id); // 5. 

            Participant participant = performAssignment(participants, id, week, size); // 6.

            if (pos == -1) {
                participants.add(participant);
            } else {
                participants.set(pos, participant);
            }
        }
    }
    return participants;
}
```

assignments를 매 회차별로 돌며 getPerformComments() 메서드를 이용해 댓글을 조회한다.





### 5. getPerformComments(GHIssue assignment)

```java
private Set<String> getPerformComments(GHIssue assignment) throws IOException {
    return assignment.getComments().stream()
        .filter(comment -> comment.getBody().contains("https://"))
        .map(comment -> {
            try {
                return comment.getUser().getLogin();
            } catch (IOException e) {
                e.printStackTrace();
            }
            return StringUtils.EMPTY;
        }).filter(StringUtils::isNotEmpty)
        .filter(t -> !t.equals("whiteship"))
        .collect(Collectors.toSet());
}
```

- 과제를 제출한 참가자는, 본인이 공부한 내용 링크를 남겼는지 확인하기 위해 https:// 가 포함되어있는지 한번 확인한다.

- 사용자 아이디를 모아서 Set(중첩 댓글)으로 반환한다. 



### 6. findListIndex(List<Participant> participants, String id)

```java
private int findListIndex(List<Participant> participants, String id) {
    for (int i = 0; i < participants.size(); i++) {
        if (participants.get(i).equals(id)) {
            return i;
        }
    }

    return -1;
}
```

- 다른 회차에 동일한 참가자가, 과제를 제출했을때, 해당 사용자의 index를 반환한다.



### 7. performAssignment(List<Participant> participants, String id, int week, int size)

```java
private Participant performAssignment(List<Participant> participants, String id, int week, int size) {
    return participants.stream()
        .filter(p -> p.getId().equals(id))
        .findFirst()
        .map(p -> p.submitAssignment(week))
        .orElseGet(() -> Participant.create(id, size));
}
```

- filter을 통해, 신규 참가자인지, 기존에 참여했던 참가자인지 분리한다.
- 기존의 참여했던 참가자는 Participant.submitAssignment(week)을 이용해, 해당 week에 참가 표시를 해준다.
- 신규 참가자는 Participant.create(id, size)를 이용해 새로 생성해준다.



### 8. Participant 

```java
@Getter
@EqualsAndHashCode(of = {"id"})
public class Participant {

    private String id;
    private Progress progress;


    private Participant(String id, Progress progress) {
        this.id = id;
        this.progress = progress;
    }

    public static Participant create(String id, int size) {
        return new Participant(id, Progress.create(1, size));
    }

    public Participant submitAssignment(int week) {
        this.progress.submit(week);
        return this;
    }

    public String getProgressString() {
        StringBuilder sb = new StringBuilder(id);
        sb.append("(");
        sb.append(progress.getPercent());
        sb.append(")");
        sb.append(" ");
        sb.append(progress.draw());

        return sb.toString();
    }
}
```

- id는 참가자 id, Progress 클래스는 진척률을 담고있다.



### 9. Progress

```java
class Progress {

    private long submitCount;
    private long term;
    private boolean[] isSubmitEachWeeks;


    private Progress() {

    }

    private Progress(long submitCount, int size) {
        this.submitCount = submitCount;
        term = size;
        isSubmitEachWeeks = new boolean[size];
    }

    public static Progress create(long submitCount, int size) {
        return new Progress(submitCount, size);
    }

    public String getPercent() {
        return String.format("%.2f", ((double) submitCount / (double) term) * 100.0) + "%";
    }

    public String draw() {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < term; i++) {
            if (isSubmitEachWeeks[i]) {
                sb.append("■");
            } else {
                sb.append(" ");
            }
        }
        return sb.toString();
    }

    public Progress submit(int week) {
        isSubmitEachWeeks[week] = true;
        ++submitCount;

        return this;
    }
}
```

- submitCount는 지금까지 제출 합계
-  term은 과제의 총 갯수 (총 주차)
-  isSubmitEachWeeks는 각 주차별 제출 여부를 의미한다.





### 10. 결과

```bash
...중략...
Yo0oN(22.22%)  ■■ ■             
tbnsok40(22.22%)  ■ ■■             
WonYong-Jang(22.22%)  ■■ ■             
yeo311(22.22%)  ■■ ■             
sjhello(11.11%)  ■                
giyeon95(16.67%)  ■■               
addadda15(16.67%)  ■■               
gblee87(27.78%)  ■■■■             
coldhoon(11.11%)  ■                
Sungjun-HQ(27.78%)  ■■■■             
JsKim4(16.67%)  ■■               
ku-kim(22.22%)  ■■■              
sowhat9293(16.67%)  ■■               
memoregoing(27.78%)  ■■■■             
Jason-time(27.78%)  ■■■■            

...중략...
```

열심히 공부해야겠다..

[전체 소스 코드](https://github.com/giyeon95/whiteship-online-study/tree/week04/src/main/java/dashboard01)

## 과제 2. LinkedList를 구현하세요.

### 0. LinkedList란?

LinkedList란 배열의 단점을 보완하기 위해 고안된 자료구조중 하나이다. 

LinkedList는 node로 이루어져 있으며 node는 data와 next(다음 요소를 바라보는 주소값)으로 구성되어 있다.



#### 0.1 배열과의 차이점

| Array                                                        | LinkedList                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 크기 변경 불가 (크기를 처음에 지정함)                        | 크기 변경 가능 (node의 연결로 크기를 처음에 지정하지 않음    |
| 중간에 데이터를 추가하기 복잡함(기존 데이터 이동 및 데이터 삽입) | 중간에 데이터를 추가할때 복잡하지 않음(이전 node가 바라보는 주소를 추가 되는 노드로, 추가되는 노드 주소는 다음 노드로 지정하면 됨) |



#### 0.2 LinkedList의 종류

LinkedList는 다음과 같이 3종류로 구분되어진다.

- 단일 연결 리스트 (Singly Linked List)
- 이중 연결 리스트 (Doubly Linked List)
- 원형 연결 리스트 (Circular Linked List)



##### 0.2.1 단일 연결 리스트

각 노드가 다음 노드에 대해서만 참조하는 가장 단순한 형태의 연결 리스트

일반적으로 Queue를 구현할때 사용됨



##### 0.2.2 이중 연결 리스트

각 노드가 이전 노드, 다음 노드 에 대해서 참조하는 형태의 연결 리스트

삭제가 간편하나, 관리할 참조가 2개이기 때문에 삽입이나, 정렬의 경우 작업량이 더 많아짐



##### 0.2.3 원형 연결 리스트

연결 리스트에서 마지막 요소가 첫번쨰 요소를 참조하는 리스트

스트림 버퍼의 구현에 많이 사용 됨



### 1. LinkedList Interface

```java
public interface LinkedList<T, R> {

    R add(T nodeToAdd);

    R add(T nodeToAdd, int position);

    R remove(int positionToRemove);

    boolean contains(T nodeToCheck);

    void print();

    int size();
}
```

추가, 삭제, 포함여부, 출력, size 의 기능을 구현하려고 정의 하였다.

제너릭 타입을 사용한 이유는 제너릭과 친해지기(?) 위해서 적용해보았다, (추후에 확장성에서도 좋지 않을까 싶다.)



### 2. HeaderNode

```java
public class HeaderNode implements LinkedList<Node, HeaderNode> {

    private Node next;
    private int size;
  
    @Override
    public HeaderNode add(Node nodeToAdd) {
      ...
    }
  
  	@Override
    public HeaderNode add(Node nodeToAdd, int position) {
      ...
    }
  
   	@Override
    public HeaderNode remove(int positionToRemove) {
      ...
    }
  
  	@Override
    public boolean contains(Node nodeToCheck) {
    	...
    }
  	
  	@Override
    public void print() {
      ...	
    }
  
  	@Override
    public int size() {
      ...
    }
}
     
```

HeaderNode는 LinkedList에서 header을 따로 분리(?)하는게 더 좋고 명확할 것 같아 분리 하였다. (추가, 삭제등, HeaderNode를 반환)

[HeaderNode 전체 소스 보기](https://github.com/giyeon95/whiteship-online-study/blob/week04/src/main/java/linkedlist02/HeaderNode.java)

### 3. Node

```java
public class Node {

    private int data;
    private Node next;

    public int getData() {
        return data;
    }

    public Node getNext() {
        return next;
    }

    public void setData(int data) {
        this.data = data;
    }

    public void setNext(Node next) {
        this.next = next;
    }


    public Node(int data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Node(int data) {
        this.data = data;
    }
}
```

Node는 LinkedList에서 담을 데이터를 정의했다.



### 4. ListNodeTest

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

class ListNodeTest {

    HeaderNode list;

    @BeforeEach
    void initListNode() {
        this.list = new HeaderNode();

        list.add(new Node(3))
            .add(new Node(4))
            .add(new Node(5))
            .add(new Node(6));

    }

    @Test
    @DisplayName("노드 초기화")
    void nodePrintTest() {
        list.print();

        assertEquals(list.size(), 4);
    }

    @Test
    @DisplayName("노드 추가")
    void nodeAddToPositionTest() {
        list.add(new Node(2), 0);
        assertTrue(list.contains(new Node(2)));

        list.print();
    }

    @Test
    @DisplayName("노드 삭제")
    void nodeRemoveToPositionTest() {
        list.remove(2);
        assertFalse(list.contains(new Node(2)));

        assertThrows(IndexOutOfBoundsException.class, () -> list.remove(100));

        list.print();
    }

    @Test
    @DisplayName("노드 포함 여부 테스트")
    void nodeContainsTest() {
        assertTrue(list.contains(new Node(6)));
        assertFalse(list.contains(new Node(100)));
    }

}
```

@BeforeEach 어노테이션을 이용해서, 사전에 데이터를 정의해두었다.

각 케이스 별로 노드를 추가 및 삭제 해보며 테스트를 진행하였다.

[전체 소스 코드](https://github.com/giyeon95/whiteship-online-study/tree/week04/src/main/java/linkedlist02)

[이론 참고: Java로 연결 리스트(Linked List) 구현하기](https://freestrokes.tistory.com/84)



## 과제 3. Stack을 구현하세요.

### 1. Stack Interface

```java
import java.util.EmptyStackException;

public interface Stack {

    void push(int data) throws IndexOutOfBoundsException;
    int pop() throws EmptyStackException;
}
```



### 2. StackNode

```java
import java.util.EmptyStackException;

public class StackNode implements Stack {

    int[] dataHolder;
    int size = 10;
    int pos = 0;

    public StackNode() {
        dataHolder = new int[size];
    }

    public StackNode(int size) {
        this.size = size;
        dataHolder = new int[size];
    }

    @Override
    public void push(int data) throws IndexOutOfBoundsException {
        if (size <= pos) {
            throw new IndexOutOfBoundsException("Out Of Stack Capacity Space!");
        }
        dataHolder[pos++] = data;
    }

    @Override
    public int pop() throws EmptyStackException {
        if (pos < 1) {
            throw new EmptyStackException();
        }

        return dataHolder[--pos];
    }
}
```



### 3.  StackNodeTest

```java
import java.util.EmptyStackException;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

class StackNodeTest {

    Stack defaultSizeStack;

    @BeforeEach
    public void initStackNode() {
        defaultSizeStack = new StackNode();
    }

    @Test
    @DisplayName("스택 push Exception 테스트")
    public void stackPushExceptionTest() {
        int defaultSize = 10;
        for (int i = 0; i < defaultSize; i++) {
            defaultSizeStack.push(i);
        }
        Assertions.assertThrows(IndexOutOfBoundsException.class, () -> defaultSizeStack.push(11));
    }

    @Test
    @DisplayName("스택 pop Exception 테스트")
    public void stackPopExceptionTest() {
        Assertions.assertThrows(EmptyStackException.class, () -> defaultSizeStack.pop());
    }

    @Test
    @DisplayName("스택 push / pop 테스트")
    public void stackPushTest() {
        defaultSizeStack.push(3);
        defaultSizeStack.push(4);
        defaultSizeStack.push(5);

        int pop5 = defaultSizeStack.pop();
        int pop4 = defaultSizeStack.pop();
        int pop3 = defaultSizeStack.pop();
        Assertions.assertEquals(pop5, 5);
        Assertions.assertEquals(pop4, 4);
        Assertions.assertEquals(pop3, 3);
    }

}
```

[전체 소스 코드](https://github.com/giyeon95/whiteship-online-study/tree/week04/src/main/java/stack03)



## 과제 4. 앞서 만든 ListNode를 사용해서 Stack을 구현하세요.

### 1. LinkedNodeStack

```java
public class LinkedNodeStack implements Stack {

    HeaderNode linkedNode;
    int pos = 0;

    public LinkedNodeStack() {
        this.linkedNode = new HeaderNode();
    }

    @Override
    public void push(int data) throws IndexOutOfBoundsException {
        linkedNode.add(new Node(data));
        pos++;
    }

    @Override
    public int pop() throws EmptyStackException {
        if (pos < 1) {
            throw new EmptyStackException();
        }

        int targetPos = --pos;
        Node node = linkedNode.get(targetPos);
        linkedNode.remove(targetPos);

        return node.getData();
    }
}
```

과제 3.의 배열을 사용한 스택 구현과 유사하고, 배열대신 직접 구현한 HeaderNode.class를 사용하였다.

>  HeaderNode 안의 remove 메서드는 삭제한 값을 반환하지 않기때문에, get이란 메서드를 새로 만들었다.)

### 2. HeaderNode get() 추가 

```java
@Override
public Node get(int positionToGet) {

    if (positionToGet < 0 || positionToGet >= size) {
        throw new IndexOutOfBoundsException("position out of index");
    }

    Node preNode = next;

    for (int i = 0; i < positionToGet; i++) {
        preNode = preNode.getNext();
    }
    
    return preNode;
}
```

HeaderNode의 인터페이스인 LinkedList에도 get을 정의 해야 한다.



### 3. LinkedNodeStackTest

```java
import java.util.EmptyStackException;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

class LinkedNodeStackTest {

    Stack stack;

    @BeforeEach
    public void initStackNode() {
        stack = new LinkedNodeStack();
    }

    @Test
    @DisplayName("스택 pop Exception 테스트")
    public void stackPopExceptionTest() {
        Assertions.assertThrows(EmptyStackException.class, () -> stack.pop());
    }

    @Test
    @DisplayName("스택 push / pop 테스트")
    public void stackPushTest() {
        stack.push(3);
        stack.push(4);
        stack.push(5);

        int pop5 = stack.pop();
        int pop4 = stack.pop();
        int pop3 = stack.pop();
        Assertions.assertEquals(pop5, 5);
        Assertions.assertEquals(pop4, 4);
        Assertions.assertEquals(pop3, 3);
    }


}
```

### 

[전체 소스 코드](https://github.com/giyeon95/whiteship-online-study/tree/week04/src/main/java/stack)



## 과제 5. Queue를 구현하세요.
###  1. Stack Interface

```java
public interface Queue {

    void push(int data);

    int pop();

}
```



### 2. QueueNode

```java
public class QueueNode implements Queue {

    private final int[] dataHolder;
    private final boolean[] useFlag;
    private int size = 10;
    private int frontPos = 0;
    private int backPos = 0;

    public QueueNode() {
        useFlag = new boolean[size];
        dataHolder = new int[size];
    }

    public QueueNode(int queueSize) {
        this.size = queueSize;
        useFlag = new boolean[size];
        dataHolder = new int[size];
    }


    @Override
    public void push(int data) throws IndexOutOfBoundsException {
        int bp = backPos % size;

        if (useFlag[bp]) {
            throw new IndexOutOfBoundsException();
        }

        useFlag[bp] = true;
        dataHolder[bp] = data;

        backPos = bp + 1;
    }

    @Override
    public int pop() throws QueueEmptyException {
        int fp = frontPos % size;

        if (!useFlag[fp]) {
            throw new QueueEmptyException();
        }

        useFlag[fp] = false;
        int popData = dataHolder[fp];
        frontPos = fp + 1;

        return popData;
    }
}
```

* 배열에 저장하기 때문에, size 크기만큼 순회한다. (bp, sp)
* useFlag 배열은 현재 데이터가 queue에 들어가 있는 데이터인지 확인하는 boolean 값을 넣었다.



### 3. QueueNodeTest

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

class QueueNodeTest {

    Queue queue = new QueueNode();


    @Test
    @DisplayName("push / pop 테스트")
    void pushPopTest() {
        for (int i = 0; i < 10; i++) {
            queue.push(i);
        }

        for(int i = 0 ; i< 10 ; i++) {
            assertEquals(queue.pop(), i);
        }
    }

    @Test
    @DisplayName("push / pop 번갈아가며 테스트")
    void randomPushPopTest() {
        for (int i = 0; i < 5; i++) {
            queue.push(i);
        }

        for(int i = 0 ; i< 5 ; i++) {
            assertEquals(queue.pop(), i);
        }

        for (int i = 0; i < 10; i++) {
            queue.push(i);
        }

        for(int i = 0 ; i< 10 ; i++) {
            assertEquals(queue.pop(), i);
        }

    }
}
```

[전체 소스 코드](https://github.com/giyeon95/whiteship-online-study/tree/week04/src/main/java/)

