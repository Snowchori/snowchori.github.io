---
layout: post
title:  "ArrayList LinkedList"
date:   2021-11-28 15:00:00 +0900
category: DataStructure
---

# ArrayList LinkedList

## ArrayList

### ArrayList란?

- ArrayList는 중복을 허용하고 순서를 유지하며 인덱스로 원소들을 관리한다는 점에서 배열과 상당히 유사
- 배열은 크기가 지정되면 고정되지만 ArrayList는 클래스이기 때문에 배열을 추가, 삭제 할 수 있는 메소드들도 존재
- 추가했을 때 배열이 동적으로 늘어나는 것이 아니라 용량이 꽉 찼을 경우 더 큰 용량의 배열을 만들어 옮기는 작업을 하게 된다

### 구조

<img src="http://snowchori.github.io/assets/img/ArrayList.png">

- ArrayList는 내부적으로 배열의 형태

### get / set

- 배열의 index를 통해 접근하는 방식이기 때문에 random access 속도가 빠르며
  get / set 메소드는 상수 시간을 가지게 된다
  
### add

- ArrayList는 배열이기 때문에 중간에 값을 끼워넣는 연산이 불가능
- 새로운 값을 추가하려고 할 때 List의 크기가 생성되어 있는 배열의 size보다 커지게 되면
  이전 크기의 2배가 되는 배열을 생성해 배열 전체를 복사하여 새로운 배열에 복사하고 제일 뒤에 값을 추가
- 기존에 있던 배열에서 추가하고 싶은 index부터 마지막 index까지 한 칸씩 뒤로 미루는 연산이 필요
- 해당하는 인덱스를 찾아가는 시간(O(1)) + 배열을 복사하는 시간(O(n)) = O(n)의 시간이 소요

### remove

- add와 유사하게 remove는 삭제된 index + 1 부터 마지막 index까지 한 칸씩 앞으로 당기는 연산을 하게 된다
- add와 동일한 O(n)의 시간 복잡도를 가지게 된다

ArrayList는 탐색은 빠르게 할 수 있지만 중간에서 추가, 삭제가 빈번하게 일어나면 비효율적인 특징을 가지고 있다

## LinkedList

### LinkedList란?

- LinkedList는 내부적으로 양방향의 연결 리스트로 구성되어있어 참조하려는 원소에 따라 처음부터 순방향으로
  또는 역순으로 순회할 수 있다
  
### 배열의 단점

- 크기를 변경할 수 없다
  * 크기를 변경할 수 없으므로 새로운 배열을 생성해서 복사해야한다
  * 실행속도를 향상시키기 위해서는 충분히 큰 용량을 미리 정해놔야 하는데 이것이 메모리 낭비가 될 수 있다
- 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다
  * 차례대로 데이터를 추가하고 마지막에서부터 데이터를 삭제하는 것은 빠르다
  * 배열의 중간에 데이터를 추가하거나 삭제하면 빈공간을 만들기 위해 데이터 이동이 필요하고
    빈공간을 채우기 위해 데이터 이동이 빈번할 것이다
    
### 구조

<img src="http://snowchori.github.io/assets/img/LinkedList.png">

### get / set

- LinkedList는 연결 리스트 형태를 띄고 있기 때문에 해당하는 index에 대한 값을 얻어올 때
  시작이나 끝에서부터 해당 index까지 순차적으로 접근하여 값을 얻어온다
- 시간 복잡도는 O(n)을 가진다

### add

- 일반적인 경우
  * 추가를 원하는 index에 도달할 때까지 순차 접근을 하는 시간복잡도 O(n)
  * index-1의 노드의 next와 index+1의 prev를 새로 추가한 노드에 연결하는 작업은 n에 영향을 받지않는
    상수 시간이기 때문에 O(1)이다
  * 시간 복잡도는 O(n)을 가진다
- 시작이나 끝에 요소를 추가할 때
  * LinkedList는 head와 tail을 갖는 doubleLinkedList의 구조이기 때문에 시작과 끝에
    해당하는 노드를 찾아가는데는 O(1)이라는 시간 복잡도를 갖게 된다
    
### remove

- 일반적인 경우
  * 원하는 index에 도달할 때까지 순차 접근을 하는 시간복잡도 O(n)
  * index-1의 노드의 next를 index의 next의 요소와 연결하고 index+1의 prev를 index의
    prev요소로 변경하면 된다
  * List의 길이에 영향을 받지않는 상수 시간이기 때문에 O(1)이다
  * 시간복잡도는 O(n)을 가진다
- 시작이나 끝에 요소를 삭제할 때
  * 시작과 끝 요소를 찾아가는데 O(1)이라는 시간 복잡도를 갖기 때문에 시간복잡도는 O(1)이다
  
## 차이점

- 순차적으로 추가/삭제하는 경우에는 ArrayList가 LinkedList보다 빠르다
- 중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다

## 시간 복잡도

| |ArrayList|LinkedList|
|:---:|:---:|:---:|
|get/set|O(1)|O(n)|
|add(시작)|O(n)|O(1)|
|add(끝)|O(1)|O(1)|
|add(일반)|O(n)|O(n)|
|remove(시작)|O(n)|O(1)|
|remove(끝)|O(1)|O(1)|
|remove(일반)|O(n)|O(n)|

### [참고]
<https://devlog-wjdrbs96.tistory.com/64> <br>
<https://girawhale.tistory.com/8>