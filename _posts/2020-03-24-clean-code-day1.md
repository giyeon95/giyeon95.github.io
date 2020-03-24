---
title: "Clean Code 기록하기 Day-1"
date: 2020-03-24 23:02:02 -0400
categories: book
---

깨끗한 코드, 가독성이 좋은 코드는 좋은 개발자의 역량중 한 부분이라 생각합니다.
Clean Code 책을 하루에 얼마나 읽을 수 있을지는 모르겠지만, 책을 읽으며 내용을 정리해 보았습니다.

## 의도를 분명히 밝혀라
> 내가 작성한 코드는 나만 보는것이 아닐 것이다.
> 주석을 달지 않아도 변수, 함수, 클래스명을 나타낼 수 있는 네이밍을 사용하자


## 그릇된 정보를 피해라
> 그릇된 단서는 코드의 의미를 흐린다.
> 유사한 개념은 유사한 표기법을 사용한다.
> 실제로도 자바에서 제공하는 자동완성 기능을 사용할때 각 함수의 주석을 들어가 파악하기 보다는 이름을 보고 파악한다.


## 의미있게 구분하라
> 컴파일러를 통과할 지라도 연속적인 숫자를 덧붙이거나 불용어를 추가하는 방식은 적절하지 않다 ex) a1, a2, a3...

```
public static void copyChars(char[] a2, char[] a2) {
    for(int i = 0; i< a1.length ; i++) {
        a2[i] = a1[i]
    }
}
```
* 개선 하기 전

```
public static void copyChars(char[] source, char[] destination) {
    for(int i = 0; i< source.length ; i++) {
        destination[i] = source[i];
    }
}
```
* 개선 후 (함수 인수 이름 변경)


### 발음하기 쉬운 이름, 검색하기 쉬운 이름을 사용하라

### 인코딩을 피하라

### 자신의 기억력을 자랑하지 마라

## 클래스 이름
> 명사 혹은 명사구가 적합하다. 
> 단, Manager, Processer, Data, Info등과 같은 단어는 피하고, 동사는 사용하지 않는다.

## 메서드 이름
> 동사 혹은 동사구가 적합하다.
> 접근자, 변경자, 조건자는 javabean표준에 따라 get, set, is를 붙이자.
> 생성자를 overload 할때에는 Static Factory Method 패턴을 이용하자.

```
Customer customer = Customer.SignInfo("kiyeon", "male");

Customer customer = new Customer("kiyeon", "male");

```

### 기발한 이름 보다는 명료한 이름을 선택해라.

### 한 개념에 한 단어 (일관성 있는 어휘)를 사용해라

### 말장난을 하지마라
> 같은 add 더라도 매개변수와 반환값이 다르다면 다른 단어를 선택해라

### 해법영역 혹은 문제영역에서 가져온 이름을 사용해라
> 코드를 읽을 사람은 프로그래머이다.
> 적절한 프로그래밍 용어가 없다면 문제영역에서 이름을 선택한다.


-- 추가 작성 예정입니다 --
