---
title: 7. stack, queue, deque
date: 2025-05-01 06:13:12 +09:00
last_modified_at: 2025-05-01 09:11:23 +09:00
categories: [algorithm, theory]
tags:
  [
    stack,
    queue,
    deque,
    알고리즘
  ]
--- 
# **stack, queue, deque**

사진 출처 : https://www.programiz.com/dsa<br>

스택과 큐, 그리고 이를 확장한 덱은 가장 기본적인 자료구조로 널리 사용된다.<br>
자료구조 수업에서 보통 가장 먼저 배우는 것이기도 하고.<br>

## 스택 (stack)
![image](/assets/img/algorithm/stack_1.PNG)<br>

스택이란 먼저 입력한 데이터가 가장 아래에 위치하고, 최근에 입력한 데이터가 가장 위에 있는 구조를 가진다.<br>
이를 후입선출 구조라고 하고 한 줄로 되어있는 긴 주차장이라고 생각할 수 있다.<br>
가장 먼저 들어온 차를 빼려면 뒤로 들어온 차들을 전부 빼야만 한다.<br>

이렇게 데이터를 삽입하는 연산을 push, 꺼내는 연산을 pop이라고 한다.<br>

![image](/assets/img/algorithm/stack_2.PNG)<br>

스택의 구조를 조금 더 보면 맨 처음에는 그림처럼 빈 공간이 있고, 스택 포인터(값을 넣을 주소)가 가장 밑을 가리킨다.<br>
그 다음 push를 통해 data를 넣어주면 포인터가 있던 자리에 값이 들어가고, 한 칸 올라간다.<br>

![image](/assets/img/algorithm/stack_3.PNG)<br>

반대로 pop을 하면 data를 빼고, 스택 포인터를 한 칸 내려준다.<br>
이렇게 pop과 push는 포인터가 가리키는 곳에서 일어나기 때문에 시간 복잡도가 O(1)이 보장이 되고<br>
주로 마지막에 한 일을 되돌리는 작업이 자주 필요한 곳에서 사용된다.<br>

함수 호출 구조, ctrl+z, 뒤로 가기, DFS 등을 예시로 들 수 있다.<br>

## 큐 (queue)
![image](/assets/img/algorithm/queue_1.PNG)<br>

큐는 먼저 입력한 데이터가 가장 앞에 위치하고, 최근에 입력한 데이터가 가장 뒤에 쌓이는 구조를 가진다.<br>
이를 선입선출 구조라고 하며, 표를 사려고 기다리는 매표소 줄과 비슷하다고 볼 수 있다.<br>

이렇게 데이터를 삽입하는 연산을 enqueue, 꺼내는 연산을 dequeue라고 한다.<br>

![image](/assets/img/algorithm/queue_2.PNG)<br>

큐의 구조를 조금 더 보면, 처음에는 그림처럼 비어 있고, front 포인터와 rear 포인터가 동일한 위치를 가리킨다.<br>
enqueue를 통해 데이터를 추가하면, rear가 가리키는 자리에 값이 들어가고 rear 포인터가 한 칸 뒤로 이동한다.

![image](/assets/img/algorithm/queue_3.PNG)<br>

반대로 dequeue 연산을 하면 front가 가리키는 값을 꺼내고, front 포인터가 한 칸 뒤로 이동한다.<br>
이처럼 enqueue와 dequeue는 포인터 위치에서 일정하게 작동하므로 시간 복잡도는 O(1)이며<br>
순서대로 데이터를 처리해야 하는 상황에서 큐가 많이 사용된다.<br>

예시로는 프린터 대기열, BFS, CPU 작업 스케줄링, 고객 대기 줄 처리 등이 있다.

이 때 rear는 값이 들어올 때 뒤로 이동하고, front도 값이 빠질 때 뒤로 이동하기 때문에 enque와 deque가 반복되면<br>
두 포인터는 점점 저장 공간의 끝으로 이동하게 되고, front 앞의 빈 공간들은 쓰지 못하게 된다.<br>

### 원형 큐
앞쪽의 빈 공간을 재활용할 수 있도록 rear와 front가 다시 처음으로 돌아가 순환하도록 만든 구조인 원형 큐가 있다.<br>
rear이나 front가 끝에서 이동을 할 때, 다시 처음으로 돌아와 처음과 끝을 순환하도록 만든 것이다.<br>
당연하지만 실제로 원형으로 구조가 생긴 것은 아니고 코드 상으로 마지막에서 ++가 될 때 0으로 이동하도록 구현한다.<br>

![image](/assets/img/algorithm/queue_4.PNG)<br>

이러면 낭비되는 공간 없이 전부 사용할 수 있다.<br>

## 덱 (deque)
![image](/assets/img/algorithm/deque_1.PNG)<br>

덱은 큐를 확장한 개념으로, 양쪽 끝에서 모두 삽입과 삭제가 가능한 자료구조다.<br>
앞에서도 데이터를 넣거나 뺄 수 있고, 뒤에서도 데이터를 넣거나 뺄 수 있기 때문에 유연하게 스택과 큐의 장점을 사용할 수 있다.<br>
- push_front(): 앞쪽에 데이터 삽입
- push_back(): 뒤쪽에 데이터 삽입
- pop_front(): 앞쪽에서 데이터 삭제
- pop_back(): 뒤쪽에서 데이터 삭제

이렇게 네가지 연산을 사용하고 슬라이딩 윈도우, 캐시 구현(LRU) 등에 사용 할 수 있다.

그렇다면 생기는 의문이 어째서 덱으로 다 통합해서 쓰면 되지 왜 스택과 큐를 구분지어서 사용하느냐이다.<br>
이것은 코드를 짜기만 하는 입장에서는 편할 수도 있지만 가독성, 의사 표현 면에서 좋지 않다.<br>

위에서 봤듯이 스택은 **마지막 것을 꺼내기**, 큐는 **먼저 들어온 것을 처리**에 특화되어 있고, 그럴 때 사용한다.<br>
그런데 그것을 전부 덱으로 사용한다면 push_front, pop_front.. 등으로 코드가 복잡해지고, 의도가 명확해지지 않는다.<br>
또한 여러가지 기능을 지원한다는 것은 한 가지만 지원하는 것에 비해 조금이라도 무거워지기 마련이다.<br>
그렇기에 덱은 범용적이지만 과하게 느껴질 수 있어 스택과 큐를 따로 사용하는 것이라고 생각하면 될 것 같다.

### 주의할 점?
> 구현 방식에 따라 다르긴 하지만, 대부분 pop이나 dequeue 같은 연산은 실제로 데이터를 삭제하지는 않는다.<br>
> front나 top 포인터만 이동시켜 다시 보지 않게 해서 **삭제된 것처럼 동작**하게 만든다.<br>
> 이는 시간 복잡도를 O(1)로 유지하기 위한 설계이며, 실제로 메모리에서 데이터가 제거되는 것은 아니다.<br>
> 따라서 큐나 스택을 반복해서 사용할 때는, 잔여 데이터가 남아있을 수 있다는 점에 유의해야 한다.