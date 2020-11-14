---
title: "[#1 백기선님 라이브 스터디] JVM 은 무엇이며 자바 코드는 어떻게 실행하는 것인가?"
date: 2020-11-09 22:05:10 -0400
categories: whiteship
---

백기선님이 live-study를 진행한다는 영상을 보았다.
이 기회에 **JAVA 기본기**를 정리할 수 있을 것 같아 참여해보려고 한다.. 


# 목표
* 자바 소스파일(.java)을 JVM으로 실행하는 과정 이해하기.

## JVM 이란 무엇인가?

**정의**
- JVM(Java Virtual Machine)의 줄임말로, Java로 작성된 파일이 OS에 구애받지 않고 재사용을 가능하게 해주는 소프트웨어 구현체
  (OS에 종속되는 역할은 JVM의 대신 담당해주기 때문이라고 알고있다.)

**역할**
1. OS에 구애받지 않고 JAVA가 동작 할 수 있게 해준다.

    ```text 
    ------------------------------
    |          Program           |
    ------------------------------
                   |
    ------------------------------
    |            JVM             | # JVM이 OS와 호환성을 대신 담당해준다.
    ------------------------------
                   |
    ------------------------------
    |      Operating System      | 
    ------------------------------
                   |
    ------------------------------
    |          Hardware          |
    ------------------------------
    ```
   
2. Garbage collection(GC)의 메모리 관리
    ```text
    - OS 레벨에서의 memory leak 방지
    - 프로그램이 동적으로 할당했던 메모리 영역 중 필요 없게 된 영역을 해지하는 기능
    ```
    
## 컴파일 & 실행 하는 방법
1. java 소스(.java)를 Javac로 빌드하면, javac는 bytecode(.class)파일을 생성한다.
2. bytecode는 JVM 내부에 있는 Class Loader을 통해 Execution Engine 으로 배치된다.
3. Execution Engine 안은 Interpreter와 JIT로 구성되어 있는데, Interpreter은 코드를 한줄씩 읽어들이다, 일정 시점이 되면 JIT Compiler에 캐싱하여 보관
    - 한번 컴파일된(JIT에 캐싱된)코드는 빠르게 실행되는 장점이 있지만, 한번만 실행될 코드는 Interpreter 하는게 더 유리할 수도 있다.
4. Execution Engine에서 실행한 필요한 녀석들은 Runtime Area로 이동하며, JVM의 메모리에 올려 실행한다.


## bytecode란?
- java 소스를 컴파일시 만들어지는 파일 (.class)
- JVM이 이해할 수 있는 언어로 변환된 파일

## JIT 컴파일러란?
- Just In Time의 약어로, Execution Engine의 구성요소로 들어있으며, Interpreter가 읽어들인 코드를 캐싱하여, 속도 개선을 만들어줌

## JVM의 구성요소
 1. Class Loader
    - javac가 컴파일한 byteCode를 Runtime Date Area로 적재하는 역할을 한다.
 2. Runtime Data Area
    - JVM이 프로그램을 수행하기 위해 운영체제로부터 할당 받는 메모리 영역 (총 5개 영역으로 이뤄짐)
 3. Execution Engine
    - Interpreter와 JIT로 구성되어있으며, 메모리에 적재된 클래스를 기계어로 변경해 명령어 단위로 실행한다.
 4. Garbage Collector
    - RunTime Data Area의 내부 Heap 메모리에 생성된 객체들 중, 참조되지 않는 객체들을 제거하는 역할 (Miner Gc, Major GC, Full GC)
 
 

## JDK, JRE 차이점
 - JDK = JRE + Development Tools (java를 빌드할 수 있도록 해줌)
 - JRE = JVM + Library (java 빌드 파일 실행환경 제공)


### References
> <https://asfirstalways.tistory.com/158>
>