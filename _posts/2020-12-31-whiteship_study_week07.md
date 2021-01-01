---
title: "[#7 백기선님 라이브 스터디] 패키지"
date: 2020-12-25 23:57:10 -0900
categories: whiteship
---

# 목표
자바의 패키지에 대해 학습하세요.

# 학습 내용
* package 키워드
* import 키워드
* 클래스패스
* CLASSPATH 환경변수
* -classpath 옵션
* 접근지시자

# package 키워드

## package란?

package란 클래스들의 묶음을 의미한다. 그룹 단위로 묶어놓음으로 클래스를 효율적으로 관리할 수 있게 해준다. (디렉터리 구조)



## 특징

- 소스파일에 패키지 선언시 첫 번째 문장으로 단 한번만 패키지 선언을 허용한다.
-  모든 클래스는 반드시 하나의 패키지에 속해야한다.
- 패키지는 점(.)을 구분자로 계층구조로 구성한다.
- 패키지는 물리적으로 클래스 파일(.class)을 포함하는 하나의 디렉디리이다.

> 패키지 명은 대 소문자를 모두 허용하지만 클래스 명과 쉽게 구분하기 위해 **소문자**로 작성하는 것을 원칙으로 한다.



## 이름 없는 패키지(unnamed package)

위에서 *모든 클래스는 반드시 하나의 패키지에 속해야 한다* 라고 이야기했는데, 아래 코드를 보면 패키지 선언이 안 되어있고, 컴파일 시에도 아무 문제가 없었다.

```java
public class PackageDemo {

    public static void main(String[] args) {
        System.out.println("packageDemo 입니다.");
    }
}
```

이는 java에서 패키지가 선언되어 있지 않으면 자동으로 unnamed package로 구분 된다. (이왕이면 효율적인 관리를 위해 패키지를 지정하자)



# import 키워드

## import란?

소스 코드를 작성할 때 다른 패키지의 클래스를 사용하려면 풀 패키지 명으로 지정해야 한다. 그러나 import 문을 작성하면 풀 패키지 명을 생략할 수 있게 만들어준다.

> 컴파일 단계에서 import 문을 통해 소스파일에 사용된 클래스들의 패키지를 알아낸 다음 모든 클래스 이름 앞에 패키지 명을 붙여 준다.
>
> import문은 프로그램 성능에 전혀 영향을 미치지 않고, 컴파일 시간이  아주 조금 더 걸릴 뿐이다.



java.util 패키지에 포함된 ArrayList를 import없이 풀 패키지명을 작성하면 다음과 같다.

```java
public class PackageDemo {

    java.util.ArrayList<String> list = new java.util.ArrayList<>();
    
}
```

import문을 사용하면 다음과 같이 풀 패키지명을 생략할 수 있다. (컴파일러가 패키지 명을 자동으로 붙여준다)

```java
import java.util.ArrayList;

public class PackageDemo {

    ArrayList<String> list = new ArrayList<>();
    
    public static void main(String[] args) {
       
    }
}

```



## 특징

* 소스코드 작성 시 package -> import -> 클래스 선언의 순서대로 표기한다.
* 패키지명.* 을 작성하면, 해당 패키지의 속한 모든 클래스를 import 한다. (클래스의 이름 대신 *을 사용한다.)
*  java.lang 패키지의 클래스들은 묵시적으로 import 가 선언되어 있다. (ex) java.lang.String)



# classpath(클래스 패스)

## classpath(클래스 패스)란?

JVM이 클래스 파일을 찾기 위한 경로를 의미한다.

## 특징

- classpath는 컴파일 시 지정된 classpath로 경로가 지정된다.
- JVM은 런타임과정에서 classpath를 최우선 순위로 찾는다.
- classpath가 지정되지 않았다면 현재 디렉터리에서 찾는다.



> Intellij IDE 에서 Project Structure을 보면, 프로젝트 생성시 classpath가 지정 되어진다. (Excluded Folders)

