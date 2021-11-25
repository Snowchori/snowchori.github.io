---
layout: post
title:  "Higher Order Functions"
date:   2021-11-24 20:00:00 +0900
category: Kotlin
---

# Higher Order Functions

## 고차원함수

- 코틀린의 함수는 1급 객체
- 인자로 전달될 수 있고 다른 함수의 리턴값으로도 사용될 수 있다 또한 변수에 부여될 수 있다
- 아래 2가지 조건 중 하나를 만족하는 함수를 고차원함수라 부른다
    * 함수를 인자로 받는 함수
    * 리턴값으로 함수를 사용하는 함수
- 인자로 전해지는 함수를 람다라고 한다

## Function Type

- 코틀린은 함수를 다룰 때 타입 선언에 친숙하고 이를 Function Type이라 부른다

```kotlin
(a:Int, b:Int) -> Int
```

- a, b 두 인자를 Int 타입으로 받고 Int 타입의 무언가를 리턴하겠다는 뜻

```kotlin
val onClick: () -> Unit = ...
```

- Function Type은 수신 객체 타입을 가질 수 있다

```kotlin
A.(B) -> C
```

- 수신 객체인 A가 B 타입의 인자를 받으며 C 타입의 리턴값을 내보내겠다는 의미이고
  이 함수의 호출은 A라는 객체에서 행해지게 된다

## 람다 식의 문법

- 람다는 매개 변수로 전해지는 함수이다
- 람다 식은 반드시 중괄호로 묶여 있어야 하며 아래와 같은 문법으로 정의할 수 있다

```kotlin
{ 매개변수1: 타입, 매개변수2: 타입.. -> 반환형 }
```

```kotlin
fun main(args: Array<String>) {
  val sum = {x: Int, y: Int -> x + y}
  println(sum(1, 2))
}
```

> 3

- sum이라는 변수에 람다 식을 정의하여 넣었다
- 람다가 저장된 변수는 다른 일반 함수와 동일하게 사용할 수 있다

```kotlin
val sum: (Int, Int) -> Int = {x, y -> x + y}
```

- 정식 포맷

## 익명 함수

- 람다를 사용하는 대부분의 경우에는 함수의 이름이 필요가 없다
- 이름이 없는 함수를 익명 함수라고 하며 아래와 같이 함수 구문만 정의하여 전달

```kotlin
Calculator(2, 1, {a: Int, b: Int -> a + b})
```

- {a: Int, b: Int -> a + b} = 람다식
- Calculator 함수 = 고차 함수
- 람다식을 받는 고차 함수

```kotlin
fun Calculator(a: Int, b: Int, p: (Int, Int)) {
    println("$a, $b -> ${p(a, b)}")
}

fun main(args: Array<String>) {
  Calculator(2, 1, {a: Int, b: Int -> a + b})
}
```

> 2, 1 -> 3

- 고차 함수에서는 람다 식의 형식((Int, Int) -> Int)반드시 명시
- 고차 함수에 명시된 형식과 동일한 형식의 람다 식만 전달할 수 있다

## 람다 식의 표현

- 함수 호출 시 맨 마지막 인자가 람다일 경우 람다 식을 소괄호 밖으로 뺄 수 있다

```kotlin
Calculator(2, 1) {a: Int, b: Int -> a - b}
```

- 컴파일러가 유추할 수 있는(스마트 캐스트가 가능한) 타입일 경우,
  즉 호출하는 고차 함수에 람다 식의 인자 타입이 명시되어 있는 경우 표현 식에서의 인자 타입은 생략 가능
  
```kotlin
Calculator(2, 1) {a, b -> a * b}
```

- 람다 식의 인자 값이 단 하나 뿐이라면 인자를 생략할 수 있다
- 본문에서 인자를 사용하고 싶다면 매개 변수가 하나 뿐일 때
  그 매개 변수 이름을 대신해 사용할 수 있는 키워드 'it'을 사용하면 된다
  
```kotlin
Square(2) { a -> a * a}
Square(2) {it * it} // 매개 변수 이름 생략 -> it 사용
```

- 호출하는 고차 함수에 인자가 람다 식 뿐이라면 호출 식의 소괄호 역시 생략할 수 있다

```kotlin
fun PrintInfo(p: () -> Unit) {
  print("Calculator Version: ")
  p()
}

fun main(args: Array<String>) {
  PrintInfo() { println("1.0") }
  PrintInfo { println("1.1") } // () 생략
}
```

- Unit은 반환 타입이 없다는 의미
- 람다 역시 Nullable 타입이 될 수 있다
- 람다를 매개 변수로 사용하는 고차 함수 정의에 ?문자를 붙이면 된다

```kotlin
fun PrintInfo(p: (() -> Unit)? = null) {
  print("Calculator Version: ")
  p?.invoke()?:println("no Version")
}
```

- invoke()는 매개 변수에 따라 함수를 호출하는 메서드
- 일반 함수도 람다에 들어갈 수 있다
- 함수명 앞에 ::를 붙여 넣어주면 된다

```kotlin
fun sum(a: Int, b: Int) = a + b

fun main(args: Array<String>) {
    Calculator(2, 1, ::sum)
}
```

- 함수형 변수를 만들어 넣는 것도 가능

```kotlin
fun main(args: Array<String>) {
  val minus: (Int, Int) -> Int = {a, b, -> a - b}
  Calculator(2, 1, minus)
}
```

### [참고]
<https://phantasmicmeans.tistory.com/entry/11-Kotlin-Higher-Order-Functions#recentEntries> <br>
<https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=yuyyulee&logNo=221235421524>