---
title: "WEB서버와 WAS 서버 알아보기"
date: 2020-03-19 23:08:08 -0400
categories: infra
---

Spring Boot로 개발한 어플리케이션을 서버에 배포해야할 일이 생겼는데,
WEB서버, WAS서버의 개념과 용도가 헷갈려서 개인공부겸 정리한 글입니다.

밑에 참조한 글을 보시면 더욱 쉽게 이해하실 수 있습니다.

## WEB Server
  
  **********정적(static)* 컨텐츠를 제공하는 프로그램 (html, css ..)

* WEB의 예 : Apache, MS IIS, NGINX ..

## WAS Server
  *동적(dynamic)* 컨텐츠를 제공하는 프로그램(Middle ware라고 생각하면 됨)
  주로 DB의 접근과 같은 로직을 중간계층에서 처리해줌
  
* WAS의 예 : Tomcat, JBoss, JEUS ..

```

Request : Client -> Web Server -> Was Server -> Database

Response : Database -> Was Server -> Web Server -> Client

```

#### WEB과 WAS를 분리하는 이유는?
  
###### 1. 기능이나 역할 분리 및 로드벨런싱

> * WAS는 다양한 로직을 직접적으로 처리하기 때문에 바쁘다.
> * 1개의 WEB서버로 다수의 WAS 서버를 연결함으로서 로드밸런싱이 가능하다.
> * SSL에 대한 암복호화 처리를 WEB 서버에서 담당한다.
  
###### 2. 장애 대응 및 처리에 유리(fail over, fail back)

> * WEB 서버와 WAS 서버가 독립적으로 분리됨으로서 WAS서버에서 나온 장애는 고객이 오류를 느끼지 못한채로 넘어갈 수 있다. (정상 동작하는 2중화 되어있는 WAS를 바라보면 됨)











### References
> <https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html/>