![스크린샷 2021-01-01 오후 1 22 13](https://user-images.githubusercontent.com/37217320/103433453-66b3c580-4c34-11eb-8479-0a27bf9628b5.png)

![스크린샷 2021-01-01 오후 2 51 40](https://user-images.githubusercontent.com/37217320/103434177-f3fd1700-4c40-11eb-8999-cadfc9905955.png)

## 클래스 path가 변경된다면?

1. Main.class파일은 컴파일된 파일이고, java 명령어를 이용해서 실행한 모습이다.

<img width="842" alt="스크린샷 2021-01-01 오후 1 04 58" src="https://user-images.githubusercontent.com/37217320/103433302-2b17fc00-4c32-11eb-9528-805dcec0c95e.png">

2. 임의의 디렉터리(tempDir)을 만들고, Main.class 파일을 임시 디렉터리에 넣었다.

![스크린샷 2021-01-01 오후 1 09 26](https://user-images.githubusercontent.com/37217320/103433342-d7f27900-4c32-11eb-8ff9-769b9f096b4c.png)

3. 디렉터리 이동후 동일하게 실행해보면,  NoClassDefFoundError 가 발생함을 볼 수 있다.

![스크린샷 2021-01-01 오후 1 14 27](https://user-images.githubusercontent.com/37217320/103434573-d3d05680-4c46-11eb-80e0-a4eaab0d30fe.png)

> 컴파일 시점에 지정된 classpath로 경로가 지정되었는데, 임의로 빌드된 파일의 경로를 변경한다면 지정된 classpath를 찾을 수 없으니, Exception을 뱉는다.





## CLASSPATH 환경변수

CLASSPATH 환경변수는 OS 환경변수로 지정하는 방법이다.

JVM이 시작될 때 JVM의 Class Loader은 OS에 지정된  CLASSPATH 환경변수를 항상 호출한다. 환경변수에 지정된 클래스들을 먼저 JVM에 적재한디.

> [Class Loader: javac가 컴파일한 byteCode를 Runtime Date Area로 적재하는 역할을 한다.](https://giyeon95.github.io/whiteship/whiteship_study_week01/)



## -classpath(-cp) 옵션

컴파일러가 컴파일 시점에서 참조할 클래스 파일 경로를 사전에 지정해주는 옵션이다.

> 보통 스프링의 경우 gradle이나 maven등 외부 라이브러리를 사용한다. IDE에서 컴파일 할때, 참조할 라이브러리를 클래스 패스로 지정해주고 있다.

한개 이상 경로의 클래스 패스 지정이 필요하다면 window는(;)로 linux, OSX는 (:)로 구분해준다.

```bash
java -classpath ".;lib" ClasspathDemo2 // window
java -classpath ".:lib" ClasspathDemo2 // linux, OSX
```

> 현재 디렉터리와 없다면 하위 디렉터리 중 lib 에서 클래스를 찾는다. 
> ClasspathDemo2 에 필요한 클래스들을 Class Loader로 호출하고,  ClasspathDemo2를 실행한다.



## 클래스 path가 변경되어도 실행시키기

위에서 디렉터리를 이동하여도 classpath를 지정하여, 정상 실행하도록 변경해보자.

![스크린샷 2021-01-01 오후 3 04 59](https://user-images.githubusercontent.com/37217320/103434298-b9947980-4c42-11eb-9c7c-66e8fdc551f6.png)

클래스 패스를 현재 디렉터리 및 상위 디렉터리를 지정함으로서 정상 실행되는 것을 볼 수 있다.



----

---



# 접근 지시자

## 접근 지시자란?

접근 지시자는 멤버 또는 클래스에 사용되어 해당 멤버변수 또는 클래스를 외부에서 접근하지 못하도록 제한하는 역할을 한다.



## 사용 목적

외부에는 불필요한, 내부적으로만 사용되는 부분을 감출 수 있고, 데이터를 외부에서 보호할 수 있다.

> 객체지향개념의 캡슐화(encapsulation)에 해당하는 내용
>
> private 멤버변수는 public 메소드인 getter/ setter을 사용하자



## 종류

- private: 같은 클래스 내에서만 접근 가능
- default: 같은 패키지 내에서만 접근 가능
- protected: 같은 패키지, 다른 패키지의 자식 클래스에서 접근 가능
- public: 접근 제한이 없음

|    지시자     | Class(같은 클래스) | Package(같은 패키지) | extends(자식 클래스) | ETC(그 외) |
| :-----------: | :----------------: | :------------------: | :------------------: | :--------: |
|  **public**   |         O          |          O           |          O           |     O      |
| **protected** |         O          |          O           |          O           |            |
|  **default**  |         O          |          O           |                      |            |
|  **private**  |         O          |                      |                      |            |





## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [[자바컴파일 - javac 명령어/옵션 사용법 및 문제해결 (Feat. 스프링부트)](https://suzxc2468.tistory.com/193)](https://suzxc2468.tistory.com/193)
>
> [클래스 패스](https://opentutorials.org/course/1223/5527)