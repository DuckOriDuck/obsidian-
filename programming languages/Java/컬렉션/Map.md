# Map
- key, value로 구성된 객체를 저장하는 구조임.
- 키와 values는 모두 구조임. key는 중복이 안되지만 value는 중복저장 가능.
- 기존에 저장됐던 키값과 동일한 키값으로 저장되면 기존의 값은 없어지고 새로운 값으로 대치됨.

## Map 인터페이스 메소드.

| 기능   | 메소드                                 | 설명                                         |
| ---- | ----------------------------------- | ------------------------------------------ |
| 객체추가 | V put(K key, V value)               | 주어진 키와 값을 추가, 저장되면 값을 리턴                   |
| 객체검색 | boolean containsKey(Object key)     | 주어진 키가 있는지 여부                              |
|      | boolean containsValue(Object value) | 주어진 값이 있는지 여부                              |
|      | Set(Map.Entry<K,V>> entrySet()      | 키와 값의 쌍으로 구성된 모든 Map.Entry 객체를 Set에 담아서 리턴 |
|      | V get(Object key)                   | 주어진 키가 있는 값을 리턴                            |
|      | boolean isEmpty()                   | 컬렉션이 비어 있는지 여부                             |
|      | `Set<K> keySet()`                   | 모든 키를 Set 객체에 담아서 리턴                       |
|      | int size()                          | 저장된 키의 총 개수 리턴                             |
|      | `Collection<V> values()`            | 저장된 모든 값을 Collection에 담아서 리턴               |
| 객체삭제 | vold clear()                        | 모든 Map.Entry(키와 값)를 삭제                     |
|      | V remove(Object key)                | 주어진 키와 일치하는 Map.Entry를 삭제하고 값을 리턴          |
## HashMap
- 해시 테이블을 사용하여 key-value 쌍을 저장함
- 요소의 삽입, 삭제, 탐색이 평균적으로 O(1)임
- 요소의 저장 순서를 보장하지 않는다
- 해시 충돌이 날 떄는 Seperate chaining을 사용한다. 
	- 해시 충돌이 적으면 linked list를 사용하여 한 버킷 내에 데이터들을 관리. 
	- 한 버킷에 해시 충돌이 너무 많아지면 red black tree 자료구조를 사용한다.
```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Apple", 1);
hashMap.put("Banana", 2);
hashMap.put("Orange", 3);
System.out.println(hashMap); // 순서는 보장되지 않음

```

## HashTable
- 해시 테이블을 사용하여 값을 저장한다.
- Thread Safe: HashMap과 유사하지만 synchronized 메소드로 구성돼있어 멀티스레드가 동시에 이 메소드를 실행할 수 없고, 하나의 스레드가 실행을 완료해야 다른 스레드도 실행할 수 있다. 그래서 멀티스레드 환경에서 안전하게 객체를 추가/삭제할 수 있는 특징을 가짐.
- 동기화 함수의 오버헤드로 인해 속도가 HashMap보다 느림.

```java
Map<String, Integer> hashtable = new Hashtable<>();
hashtable.put("Apple", 1);
hashtable.put("Banana", 2);
hashtable.put("Orange", 3);
System.out.println(hashtable); // 순서는 보장되지 않음

```


## LinkedHashMap
- 해시 테이블과 이중 연결 리스트를 사용하여 저장한다.
- hashMap보다 약간 느리지만 여전히 O(1)
- 삽입 순서를 유지한다
```java
Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
linkedHashMap.put("Apple", 1);
linkedHashMap.put("Banana", 2);
linkedHashMap.put("Orange", 3);
System.out.println(linkedHashMap); // [Apple=1, Banana=2, Orange=3]

```
## TreeMap
- red-black 트리를 사용하여 값을 저장한다.
- 요소의 삽입, 삭제, 탐색이 O(log n)이다.
- 키의 자연 순서 또는 제공된 comparator에 따라 정렬된 순서를 유지한다
```java
Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("Banana", 2);
treeMap.put("Apple", 1);
treeMap.put("Orange", 3);
System.out.println(treeMap); // [Apple=1, Banana=2, Orange=3]

```