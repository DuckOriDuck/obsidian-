## Stack
- LIFO 자료구조이다.

| 리턴 타입 | 메소드          | 설명                                   |
| ----- | ------------ | ------------------------------------ |
| E     | push(E item) | 주어진 객체를 스택에 넣는다                      |
| E     | peek()       | 스택의 맨 위 객체를 가져온다. 객체를 스택에서 제거하지 않는다. |
| E     | pop()        | 스택의 맨 위 객체를 가져오고, 객체를 스택에서 제거한다.     |
|       |              |                                      |

```java
Stack<E> stack = new Stack<E>();
```

## queue
- FIFO 자료형이다.

| 리턴 타입   | 메소드        | 설명                             |
| ------- | ---------- | ------------------------------ |
| boolean | offer(E e) | 주어진 객체를 넣는다.                   |
| E       | peek()     | 객체 하나를 가져온다. 객체를 큐에서 제거하지 않는다. |
| E       | poll()     | 객체 하나를 가져온다. 객체를 큐에서 제거한다.     |

```java
Queue<E> queue = new LinkedList<E>();
```

## PriorityQueue
- `add()` : 우선순위 큐에 원소를 추가. 큐가 꽉 찬 경우 에러 발생
- `offer()` : 우선순위 큐에 원소를 추가. 값 추가 실패 시 false를 반환
- `poll()` : 우선순위 큐에서 첫 번째 값을 반환하고 제거, 비어있으면 null 반환
- `remove()` : 우선순위 큐에서 첫 번째 값을 반환하고 제거, 비어있으면 에러 발생
- `isEmpty()` : 우선순위 큐에서 첫 번째 값을 반환하고 제거, 비어있으면 에러 발생
- `clear()` : 우선순위 큐를 초기화
- `size()` : 우선순위 큐에 포함되어 있는 원소의 수를 반환
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
```
객체 우선순위 정하는 법:
1. Comparator 인터페이스를
```java
Comparator<Person> ageComparator = (p1, p2) -> Integer.compare(p1.age, p2.age);
PriorityQueue<Person> pq = new PriorityQueue<>(ageComparator);
```
이런식으로 한다.