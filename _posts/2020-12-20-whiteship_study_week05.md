---
title: "[WHITESHIP #1] 5. 클래스"
date: 2020-12-20 18:57:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

[`#Season1 백기선님과 함께하는 자바 온라인 스터디`](https://github.com/whiteship/live-study)

# 목표

자바의 Class에 대해 학습하세요.

# 학습 내용
* 클래스 정의하는 방법
* 객체 만드는 방법 (new 키워드 이해하기)
* 메소드 정의하는 방법
* 생성자 정의하는 방법
* this 키워드 이해하기



# 객체와 클래스

##  클래스

클래스란 '객체를 정의해놓은 것' 또는'객체의 설계도 또는 틀'이라고 정의할 수 있다. 클래스는 객체를 정의하는데 사용된다.



## 명명법 (Code Convention)

> Code Convention은 코드가 동작하는데는 영향을 주지 않지만,
> 타 개발자들이 소스 코드를 보았을때 빠르게 이해할수 있는 규칙(?)이므로 지키며 코딩하는 습관을 기르자.

* 클래스 이름은 명사이어야 하며, 여러 단어를 사용할  경우 각 단어의 첫 글자는 대문자로 작성한다.
* 클래스 이름은 간단하고 명시적으로 작성해야한다.
* 완전한 단어를 사용하고 약어는 피한다.



### 정의해보기 (1)

학생을 클래스로 정의 해보자.

- 학생은 이름, 학년, 전화번호 정보를 가지고 있어야 한다.

```java
public class Student {

    private String name;
    private int grade;
    private String phone;
}
```



## 객체

'실제로 존재하는 것'이라는 사전적인 의미를 가지고 있으며 객체는 클래스에 정의된 대로 생성된다.

프로그래밍에서의 객체는 클래스에 정의된 내용대로 메모리에 생성된 것을 뜻한다.



### 인스턴스화(instaniate) / 인스턴스(instance)

클래스로부터 객체를 만드는 과정을 클래스의 *인스턴스화(instaniate)* 라 한다.

클래스로부터 만들어진 객체는 그 클래스의 *인스턴스(instance)* 라 한다.



### 정의해보기 (2)

위에서 정의한 학생 클래스를 인스턴스화 하여 인스턴스를 만들어보자.

```java
public class Main {
  
	public static void main(String[] args) {
		Student kiyeon = new Student(); 
	}
}
```

* kiyeon이라는 이름으로 인스턴스를 생성하였다.

* 클래스를 인스턴스화 할때는 new 연산자를 이용해 객체를 생성한다.

  > new 연산자는 메모리(Heap 영역)에 데이터를 저장할 공간을 할당받고, 그 공간의 참조값(reference value) 을 객체에게 반환하여 인스턴스(객체)를 생성해주는 역할을 담당한다.

## 

## 메소드

메소드는 어떤 작업을 수행하기 위한 명령문의 집합이다. 입력 값을 받아 처리할 수 있고 결과값을 반환할 수도 있으며, 반환하지 않을 수 도 있다.

(하나의 메소드는 한가지 기능만 수행하도록 작성하는 것이 좋다.)



## 명명법 (Code Convention)

* 메소드 이름은 동사이어야 한다.
* 여러 단어를 사용할 경우 첫 단어는 소문자로 시작하고, 그 이후 단어는 대문자를 사용한다.



### 정의해보기 (3)

위에 정의한 학생 클래스에 학년을 출력하는 메소드(printGrade)를 만들어보자.

```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 

  	public void printGrade() {
      	System.out.println(name + "은 " + grade + "학년 입니다.");
    }
}

public class Main {
  
	public static void main(String[] args) {
		Student kiyeon = new Student();
    kiyeon.printGrade(); // null은 0학년 입니다.
	}
}

```



```bash
null은 0학년 입니다.
```



위의 kiyeon이라는 인스턴스에는 name과 grade를 따로 지정하지 않았기에 위와같이 출력된다. Student를 인스턴스화시 이름을 지정하고 싶다면? *생성자* 를 사용하면 된다.



## 생성자(Constructor)

클래스를 인스턴스화 할때 인스턴스 변수를 초기화 해주는 메소드(?)이다 (메소드랑은 조금 다르다.)



### 특징 

- 생성자를 지정하는 클래스와 동일한 이름을 가진다.
- 반환타입이 존재하지 않는다.
- 클래스에는 반드시 생성자가 존재한다. (한개 이상일 수 있다.)
- 인스턴스 생성시 딱 한번 호출 된다. 



생성자의 특징중 *클래스에는 반드시 생성자가 존재한다.* 가 있다. 그러나 아래 구문에는 생성자가 보이지 않는다. 왜 그럴까?

```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 

  	public void printGrade() {
      	System.out.println(name + "은 " + grade + "학년 입니다.");
    }
}
```



클래스에는 별도의 생성자를 지정하지 않으면, 아래와 같은 생성자가 숨겨져 있다.

```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 
  
  	// 생성자 미지정시 숨겨져 있는 생성자
  	public Student() {
      
    }

  	public void printGrade() {
      	System.out.println(name + "은 " + grade + "학년 입니다.");
    }
}
```



> 그러나 생성자를 지정하면 default 생성자가 없어지므로, 주의하도록 하자. (별도의 선언을 추가로 해주면 된다.)



```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 
  
  	public Student(String name, int grade) {
      	this.name = name;
      	this.grade = grade;
    }

  	public void printGrade() {
      	System.out.println(name + "은 " + grade + "학년 입니다.");
    }
}
```



아래와 같이 IDE에서 error을 뱉는다.

<img width="1138" alt="스크린샷 2021-01-01 오후 11 03 29" src="https://user-images.githubusercontent.com/37217320/103440021-92ab6700-4c85-11eb-931a-30640f3c21e5.png">



```java
public class Main {
  
	public static void main(String[] args) {
		Student kiyeon = new Student();
    kiyeon.printGrade("kiyeon", 3); // kiyeon은 3학년 입니다.
	}
}

```



```bash
kiyeon은 3학년 입니다.
```





## this 키워드 이해하기

위에서 생성자로 값을 넣을때 this 키워드를 이용하였다. 그렇다면 this 키워드는 무엇일까?

```java
  	public Student(String name, int grade) {
      	this.name = name;
      	this.grade = grade;
    }
```



### this 키워드란?

this 키워드란 객체 자신을 가리키는 키워드이며, 현재 실행되고 있는 컨텐스트에 속한 객체에 대한 레퍼런스를 말한다.

this 키워드는 다음과 같은 상황에서 사용할 수 있다.



### 멤버변수에 접근

아래 예제에서의 name은 Student의 멤버 변수와, 매개 변수 이름이 같다.

this 키워드가 붙은 name은 해당 인스턴스의 멤버변수에 지정함을 의미한다.

```java
public Student(String name, int grade) {
  	this.name = name;
  	this.grade = grade;
}
```


### this로 다른 생성자 호출

this 키워드를 생성자에서 사용한다면 자기자신의 생성자를 호출 할 수도 있다. 

>  this() 키워드는 생성자의 가장 첫 번째 줄에 위치해야 한다.
>
> 호출한 생성자 자기자신을 호출할 수는 없다.

```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 
  
  	public Student() {
      System.out.println("create Student");
    }
  
  	public Student(String name, int grade) {
      	this();
      	this.name = name;
      	this.grade = grade;
    }

  	public void printGrade() {
      	System.out.println(name + "은 " + grade + "학년 입니다.");
    }
}
```



### this로 자신 반환

아래 예제와 같이 this 키워드를 메소드의 return에 사용할 수 있다. 메소드의 리턴 타입은 클래스의 타입이 되어야 한다.

```java
public class Student {
		private String name;
		private int grade;
  	private String phone; 
  
  	...
  	
  	public Student updateName(String name) {
  			this.name = name;
  			return this;
  	} 
  	
  	...
  	
}
```



## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [[JAVA/자바] new 연산자](http://blog.naver.com/PostView.nhn?blogId=heartflow89&logNo=220955262405)
>
> [[Java] 자바 코딩 규칙 (Java Code Conventions)](https://velog.io/@aidenshin/Java-자바-코딩-규칙-Java-Code-Conventions)
>
> [[[자바(JAVA)\] 생성자란 무엇인가?](https://bbigbros.tistory.com/entry/자바JAVA-생성자란-무엇인가)](https://bbigbros.tistory.com/entry/자바JAVA-생성자란-무엇인가)
>
> [자바의 this 키워드](https://madplay.github.io/post/what-is-the-meaning-of-this-in-java)

