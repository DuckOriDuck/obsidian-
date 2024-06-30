# 컬렉션?
- **데이터를 그룹으로 저장하고 관리하는데 필요한 인터페이스와 클래스들의 집합이다**
- List, Set, Queue, Map 인터페이스와 이를 구현하는 클래스들로 구성됨.
---

# List
- 순서가 있는 요소의 집합으로 중복을 허용.
- 배열과 다르게 계속해서 자료형의 개수가 달라지고 할당된 공간이 달라지는 상황에서 유리함

## 공통으로 사용하는 List 인터페이스 메소드이다.

| 기능    | 메소드                            | 설명                          |
| ----- | ------------------------------ | --------------------------- |
| 객체 추가 | boolean add(E e)               | 주어진 객체를 맨 끝에 추가             |
|       | void add(int index, E element) | 주어진 인덱스에 객체를 추가             |
|       | set(int index, E element)      | 주어진 인덱스에 저장된 객체를 주어진 객체로 바꿈 |
| 객체 검색 | boolean contains(Object o)     | 주어진 객체가 저장되어있는지 여부          |
|       | E get(int index)               | 주어진 인덱스에 저장된 객체를 리턴         |
|       | isEmpty()                      | 컬렉션이 비어있는지 여부               |
|       | int size()                     | 저장되어 있는 전체 객체 수 리턴          |
| 객체 삭제 | void clear()                   | 저장된 모든 객체를 삭제               |
|       | E remove(int index)            | 주어진 인덱스에 저장된 객체를 삭제         |
|       | boolean remove(Object o)       | 주어진 객체를 삭제(리스트에서 발견되는 첫 번째) |

## ArrayList
- List 인터페이스의 구현클래스. ArrayList에 객체를 추가하게되면 객체가 인덱스로 관리됨.
- 용량을 유동적으로 바꿀 수 있어 배열보다 사용하는데 있어서 편함
```Java
List<String> list = new ArrayList<String>(30);  //String객체 30개를 저장할 수 있는 용량
```
- 이렇게 선언하고 사용한다.
- 만약 중간의 값이 사라진다면 바로 뒤 인덱스부터 모두 앞으로 1씩 달라짐. 그래서 삭제와 추가가 빈번한 상황에는 ArrayList가 아닌 LinkedList를 사용하는것이 바람직하다.
## LinkedList
- ArrayList와 다르게 인접 참조를 링크해서 체인처럼 관리한다.
- 중간의 객체를 제거하거나 삽입할 때 요소들의 연결을 수정해주면 되다보니 ArrayList보다 객체의 삽입과 삭제가 빠르다.
- 순차적으로 요소를 따라가야하기 때문에 ArrayList보다 요소를 찾는 시간이 느리다.
```java
List<E> list = new LinkedList<E>();
```
---
## Vector
- 동기화가 필요하다면 vector를 쓰면 된다. 사용법은 ArrayList와 동일하다.
- 