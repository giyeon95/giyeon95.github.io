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

InputStream의 경우 ByteArrayInputStream을 구현체로 사용하였다.

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
        System.out.println(Arrays.toString(buf));
        
        System.out.println(input.read(buf)); // 남은 2개가 담기므로 2를 반환한다
        System.out.println(Arrays.toString(buf));

        System.out.println(input.read(buf)); // 담은 내용이 없으므로 -1을 반환한다.
}
```



<img width="1552" alt="스크린샷 2021-02-15 오후 3 21 00" src="https://user-images.githubusercontent.com/37217320/107912246-882cfe80-6fa1-11eb-8da0-fe39662da5ec.png">



#### 3.  read(byte[] b, int off, int len)

입력 스트림으로부터 len개의 바이트만큼 읽고 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장한다. 그리고 실제로 읽은 바이트 수인 len개를 리턴한다. 만약 len개를 모두 읽지 못하면 실제로 읽은 바이트 수를 리턴한다.

```java
  @Test
    @DisplayName("Byte Test 3")
    void byteTest3() throws IOException {
        InputStream input = new ByteArrayInputStream(new byte[]{1, 2, 3, 4, 5, 6});

        byte[] buf = new byte[4];

        System.out.println(input.read(buf, 2, 1)); // len에 표기된 길이만큼 buf의 index 2부터 저장한다. 리턴값은 실제 읽은 byte수를 리턴한다.
        System.out.println(Arrays.toString(buf));

        System.out.println(input.read(buf, 0, 2));
        System.out.println(Arrays.toString(buf));

        System.out.println(input.read(buf, 0, 4)); // 실제로 읽은 byte수는 3이다.
        System.out.println(Arrays.toString(buf)); // index 0 부터 읽은 값을 덮어씌운다.


    }
```



<img width="1240" alt="스크린샷 2021-02-15 오후 3 29 47" src="https://user-images.githubusercontent.com/37217320/107912861-a6dfc500-6fa2-11eb-873a-1ced4e1bc215.png">



#### 4. close()

사용한 시스템 자원을 반납하고 입력스트림을 닫아준다.

```java
  @Test
    @DisplayName("InputStream close Test")
    void bytesTest4() throws IOException {
        //given
        InputStream input = new ByteArrayInputStream(new byte[]{1, 2, 3});

        //when
        input.close();

        //then
        System.out.println(input.read());
    }
```



위 close()를 이용하면, input.read()가 동작을 안할줄 알았지만 아래와 같이 정상 동작함을 볼 수 있었다.

<img width="1419" alt="스크린샷 2021-02-15 오후 3 43 48" src="https://user-images.githubusercontent.com/37217320/107913892-9af50280-6fa4-11eb-8891-b7d3a7c53e20.png">

왜그런지 조금 더 살펴보았더니 InputStream의 close메소드는 아무작업도 수행하지 않는다고 한다. 

>Closes this input stream and releases any system resources associated with the stream.
>The close method of InputStream does nothing.
>Throws:
>IOException – if an I/O error occurs.



### OutputStream의 주요 메소드

outputStream의 경우, FileOutputStream을 구현체로 사용하였다.

#### 1. write(int b)

매개변수 b는 0에서 255까지의 정수를 인자로 받고 이에 대응하는 바이트를 출력 스트림에 쓴다.

만약 0에서 255의 범위를 벗어난 int타입의 값이 write(int b) 메소드에 전달되면,

int 타입의 최하위 비트(least significant byte)가 쓰이고 나머지 3바이트는 무시된다.

(int 타입을 byte 타입으로 캐스팅하면서 이러한 효과가 발생한다.)

출처: https://cbts.tistory.com/54 [IT일기장]



```java
    @Test
    @DisplayName("OutputStream test")
    void outputStreamTest() throws IOException {
        final String PATH = "/Users/giyeon/Desktop";

        try (OutputStream newOutput = new FileOutputStream(PATH + "/outputExample.txt")) {
            newOutput.write(256 + 97); // a
            newOutput.write(98); // b
        }
    }
