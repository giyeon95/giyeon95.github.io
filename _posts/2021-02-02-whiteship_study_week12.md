---
title: "[#12 백기선님 라이브 스터디] 애노테이션"
date: 2021-01-29 20:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

# 목표

자바의 애노테이션에 대해 학습하세요.

# 학습 내용
* 애노테이션 정의하는 방법

* [@retention](https://github.com/retention)

* [@target](https://github.com/target)

* [@documented](https://github.com/documented)

* 애노테이션 프로세서

  



## 애노테이션(Annotation)이란

애노테이션은 JDK 5부터 Java에서 지원하는 기능이다.  사전상의 의미는 주석이지만, 그 이상의 역할을 할수 있다.



## Java에서 지원하는 애노테이션

### 1. @Override

- 해당 메소드가 오버라이드 함을 보여준다.
- 부모 클래스 & 구현하는 대상 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 에러를 발생시킵니다.
- 단 주석의 역할을 하기에, Override를 한 메소드에 @Override 에노테이션을 제외하여도 컴파일 에러는 발생하지 않습니다.



<img width="909" alt="스크린샷 2021-02-02 오전 12 06 40" src="https://user-images.githubusercontent.com/37217320/106476565-887dc200-64ea-11eb-89d0-270cd746b96c.png">



<img width="901" alt="스크린샷 2021-02-02 오전 12 06 05" src="https://user-images.githubusercontent.com/37217320/106476481-7439c500-64ea-11eb-912f-54ac32e58a25.png">

 

<img width="394" alt="스크린샷 2021-02-02 오전 12 07 01" src="https://user-images.githubusercontent.com/37217320/106476612-96cbde00-64ea-11eb-9f97-6fe9bbfb34e7.png">

### 2. @Depercated

* 메서드가 더 이상 사용되지 않음을 표시합니다.
* 컴파일에는 문제는 없으나, 컴파일 경고(Warning)을 보여줍니다.



<img width="935" alt="스크린샷 2021-02-02 오전 12 12 16" src="https://user-images.githubusercontent.com/37217320/106477264-515be080-64eb-11eb-8d42-d94530f7f45e.png">



<img width="902" alt="스크린샷 2021-02-02 오전 12 11 29" src="https://user-images.githubusercontent.com/37217320/106477199-3db07a00-64eb-11eb-8310-46fd75742b79.png">



### 3. @SuppressWarnings



## Reference

> [Java에서 어노테이션(Annotation)이란?](https://elfinlas.github.io/2017/12/14/java-annotation/)

