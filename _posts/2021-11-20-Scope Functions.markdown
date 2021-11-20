---
layout: post
title:  "Scope Functions"
date:   2021-11-20 20:00:00 +0900
category: Kotlin
---
# 범위 지정 함수

- 코틀린 표준 라이브러리는 간결하고 명료한 코드를 작성하는데 도움을 주는 확장함수들을 제공
- 확장함수는 람다를 인자로 받아 동작하며 확장함수를 실행하는 주체를 수신자 또는 수신자 객체라고 한다
- 확장함수에는 범위 지정 함수 let, with, run, apply, also가 있다
- 람다식이 제공된 객체에서 범위 지정 함수를 호출하면 임시 범위가 형성되는데, 이 범위에서
  이름없이 객체에 접근할 수 있다
- 이 다섯가지 범위 지정 함수들은 기본적으로 객체에서 코드 블럭을 실행하는 기능을 수행
- let, with, run, apply, also의 주요 차이점
  * 객체가 블록 내에서 사용 가능한지 여부
  * 전체 표현식의 결과
  
## apply 함수

### apply 함수란?

- apply 함수의 람다에 작성된 코드를 수신자 객체에 적용하는 함수
- apply 함수를 사용하면, apply 함수를 호출한 수신자 객체를 구성하기 위해 apply의 람다에
  포함된 수신자 함수들을 연속적으로 호출
- 람다의 실행이 끝나면 구성된 수신자 객체가 반환

### 사용방법

- apply 함수의 선언

```kotlin
fun <T> T.apply(block: T.() -> Unit): T
```

- T라는 객체를 통해서 apply 함수가 호출
- T.()는 람다 리시버
- apply 함수 선언에서 람다 리시버는 수신자 객체의 타입 T를 block 함수의 입력인 T.()로 전달
- apply 함수는 반환값이 수신자 객체 자기 자신
- 그래서 값을 반환하지 않고 수신자 객체의 멤버에서 작동하는 코드 블록에 apply를 사용

```kotlin
// apply 적용 전
private val cal = Calendar.getInstance()
cal.set(YEAR, 1998)
cal.set(MONTH, SEPTEMBER)
cal.set(DAY_OF_MONTH, 4)

// apply 적용 후
private val cal = Calendar.getInstance().apply { 
  set(YEAR, 1998)
  set(MONTH, SEPTEMBER)
  set(DAY_OF_MONTH, 4)
}
```

### apply 함수가 주로 사용되는 경우

- 우리가 사용할 객체를 구성할 때 반복되는 코드 양을 줄일 경우
- 특정 객체를 생성함과 동시에 초기화 할 경우

## let 함수

### let 함수란?

- let 함수를 호출하는 수신자 객체를 블록의 인자로 넘기고, 블록의 결과값을 반환하는 함수

### let 함수를 사용하는 방법

- let 함수의 선언

```kotlin
fun <T, R> T.let(block: (T) -> R) : R
```

- T라는 객체를 통해서 let 함수가 호출
- let 함수를 호출한 자기자신, 수신자 객체 T를 받아서 람다 실행의 결과값인 R을 반환하는 block을 입력으로 받는다

```kotlin
// let 사용 전
val file = File(path)
if (!file.exists()){
    throw FileNotFoundException(file.toString())
}
val metadata = loadModuleMetadata(file)

// let 사용 후
File(path).let {
    if (!it.exists()) {
        throw FileNotFoundException(it.toString())
    }
  val metadata = loadModuleMetadata(it)
}

// message가 null이 아닌 경우에만 let 함수 호출
message?.let{
    showToast(message)
}
```

### let 함수가 주로 사용되는 경우

- 호출 체인 결과에서 하나 이상의 함수를 호출하는 경우
- null 체크 후 코드를 실행할 경우 (지정된 값이 null이 아닌 경우에 코드 실행)
- 단일 지역 변수의 범위를 제한할 경우
- 블록 내의 결과물을 반환할 경우

