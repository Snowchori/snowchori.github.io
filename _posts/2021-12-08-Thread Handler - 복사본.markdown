---
layout: post
title:  "Thread Handler"
date:   2021-12-08 19:00:00 +0900
category: Android
---

# Thread Handler

## Thread

### Thread란?

- 프로세스 내부에서 자원을 공유하며 실행되는 흐름의 단위
- 여러 갈래의 작업 흐름을 만듦으로써 한 앱 안에서 여러 동작을 하는 것이 가능하게 해준다
- 하나의 프로세스에서 동작하기 위해 한 프로세스의 자원을 공유한다

### 안드로이드의 Thread

- 안드로이드에서 어플리케이션을 실행했을 때 특별히 다른 작업을 하게끔 구현한 것이 아니라면
  기본적으로 안드로이드 시스템은 싱글 스레드를 갖는 Linux 프로세스를 생성
- 안드로이드 어플리케이션에는 Activity, Service 등 다양한 컴포넌트가 존재하는데 별도의 설정을
  해주지 않는다면 기본적으로 동일 프로세스의 동일 스레드에서 실행
- 어플리케이션을 실행하면 안드로이드 시스템이 그 어플리케이션에 대한 스레드를 생성하는데
  이를 메인 스레드라고 한다
- 안드로이드의 메인 스레드는 안드로이드 UI와 어플리케이션이 상호작용하는 스레드
- Main Thread를 UI Thread 라고도 부른다
- 안드로이드의 컴포넌트들은 기본적으로 UI Thread에서 시작되기 때문에 시스템 콜백에
  응답하는 메소드 또한 항상 UI Thread에서 실행
- UI Thread에게는 오직 UI와 관련된 처리만 할 수 있도록 하고 네트워크나 DB 등 리소스를
  많이 잡아먹는 작업에 대해서는 차단한다
- UI Thread가 차단하고 있는 작업들에 대해 대신 처리를 해주는 별도의 스레드를 생성해야하는데
  이를 Worker Thread라고 한다
- 안드로이드는 UI Thread에게는 네트워크, DB 쿼리 등에 대한 작업을, Worker Thread에게는
  UI 관련 작업을 못하게 하고있다

## Handler

### Handler란?

- 스레드와 스레드간의 통신 스레드에서 View 자원에 접근을 도와주는 다리
- 메세지를 전달하는 기능
- 스레드의 메시지큐와 관련된 Message와 Runnable 객체에 대해 전송하거나 실행할 수 있게 해준다
- 핸들러 인스턴스는 핸들러 인스턴스를 생성하는 스레드와 그 스레드의 메시지큐와 연관되어 있다
- 핸들러가 그 메시지큐에 Message나 Runnable을 전달하기도 하고 메시지큐에서 꺼내 실행하기도 할 수 있다

### Handler가 쓰이는 용도

- 다른 스레드에서 수행되어야 하는 작업을 Message Queue에 enqueue
- 미래 특정 시점에 실행되어야 하는 Message나 Runnable 객체 스케줄링

<img src="http://snowchori.github.io/assets/img/Thread.png">

### [참고]
<https://readystory.tistory.com/50> <br>
<https://maejing.tistory.com/entry/Android-%EC%8A%A4%EB%A0%88%EB%93%9CThread%EC%99%80-%ED%95%B8%EB%93%A4%EB%9F%ACHandler-%EC%9E%91%EC%97%85-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%97%90%EC%84%9C-UI-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%EC%9A%94%EC%B2%AD%ED%95%98%EA%B8%B0> <br>
<https://blog.naver.com/PostView.nhn?blogId=sksghkdwjddk&logNo=221493838038&parentCategoryNo=&categoryNo=47&viewDate=&isShowPopularPosts=true&from=search>