```



![image](https://user-images.githubusercontent.com/37217320/107920629-9fbfb380-6fb0-11eb-88c3-73fa951c23b4.png)



<img width="842" alt="스크린샷 2021-02-15 오후 5 06 12" src="https://user-images.githubusercontent.com/37217320/107920322-1e682100-6fb0-11eb-9373-1171659a454d.png">



#### 2. write(byte[] b)

매개변수 b로 전달된 byte array를 출력 스트림에 쓴다.

```:q!
@Test
@DisplayName("OutputStream test2")
void outputStream2Test() throws IOException {
    final String PATH = "/Users/giyeon/Desktop";

    String contents = "Hello whiteship java online study!!";
    byte[] bytes = contents.getBytes();

    try (OutputStream output = new FileOutputStream(PATH + "/outputExample.txt")) {
        output.write(bytes);
    }
}
```

<img width="842" alt="스크린샷 2021-02-15 오후 5 17 49" src="https://user-images.githubusercontent.com/37217320/107921359-be727a00-6fb1-11eb-8185-6dcc9070e19e.png">

#### 3. write(byte[] b, int off, int len)

b[] 값중 b[off] 부터, off+len 까지의 값만 출력스트림에 쓴다.

off+len의 길이가, b[]의 길이를 초과하면 IndexOutOfBoundsException가 발생한다.

```java
@Test
@DisplayName("OutputStream test3")
void outputStream3Test() throws IOException {
    final String PATH = "/Users/giyeon/Desktop";

    String contents = "Hello whiteship java online study!!";
    byte[] bytes = contents.getBytes();

    try (OutputStream output = new FileOutputStream(PATH + "/outputExample.txt")) {
        output.write(bytes, 6, 10);
    }
}
```

![스크린샷 2021-02-15 오후 5 22 18](https://user-images.githubusercontent.com/37217320/107921799-5ec89e80-6fb2-11eb-90a2-d73095937b0c.png)

#### 4. flush()

출력 스트림 내부에는 작은 버퍼가 있는데, 데이터 출력전에 버퍼에 쌓여있다가 순서대로 출력한다. flush는 버퍼에 남아있는 데이터를 모두 출력시키고 버퍼를 비워준다.

> FileOutputStream에서는 flush()를 구현하지 않아, 별도 동작이 없다.



#### 5. close()

출력 스트림을 닫고, 이 스트림과 관련된 모든 시스템 리소스를 해제한다.

> try-catch-resources를 사용하면 별도의 close() 호출 없이 내부적으로 close()가 호출한다.

<img width="505" alt="스크린샷 2021-02-15 오후 5 35 33" src="https://user-images.githubusercontent.com/37217320/107923082-55403600-6fb4-11eb-81e3-99450cdbdd00.png">





## Byte와 Character 스트림

byte 스트림은 입출력 단위가 1 byte이다. Java에서는 한문자를 의미하는 char형이 2 byte 이기에 byte 스트림으로는 처리하는 데에는 어려움이 있다.

 이점을 보완하기 위해 문자기반의 스트림이 지원 된다. (Reader, Writer)



| 바이트 기반 보조 스트림                       | 문자 기반 보조스트림               |
| :-------------------------------------------- | :--------------------------------- |
| BufferedInputStream<br />BufferedOutputStream | BufferedReader<br />BufferedWriter |
| FilterInputStream<br />FilterOutputStream     | FilterReader<br />FilterWriter     |
| LineNumberInputStream(deprecated)             | LineNumberReader                   |
| PrintStream                                   | PrintWriter                        |
| PushbackInputStream                           | PushbackReader                     |





## 표준 스트림 (System.in, System.out, System.err)

System.in, System.out, System.err 은 java.lang.System으로 멤버변수인 in, out, err을 통해 표준 입력, 출력, 에러를 제공한다.



### 1. System.in

System 클래스 안의 멤버변수 in의 타입은 InputStream이다. InputStream은 최상위 클래스이면서 추상클래스이다. 

System.in을 통하여 지정하는 객체는 JVM이 메모리로 올라오면서 객체를 생성하고 넣어주는 것으로 보인다. 특징으로는 InputStream이므로 바이트 단위로만 입출력이 허용된다.

![스크린샷 2021-02-19 오전 12 07 46](https://user-images.githubusercontent.com/37217320/108376620-82a41280-7246-11eb-9690-5c87cdf7f0db.png "System.java 내의 in 변수")



![스크린샷 2021-02-19 오전 12 15 48](https://user-images.githubusercontent.com/37217320/108377686-9f8d1580-7247-11eb-973b-5463dff4065d.png)



![스크린샷 2021-02-19 오전 12 15 34 "in을 지정해주는 듯한 클래스(확실하지는 않다)"](https://user-images.githubusercontent.com/37217320/108377654-98fe9e00-7247-11eb-99f7-473dc7f083ab.png)



### 2. System.out

System 클래스 안의  out 변수는 PrintStream 타입이다. PrintStream은 OutputStream을 구현하고 있으며, Exception을 안전하게 처리한 메소드로만 구성되어 있다.

 ![스크린샷 2021-02-19 오전 12 24 22](https://user-images.githubusercontent.com/37217320/108378989-d3b50600-7248-11eb-8c1a-2de6dab63d1c.png)



### System.err

System.err은 out과 유사하며, PrintStrea을 구현한다. 정상적인 출력은 out으로 나가고, 오류가 발생하였을때 알려주어야 할 내용은 System.err로 나간다고 한다.



![스크린샷 2021-02-19 오전 12 28 09](https://user-images.githubusercontent.com/37217320/108379499-5a69e300-7249-11eb-84ad-b3cafec0bb48.png)



## Reference

> [Java의 정석 [2판]](https://www.kangcom.com/sub/view.asp?sku=201002020001)
>
> [21. 채널 (Channel)](https://adrian0220.tistory.com/150?category=775742)
>
> [[JAVA\] System 클래스(표준 입출력) : System.in, System.out, System.err](https://hyeonstorage.tistory.com/235)