## with 함수

### with 함수란?

- with 함수는 비 확장함수
- context 객체를 인자로 직접 전달 받고, 람다 내부에서 이를 수신자로 사용
- run 함수와 유사하지만 리시버로 전달할 객체가 어디있는지 다르며 Null 안정성 안전한 호출(?.)을 지원하지 않는다

### with 함수를 사용하는 방법

- with 함수의 선언

```kotlin
fun <T, R> with(receiver: T, block: T.() -> R): R
```

- 객체를 직접 입력 받고, 객체를 사용하기 위한 block 함수를 두 번째 매개변수로 받는다
- 람다 리시버는 첫 번째 매개변수로 받은 receiver의 타입을 block 함수의 입력인 T.()로 전달
- 그러면 block 함수에서는 receiver로 받은 객체에 this를 사용하지 않고 직접 접근할 수 있다
- with 함수는 block 함수의 반환값을 그대로 반환
- 람다 결과값을 제공하지 않고 컨텍스트 객체에서 함수를 호출하는 것이 좋다

```kotlin
// with 사용 전
val person: Person = getPerson()
print(person.name)
print(person.age)

// with 사용 후
val person: Person = getPerson()
with(person){ 
  print(name)
  print(age)
}
```

### with 함수가 주로 사용되는 경우

- Non-Nullable 객체이어야하는 경우
- 결과 값이 필요하지 않은 어떤 동작을 해야할 경우

## run 함수

### run 함수란?

- 어떤 값을 계산할 필요가 있거나 여러개의 지역 변수의 범위를 제한하기 위해 사용하는 함수
- apply 함수와 유사하나 apply 함수는 객체를 생성함과 동시에 연속된 작업을 수행하고자 할 때 사용하고
  run 함수는 이미 생성된 객체의 메서드나 필드를 연속적으로 호출할 때 사용
  
### run 함수를 사용하는 방법

- run 함수의 선언

```kotlin
fun <T, R> T.run(block: T.() -> R): R
```

```kotlin
// run 사용 전
val person: Person = getPerson()
val personDao: PersonDao = getPersonDao()
val inserted: Boolean = personDao.insert(person)

fun printPerson(person: Person) { 
  print(person.name)
  print(person.age)
}

// run 사용 후
val insert: Boolean = run {
  val person: Person = getPerson()
  val personDao: PersonDao = getPersonDao()
  
  personDao.insert(person)
}

fun printPerson(person: Person) = person.run {
  print(name)
  print(age)
}
```

### run 함수가 주로 사용되는 경우

- 람다에 객체 초기화와 반환 값 계산이 모두 포함될 경우
- 이미 생성된 객체의 메서드나 필드를 연속적으로 호출할 경우

## also 함수

### also 함수란?

- 수신 객체 람다가 전달된 수신 객체를 전혀 사용하지 않거나 수신 객체의 속성을 변경하지 않고
  사용하는 경우 also를 사용
  
### also 함수를 사용하는 방법

- also 함수의 선언

```kotlin
fun <T> T.also(block: (T) -> Unit): T
```

```kotlin
// also 사용 전
class Book(val author: Person) {
    init {
      requireNotNull(author.age)
      print(author.name)
    }
}

// also 사용 후
class Book(author: Person) {
    val author = author.also {
      requireNotNull(it.age)
      print(it.name)
    }
}
```

### also 함수가 주로 사용되는 경우

- 디버그 정보 로깅 또는 인쇄와 같이 오브젝트를 변경하지 않는 추가 조치에 사용되는 경우

### [참고]
<https://salix97.tistory.com/224>
<https://kimdabang.tistory.com/entry/%EC%BD%94%ED%8B%80%EB%A6%B0-%EB%B2%94%EC%9C%84-%EC%A7%80%EC%A0%95-%ED%95%A8%EC%88%98-let-apply-with-run-also?category=907875>
<https://learnrecord.tistory.com/8>