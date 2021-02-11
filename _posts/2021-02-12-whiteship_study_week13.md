---
title: "[#13 백기선님 라이브 스터디] I/O"
date: 2021-02-11 19:07:10 -0900
categories: whiteship
---



![maxresdefault](https://user-images.githubusercontent.com/37217320/106457066-c5898a80-64d1-11eb-9cf2-22830bd214cc.jpg)

# 목표

자바의 Input과 Ontput에 대해 학습하세요.

# 학습 내용
* 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O

* InputStream과 OutputStream

* Byte와 Character 스트림

* 표준 스트림 (System.in, System.out, System.err)

* 파일 읽고 쓰기

  

## IO

Java에서 I/O란 Input Out의 약자로 입력 출력 작업을 의미한다. 

#### IO의 특징

* 스트림(Stream) 기반으로 동작하며, *블로킹* 방식이다.
* Non Buffer을 사용한다.



### 스트림(Steam)

스트림이란 입출력을 위해 데이터를 전송하는데에 사용되는 연결 통로이다. Stream은 단방향 통신만 가능하므로 입/출력을 하고싶으면 스트림을 두개가 필요하다.(Input, Output)



#### 스트림의 종류

* 바이트 기반 스트림 - 1 Byte 단위로 데이터를 전송한다.
* 문자 기반 스트림 - 문자 데이터를 전송할때 사용 된다. (Java에서는 char형이 2byte이므로 문자를 처리하기 위해서는 문자 기반 스트림을 이용해야한다.)
* 보조 스트림 - 실제 데이터를 주고받는 스트림은 아니며, 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.



##### IO Blocking 테스트

```java
    @Test
    @DisplayName("IO Blocking 테스트")
    void blockingTest() {
        //given
        byte[] inSrc = {1, 2, 3, 4};
        byte[] outSrc = null;


        ByteArrayInputStream input = new ByteArrayInputStream(inSrc);
        ByteArrayOutputStream output = new ByteArrayOutputStream();

        int data = 0;

        //when
        while ((data = input.read()) != -1) {
            output.write(data);
        }

        outSrc = output.toByteArray();

        //then
        Assertions.assertEquals(Arrays.toString(inSrc), Arrays.toString(outSrc));

        System.out.println("input = " + Arrays.toString(inSrc));
        System.out.println("output = "+Arrays.toString(outSrc));
    }

```



<img width="1403" alt="스크린샷 2021-02-11 오후 11 35 00" src="https://user-images.githubusercontent.com/37217320/107650389-c97f8e80-6cc1-11eb-8eea-7e2163181033.png">





## NIO(New IO)

NIO(New IO): Java 1.4 부터 도입되었으며 Java7부터 네트워크 지원이 강화된 NIO.2 API 기능이 추가되었다. 주로 연결 클라이언트 수가 많은 서버 프로그램에서 사용된다.

#### NIO의 특징

* 채널(Channel) 기반으로 동작하며 *논 블로킹* 방식이다.
* 기본적으로 Buffer을 사용한다.
* 하나의 스레드에 여러개의 연결이 되어있기에, Selector을 이용해서 Channel을 선택할 수 있다.



### 채널(Channel)

 채널은 외부 리소스와 내부의 버퍼를 연결해주는 양방향 통로이다. 단방향으로도 사용할 수 있으며, 반드시 버퍼에서 데이터를 가져와야한다.



### 셀렉터(Selector)

Selector은 어느 Channel에 연결할지 선택하는 역할을 한다.





## InputStream과 OutputStream

두 클래스는 Byte Stream의 입력과 출력을 담당하는 클래스이다.



### InputStream의 주요 메소드



#### 1. read()

1바이트를 읽고 읽은 데이터를 리턴해준다. 더이상 읽을 바이트가 없으면 -1을 리턴한다.

```java
    @Test
    @DisplayName("Byte Test")
    void byteTest() throws IOException {
        InputStream input = new ByteArrayInputStream(new byte[]{1, 'a'});

        System.out.println(input.read()); // 1을 반환한다.
        System.out.println(input.read()); // char a를 반환하며 ascii code 값인 97이 출력된다.
        System.out.println(input.read()); // inputStream이 비어있으므로 -1을 반환한다.

    }

```



#### 2.  read(byte[] b)

매개변수로 전달하는 b의 최대 크기만큼 읽고 순서대로 배열 b에 덮어씌우며, 읽은 바이트의 수를 리턴해준다.



```java
@Test
@DisplayName("Byte Test 2")
void byteTest2() throws IOException {
    InputStream input = new ByteArrayInputStream(new byte[]{1, 2, 3, 4, 5, 6});

    byte[] buf = new byte[4];

    System.out.println(input.read(buf)); // 4개가 담기므로 4를 반환한다

    for (byte b : buf) {
        System.out.print(b+ ", ");
    }
    System.out.println();


    System.out.println(input.read(buf)); // 남은 2개가 담기므로 2를 반환한다

    for (byte b : buf) {
        System.out.print(b+ ", ");
    }
    System.out.println();

    System.out.println(input.read(buf)); // 담은 내용이 없으므로 -1을 반환한다.
}
```



<img width="1349" alt="스크린샷 2021-02-12 오전 12 32 55" src="https://user-images.githubusercontent.com/37217320/107658814-dbfdc600-6cc9-11eb-803a-537ccf8d82ad.png">







## Reference

> [21. 채널 (Channel)](https://adrian0220.tistory.com/150?category=775742)