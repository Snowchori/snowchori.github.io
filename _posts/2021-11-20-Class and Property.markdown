---
layout: post
title:  "Class and Property"
date:   2021-11-20 20:00:00 +0900
category: Kotlin
---

# Class and Property

## Class

- 클래스 : 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀
- 클래스 사용 목적 : 데이터의 캡슐화와 캡슐화한 데이터를 다루는 코드를 한 주체 아래 담는 것
- 클래스는 사용자에게 그 데이터에 접근할 수 있도록 접근자 메소드를 제공
- 접근자 메소드 : 보통 필드를 읽기 위한 getter와 필드를 변경하기 위한 setter를 제공

자바 클래스

```java
public class Person {
  private final String name;

  public Person(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }
}
```

- 필드가 둘 이상으로 늘어나면 생성자인 Person(String name)의 본문에서 파라미터를
  이름이 같은 필드에 대입하는 대입문의 수도 늘어난다
- 자바에서는 생성자 본문에 이같은 코드가 반복적으로 들어가는 경우가 많다

코틀린 클래스

```kotlin
class Person(val name: String)
```

- 코드가 간결해짐
- 이런 유형의 클래스를 value object라고 부름
- value object : 코드가 없이 데이터만 저장하는 클래스
- 코틀린의 기본 가시성은 public이므로 변경자를 생략해도 된다

## 프로퍼티

- 프로퍼티 : 언어 기본 기능이며 자바의 필드와 접근자 메소드를 완전히 대신한다
- 프로퍼티를 선언할 때는 val이나 var를 사용
- val로 선언한 프로퍼티는 읽기 전용
- var로 선언한 프로퍼티는 변경 가능

```kotlin
class Person(
  val name: String, // 읽기 전용 프로퍼티, 필드와 필드를 읽는 게터를 만들어낸다
  var isMarried: Boolean // 쓸 수 있는 프로퍼티, 필트, 게터, 세터를 만들어낸다
)
```

코틀린 Person 클래스 사용

```kotlin
val person = Person("Bob", true) // new 키워드를 사용하지 않고 생성자 호출
println(person.name) // 프로퍼티 이름을 직접 사용해도 자동으로 게터 호출
println(person.isMarried)
```

출력

> Bob <br> true

- 세터를 사용할 때도 person.isMarried = false 로 바꿔주면 된다
- 대부분 프로퍼티에는 그 프로퍼티의 값을 저장하기 위한 필드가 있는데,
  이를 프로퍼티를 뒷받침하는 필드라 부른다
- 프로퍼티의 값을 그때그때 계산할 수도 있다 (다른 프로퍼티들로부터 값을 계산할 수 있다)
  -> 커스텀 게터 작성
  
## 커스텀 접근자

- 직접 작성한 프로퍼티의 접근자

자신이 정사각형인지 판단하는 기능을 가진 Rectangle 클래스

```kotlin
class Rectangle(val height: Int, val width: Int){
    val isSquare: Boolean
    
    get(){ // 프로퍼티 게터 선언
        return height == width
    }
}
```

- 이 프로퍼티에는 자체 구현을 제공하는 게터만이 존재하기 때문에 사용자가 프로퍼티에 접근할 때 마다
  게터가 프로퍼티의 값을 매번 계산
  
위의 코드를 더 간결하게 작성

```kotlin
class Rectangle(val height: Int, val width: Int){
    val isSquare: Boolean
    
    get() = height == width
}
```

### [참고]
Kotlin IN ACTION <br>
<https://hyeals.tistory.com/10>