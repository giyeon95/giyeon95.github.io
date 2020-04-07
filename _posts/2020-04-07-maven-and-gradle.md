---
title: "maven과 gradle 차이점 간단히 알아보"
date: 2020-04-07 23:21:08 -0400
categories: infra
---

근래 프로젝트를 진행하면서 신규 프로젝트의 빌드도구는 Gradle로 구성되었으나,
기존에 개발되었던 파일들은 Maven 으로 작성되기어 있는게 대다수였다.
작성 문법이 다른점 말고 Gradle의 장점이 무었이 있을까 생각하다 정리겸 글을 작성한다.


## What is Maven?
* Java용 프로젝트 관리 도구로서 Apache Ant의 대안으로 제작되었다.
* Apache 라이선스로 배포된 오픈소스 소프트웨어
* 멀티 프로젝트에서 상속 구조 사용
* pom.xml 파일에 필요한 라이브러리를 선언하는 방식을 사용한다.

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```


## What is Gradle?
* Java, C/C++, Python 등 여러가지 언어 지원한다
* Apache 라이선스로 배포된 오픈소스 소프트웨어용
* 멀티 프로젝트에서 주입 방식 사용
* Groovy라는 언어를 활용한 DSL 스크립트로 사용한다.
* 현재 안드로이드 OS 빌드도구로 채택

```groovy
// https://mvnrepository.com/artifact/junit/junit
testCompile group: 'junit', name: 'junit', version: '4.12'
```


## Gradle이 Maven보다 장점?
* XML로 정의하는 Maven의 방식보다 DSL 스크립트의 가독성이 좋음
* 주입 방식을 사용하면서, 상속 구조의 단점을 해소
* Build속도가 Maven보다 100배 이상? 빠르다고 함 
> ref: <https://gradle.org/gradle-vs-maven-performance//>


## 마치며..
> 간단한 내용만 정리했지만, Gradle과 Maven은 작성방법만 차이가 있는줄 알았다.
> 그러나 작성법 외에도 멀티프로젝트에서의 **내부 동작 방식**, **빌드 속도**, **언어 지원 범위** 등
> 몰르고 넘어갈뻔한 다양한 차이점이 있었다. 추후에 시간에 여유가 있을때 조금 더 깊게 정리해보아야겠다.



