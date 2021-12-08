---
layout: post
title:  "Memory Leak"
date:   2021-12-08 00:00:00 +0900
category: Android
---

# Memory Leak

## Memory Leak란?

- 어플리케이션이 사용이 끝난 메모리를 반환하지 않는 것
- 사용한 메모리는 반환하지 않은 채 추가로 필요한 메모리를 시스템에 요청하기 때문에 사용하는 메모리 양이 계속 증가한다
- 이 과정이 계속되면, 최악의 경우 OOM(Out Of Memory)를 발생시키고 어플리케이션이 강제 종료 되면서
  어플리케이션에 할당되었던 메모리가 시스템으로 회수된다

## 자바의 Memory Leak

- 자바는 사용한 메모리를 명시적으로 해제하지 않는다
- GC(Garbage Collection)이 이를 대신한다
- 어플리케이션이 사용하지 않는 메모리라도 GC 실행 시 시스템이 사용중인 메모리로 판단하고
  회수하지 않는다면 여전히 메모리 릭은 발생할 수 있다
  
## 안드로이드의 Memory Leak

- 안드로이드 앱의 메모리가 누수되는 과정은 대체로 자바와 같다
- 시스템이 앱에 할당하는 메모리 제한이 있다
- 어플리케이션이 사용할 수 있는 최대 메모리 크기를 넘어서면 OOM이 발생
- 안드로이드 앱에서 사용하는 모든 객체는 RAM에 상주
- 안드로이드에서 발생하는 릭은 보통 액티비티나 이미지와 함께 발생

## Memory Leak 원인

### Broadcast Receivers

- Activity 내에 local Broadcast Receiver를 register 해두었는데 unregister를 하지 않으면
  Activity가 종료된 후에도 Broadcast receiver가 activity에 대한 참조를 가지고 있어서 leak 발생
- 해결책
  * 항상 onStop에서 unregiser()를 call한다
- Broadcast receiver를 onCreate()에서 register했다면 앱이 background에서 onResume()으로 돌아왔을 때
  정상적으로 register 되지않으므로 항상 register는 onStart() 또는 onResume()에서 해야한다
  
### Static Acctivity or View Reference

- textview를 static으로 선언했다면 이 static field가 activity 또는 view를 직간접적으로
  참조한다면 leak 발생
- 해결책
  * activity, view, context에 대해서는 절대 static 변수를 사용하지 않는다

### Singleton Class Reference

- Singleton class를 정의하고 local storage로부터 파일 몇개를 fetch하기 위해 context를 전달할 필요가 있다고 가정
- 해결책
  * singleton class에 activity context 대신 application context()를 넘긴다
  * activity context를 넘겨야 한다면, activity가 destroy될 때 확실하게 single class에
    넘긴 context가 null로 set 되었는지 확인한다
    
### Inner Class Reference

- inner class를 정의했고, 다른 activity에 redirect 하기위해 activity를 넘겨야할 필요가 있다고 가정
- inner class 변수의 static 선언 X non-static class로 변경
- inner class에서는 class 변수 생성 X
- inner class에 activity instance 전달 X
- 해결책
  * class는 static으로 set해야한다
  * anonymous class instance는 static으로 선언되면 outer class에 대해 암묵적인 참조를 갖지 않는다
  * 어떤 View/Activity에 대해서 WeakReference를 사용
  * Garbage Colletor는 WeakReference로 가리키는 객체에 대해서만 collect 할 수 있다
    
### Anonymous Class Reference

- Inner Class Reference와 같다
- 해결책
  * inner class에 static 변수 쓰지 않기
  * inner class는 static class로 사용
  * weakReference로 activity 참조하기
    
### AsyncTask Reference

- onPostExecute()에서 textView를 업데이트 하기 위해 사용된 strig 변수를 asyncTask를 사용해서 get 해오고 있다고 가정
- activity 안에서 class를 참조하지 않는다
- inner class의 부모 activity class에 대해 어떤 암묵적 참조도 하지 않도록 static inner class로 해줘야한다
- activity가 destroy될 때 항상 asyncTask를 cancel 해줘야한다
- asyncTask 내에서 activity로 부터 View에 대한 직접적인 reference를 하지 않는다
- weakReference 사용

### Handler Reference

- 5초 후에 새로운 스크린으로 redirect하는 Handler를 사용하고 있다고 가정
- activity 내에서 class를 참조하지 않는다 
- Handler는 main Thread에서 인스턴스화되어서 Looper의 message queue와 연관되어있다
- 바로 참조하지 말고 activity의 weakReference를 사용

### Thread Reference

- Thread와 TimerTask class 양쪽에서 이같은 실수를 반복할 수 있다
  * static 변수 사용
  * non-static anonymous class 사용
- 해결책
  * static 변수 제거
  * activity destroy할 때 thread를 kill 해준다
  * static inner class로 Thread class를 사용한다

### [참고]
<http://sunphiz.me/wp/archives/2332> <br>
<https://ciwhiz.tistory.com/283>