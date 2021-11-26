---
layout: post
title:  "LayoutInflater"
date:   2021-11-26 20:00:00 +0900
category: Android
---

# LayoutInflater

## LayoutInflater란?

- 안드로이드에서 View를 만드는 방법 중 하나
- xml에 정의된 Resource를 View 객체로 반환해주는 역할을 한다
- xml에 미리 정해둔 틀을 실제 메모리에 올려주는 역할을 한다
- Activity를 만들면 onCreate에 추가되는 setContentView 메서드와 유사한 역할
- xml 레이아웃 파일에 대한 뷰를 생성할 때 LayoutInflater를 이용해야 한다
- LayoutInflater 객체의 inflate 메서드를 이용해 새로운 뷰를 생성할 수 있다
- root를 지정하지 않을 경우, xml 상의 최상위 뷰의 xml layout 설정들은 무시된다

## LayoutInflater 생성

### Context#getSystemService()

```kotlin
val inflater : LayoutInflater = context.getSystemService(Context.LAYOUT_INFLATER_SERVICE)
```

- context에서 LayoutInflater를 가져온다

### Activity#getLayoutInflater()

```kotlin
val inflater : LayoutInflater = getLayoutInflater()
```

- activity에서 LayoutInflater를 얻어온다
- Activity는 자신의 window의 LayoutInflater를 사용

### LayoutInflater.from()

```kotlin
val inflater : LayoutInflater = LayoutInflater.from(context)
```

- LayoutInflater에 static으로 정의되어있는 LayoutInflater.from()을 이용해 LayoutInflater를 만든다
- 내부적으로 context에서 LayoutInflater를 가져온다

## LayoutInflater 사용시 주의사항

- inflate() 메서드로 layout을 inflate한 경우, 해당 xml의 land, port layout을 자동으로 참조하게 된다
- inflate()된 view의 child view는 inflate된 view.findViewById로 찾아야 한다
- inflate()된 view의 layoutParams 속성은 실제 layout에서 match_parent라도, wrap_content로 갖에 변경된다
- inflate된 뷰에서 다시 layout inflater를 사용할 경우, 기존의 findViewById와 event 설정들이 모두 사라진다

## View inflate하기

inflater에서 View 객체를 만들기 위해서는 inflater를 사용하면 된다

```kotlin
inflate(resource: Int, root: ViewGroup?, attachToRoot: Boolean)
```

- resource : View를 만들고싶은 레이아웃 파일의 id
- root : 생성될 View의 parent를 명시, null일 경우에는 LayoutParam 값을 설정 할 수 없기 때문에
  XML내의 최상위 android:layout_xxxxx 값들이 무시되고 merge tag를 사용할 수 없다
- attachToRoot : true로 설정해 줄 경우 root의 자식 view로 자동으로 추가된다 이 때 root는 null일 수 없다
- return : attachToRoot에 따라서 리턴 값이 달라진다 true일 경우 root가 false일 경우 XML내 최상위 뷰가 리턴된다

### [참고]
<https://yejinson97gaegul.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-Android-LayoutInflater> <br>
<https://medium.com/vingle-tech-blog/android-layoutinflater-b6e44c265408>