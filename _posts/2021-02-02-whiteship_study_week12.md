---
title: "[#12 백기선님 라이브 스터디] 애노테이션"
date: 2021-01-29 20:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

# 목표

자바의 애노테이션에 대해 학습하세요.

# 학습 내용
* [애노테이션 정의하는 방법](https://giyeon95.github.io/whiteship/whiteship_study_week12/#java에서-지원하는-애노테이션)

* [@Retention](https://giyeon95.github.io/whiteship/whiteship_study_week12/#retention)

* [@Target](https://giyeon95.github.io/whiteship/whiteship_study_week12/#target)

* [@Documented](https://giyeon95.github.io/whiteship/whiteship_study_week12/#document)

* [애노테이션 프로세서](https://giyeon95.github.io/whiteship/whiteship_study_week12/#애노테이션-프로세서)

  



## 애노테이션(Annotation)이란

애노테이션은 JDK 5부터 Java에서 지원하는 기능이다.  사전상의 의미는 주석이지만, 그 이상의 역할을 할수 있다.



## Java에서 지원하는 애노테이션

### 1. @Override

- 해당 메소드가 오버라이드 함을 보여준다.
- 부모 클래스 & 구현하는 대상 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 에러를 발생시킨다.
- 단 주석의 역할을 하기에, Override를 한 메소드에 @Override 에노테이션을 제외하여도 컴파일 에러는 발생하지 않는다.



<img width="909" alt="스크린샷 2021-02-02 오전 12 06 40" src="https://user-images.githubusercontent.com/37217320/106476565-887dc200-64ea-11eb-89d0-270cd746b96c.png">



<img width="901" alt="스크린샷 2021-02-02 오전 12 06 05" src="https://user-images.githubusercontent.com/37217320/106476481-7439c500-64ea-11eb-912f-54ac32e58a25.png">

 

<img width="394" alt="스크린샷 2021-02-02 오전 12 07 01" src="https://user-images.githubusercontent.com/37217320/106476612-96cbde00-64ea-11eb-9f97-6fe9bbfb34e7.png">

### 2. @Depercated

* 메서드가 더 이상 사용되지 않음을 표시한다.
* 컴파일에는 문제는 없으나, 컴파일 경고(Warning)을 보여준다.



<img width="935" alt="스크린샷 2021-02-02 오전 12 12 16" src="https://user-images.githubusercontent.com/37217320/106477264-515be080-64eb-11eb-8d42-d94530f7f45e.png">



<img width="902" alt="스크린샷 2021-02-02 오전 12 11 29" src="https://user-images.githubusercontent.com/37217320/106477199-3db07a00-64eb-11eb-8310-46fd75742b79.png">



### 3. @SuppressWarnings

* 선언 곳의 컴파일 경고를 무시한다.



####  옵션

- all : 모든 경고를 억제
- boxing : boxing/unboxing 오퍼레이션과 관련된 경고를 억제
- cast : 캐스트 오퍼레이션과 관련된 경고를 억제
- dep-ann : 권장되지 않는 어노테이션과 관련된 경고를 억제
- deprecation : 권장되지 않는 기능과 관련된 경고를 억제
- fallthrough : switch 문에서 누락된 break 문과 관련된 경고를 억제
- finally : 리턴되지 않는 마지막 블록과 관련된 경고를 억제
- hiding : 변수를 숨기는 로컬과 관련된 경고를 억제
- incomplete-switch : switch 문에서 누락된 항목과 관련된 경고를 억제(enum case)
- javadoc : javadoc 경고와 관련된 경고를 억제
- nls : 비nls 문자열 리터럴과 관련된 경고를 억제
- null : 널(null) 분석과 관련된 경고를 억제
- rawtypes : 원시 유형 사용법과 관련된 경고를 억제
- resource : 닫기 가능 유형의 자원 사용에 관련된 경고 억제
- restriction : 올바르지 않거나 금지된 참조 사용법과 관련된 경고를 억제
- serial : 직렬화 가능 클래스에 대한 누락된 serialVersionUID 필드와 관련된 경고를 억제
- static-access : 잘못된 정적 액세스와 관련된 경고를 억제
- static-method : static으로 선언될 수 있는 메소드와 관련된 경고를 억제
- super : 수퍼 호출을 사용하지 않는 메소드 겹쳐쓰기와 관련된 경고를 억제
- synthetic-access : 내부 클래스로부터의 최적화되지 않은 액세스와 관련된 경고를 억제
- sync-override : 동기화된 메소드를 오버라이드하는 경우 누락된 동기화로 인한 경고 억제
- unchecked : 미확인 오퍼레이션과 관련된 경고를 억제
- unqualified-field-access : 규정되지 않은 필드 액세스와 관련된 경고를 억제
- unused : 사용하지 않은 코드 및 불필요한 코드와 관련된 경고를 억제 [출처](https://www.freeism.co.kr/wp/archives/308#easy-footnote-bottom-1-308)



### 4. @SafeVarages

- Java 7이상부터 지원하며, 제너릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시한다.



### 5. @FunctionalInterface

* Java8 이상부터 지원하며, 함수형 인터페이스를 표기해준다.
* 메소드는 한개만 존재해야하며, 그 외에는 컴파일 오류를 발생시킨다.



<img width="948" alt="스크린샷 2021-02-02 오후 11 31 40" src="https://user-images.githubusercontent.com/37217320/106614564-d8be5800-65ae-11eb-9d6f-a0796959d7ef.png">

<img width="938" alt="스크린샷 2021-02-02 오후 11 32 26" src="https://user-images.githubusercontent.com/37217320/106614661-f25f9f80-65ae-11eb-93c4-974a34805728.png">

<img width="603" alt="스크린샷 2021-02-02 오후 11 32 36" src="https://user-images.githubusercontent.com/37217320/106614659-f12e7280-65ae-11eb-9e21-039b8c234a96.png">



#### Q. @FunctionalInterface 가 없으면 FunctionalInterface가 아닐까?

@FunctionalInterface가 없으면 FunctionalInterface를 사용할 수 없을까? 라는 의구심이 들서 확인을 진행하였다.



<img width="948" alt="스크린샷 2021-02-02 오후 11 31 40" src="https://user-images.githubusercontent.com/37217320/106614564-d8be5800-65ae-11eb-9d6f-a0796959d7ef.png">

<img width="1143" alt="스크린샷 2021-02-02 오후 11 43 14" src="https://user-images.githubusercontent.com/37217320/106616024-6babc200-65b0-11eb-9a31-bf1de5666def.png">

위 코드에서 @FunctionalInterface를 제거하고 컴파일을 해봤지만, 동일하게 동작하였다.



**즉, @Override와 같이 주석의 역할을 명시적으로 표기해주는 것으로 보기해주며 문법이 안맞을 시 컴파일 에러를 발생시키는 역할로 보인다.**





## Meta Annotation

위에서 제공하는 애노테이션 외에 Meta Annotation이 존재한다. Meta Annotation을 이용해 커스텀 어노테이션을 만들 수 있다.

* @Retention: 애노테이션의 영향 범위를 표현하며, 어떤 시점까지 애노테이션이 살아있는지(?)를 결정한다.
* @Documented: 문서에 어노테이션 정보가 표현된다.
* @Target: 어노테이션이 적용될 위치를 지정한다.
* @Inherited: 자식클래스가 어노테이션을 상속 받을 수 있다.
* @Repeatable: 반복적으로 어노테이션을 선언할 수 있게 한다. [출처](https://jdm.kr/blog/216)



- FunctionalInterface 참고

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {}
```



## @Retention

애노테이션의 영향 범위를 표현하며, RetentionPolicy의 enum타입에 정의된 값으로 영향범위를 지정할 수 있다.

```java
@Retention(RetentionPolicy.CLASS) // default는 RententionPolicy.RUNTIME 으로 지정되어 있다.
```



### RetentionPolicy 종류

* **SOURCE**: 컴파일시 해당 애노테이션의 메모리를 제외한다. (사실상 주석의 역할)
* **CLASS**: 컴파일시에는 메모리를 가져가지만, 런타임시에는 메모리에서 제외된다. (Refleaction 데이터를 가져올 수 없다.)
* **RUNTIME**: 애노테이션을 런타임시에까지 사용할 수 있다.



## @Target

애노테이션을 적용할 위치를 지정하며, ElementType의 enum타입에 정의된 값으로 적용 위치를 지정할 수 있다.



### ElementType 종류

- **ElementType.PACKAGE**: 패키지 선언
- **ElementType.TYPE**: 타입 선언
- **ElementType.ANNOTATION_TYPE**: 어노테이션 타입 선언
- **ElementType.CONSTRUCTOR**: 생성자 선언
- **ElementType.FIELD**: 멤버 변수 선언
- **ElementType.LOCAL_VARIABLE**: 지역 변수 선언
- **ElementType.METHOD**: 메서드 선언
- **ElementType.PARAMETER**: 전달인자 선언
- **ElementType.TYPE_PARAMETER**: 전달인자 타입 선언
- **ElementType.TYPE_USE**: 타입 선언

 

## @Documented

javadoc으로 api문서를 만들때, 어노테이션에 대한 설명도 포함하도록 지정한다.



## 애노테이션 프로세서

자바 컴파일러 플러그인의 일종으로, 어노테이션에 대한 코드베이스를 검사, 수정, 생성하는 역할을 담당한다.

자바 컴파일 과정에서 애노테이션이 있을때, 코드를 생성하여 클래스 파일에 생성해 낼 수도 있다.

대표적으로 롬복 어노테이션을 예로 들 수 있다.



<img width="1238" alt="스크린샷 2021-02-03 오후 10 49 30" src="https://user-images.githubusercontent.com/37217320/106755971-1637e980-6672-11eb-86d6-f79eda462172.png">

* 롬복 어노테이션을 이용해 Getter/ Setter 가 class 파일에 생성



이런 동작이 가능한 원리는 롬복 내부의 AnnotationProcessorHider 클래스의 내부 클래스인  AnnotationProcessor 클래스가 AbstractProcessor을 상속 하고 있다.

<img width="1392" alt="스크린샷 2021-02-03 오후 10 55 12" src="https://user-images.githubusercontent.com/37217320/106756707-e2a98f00-6672-11eb-82a4-021dba8320c3.png">





애노테이션의 동작 방식은 다음 순서와 같다.

1. 어노테이션 클래스를 생성한다.
2. 어노테이션 파서 클래스를 생성한다.
3. 어노테이션을 사용한다.
4. 컴파일하면, 어노테이션 파서가 어노테이션을 처리한다.
5. 자동 생성된 클래스가 빌드 폴더에 추가된다.



## Reference

> [Java에서 어노테이션(Annotation)이란?](https://elfinlas.github.io/2017/12/14/java-annotation/)
>
> [자바 어노테이션(Java Annotation)](https://jdm.kr/blog/216)