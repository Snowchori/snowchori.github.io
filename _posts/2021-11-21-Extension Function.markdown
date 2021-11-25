---
layout: post
title:  "Extension Function"
date:   2021-11-21 16:00:00 +0900
category: Kotlin
---

# Extension Function

## 확장함수

- 어떤 클래스의 멤버 메소드인 것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수
- 하나의 클래스에 추가적인 메소드를 구현하고 싶을 때, 해당 클래스를 상속받은 새로운
  클래스 없이 구현할 수 있는 기능
- 이를 통해 새로운 클래스를 만들 필요도 없어지고 코드가 더욱 직관적이다

## 확장함수 만들기

- 확장함수를 선언하기 위해서는 함수의 이름 앞에 확장하고자 하는 클래스의 타입을 붙여줘야 한다
- 이 타입을 Receiver type이라고 한다
- MutableList<Int> 타입에 swap이라는 함수 추가

```kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {
  val tmp = this[index1]
  this[index1] = this[index2]
  this[index2] = tmp
}
```
  
- 확장함수에서 this 키워드는 함수명 앞에 붙여준 Receiver type에 대응
- 확장함수 내에서는 this를 통해 Receiver type 내에 정의된 함수들을 사용할 수 있다
- Receiver 클래스 내에 private으로 선언된 것들에는 접근할 수 없다
- 위에서 정의한 swap 함수는 원래 MutableList 클래스에 정의된 함수인 것처럼 사용할 수 있다

```kotlin
val list = mutableListOf(1, 2, 3)
list.swap(0, 2)
```

- Receiver type이 제네릭으로 선언되었다면 확장함수 또한 제네릭을 지정해줄 수 있다

```kotlin
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
  val tmp = this[index1]
  this[index1] = this[index2]
  this[index2] = tmp
}
```

## 확장함수가 갖는 특징

- 확장함수는 상속이나 복잡한 디자인 패턴 없이 간단하게 확장기능을 만들 수 있다
- 보일러플레이트 코드를 줄일 수가 있다
- 정적 바인딩 된다
  * 정적 바인딩은 함수 호출 부분에 메모리 주소값을 저장하는 작업이 컴파일 시간에
    행해지는 것으로 컴파일 이후의 갑싱 변경되지 않는 것을 의미

```kotlin
open class Shape
class Rectangle: Shape() // Rectangle 클래스가 Shape 클래스를 상속 받음

fun Shape.getName() = "shape" // Shape 클래스의 확장함수 getName()

fun Rectangle.getName() = "rectangle" // Rectangle 클래스의 확장함수 getName()

// 확장함수 호출
fun printClassName(s: Shape) {
    println(s.getName())
}

printClassName(Rectangle())
```

- 이 경우에는 "shape"가 출력 된다
- printClassName()에 Rectangle 타입의 인스턴스를 전달했지만 확장함수는 정적 바인딩되므로
  확장함수 호출 부분에 저장되는 메모리 주소가 이미 컴파일 되는 시간에 결정되었다
- 확장함수 호출부분인 s.getName() 부분에 이미 Shape 클래스의 확장함수인 getName() 함수가
  저장된 메모리 주소가 저장되어있다
- printClassName(Rectangle())로 호출하더라도 프로세서의 경우에는 s.getName()부분에 저장된
  메모리 주소만을 알고 있어 이 위치에 있는 코드를 실행
- 함수나 프로퍼티의 이름이 겹치지 않도록 주의하며 receiver type의 멤버함수로 변수타입
  매개변수 타입과 개수가 같은 함수가 있으면 무시된다
  
```kotlin
class Car { 
  fun shape(str: String) { 
    println("빨강") 
  }
  
  fun num(int: Int) {
      println("2")
  }
}

fun Car.shape(str: String) {
    println("노랑")
}

fun Car.num(str: String) {
    println("3")
}

fun main() {
  Car().shape("A")
  Car().num(1)
  Car().num("B")
}
```

> 빨강 <br> 2 <br> 3

- Car.shape(str: String)의 경우 멤버 함수인 shape()와 이름도 같고, 매개 변수 타입도 같아서 출력되지 않는다
- Car.num(str: String)의 경우 멤버 함수인 num()과 이름이 같지만, 매개 변수 타입이 달라서 출력이 이루어진다
- 즉 오버로딩이 이루어진다

### [참고]
<https://minz.dev/12> <br>
<https://readystory.tistory.com/130> <br>
<https://velog.io/@ho-taek/Kotlin-%ED%99%95%EC%9E%A5-%ED%95%A8%EC%88%98Extension-Function>