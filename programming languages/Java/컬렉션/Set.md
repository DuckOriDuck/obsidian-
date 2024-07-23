# Set
- set 컬렉션은 list와 다르게 저장 순서가 유지되지 않고, 객체를 중복해서 저장할 수 없다.
## Set 인터페이스의 메소드이다.

| 기능    | 메소드                        | 설명                                                     |
| ----- | -------------------------- | ------------------------------------------------------ |
| 객체 추가 | boolean add(E e)           | 주어진 객체를 저장, 객체가 성공적으로 저장되면 true를 리턴하고 중복 객체면 false를 리턴 |
| 객체 검색 | boolean contains(Object o) | 주어진 객체가 저장되어 있는지 여부                                    |
|       | isEmpty()                  | 컬렉션이 비어 있는지 조사                                         |
|       | `Iterator<E> iterator()`   | 저장된 객체를 한 번씩 가져오는 반복자 리턴                               |
|       | `int size()`               | 저장되어 있는 전체 객체 수 리턴                                     |
| 객체 삭제 | void clear()               | 저장된 모든 객체를 삭제                                          |
|       | boolean remove(Object o)   | 주어진 객체를 삭제                                             |
## HashSet
- 내부적으로 해시 테이블을 사용하여 요소를 저장한다.
- 요소의 추가, 삭제, 탐색이 평균적으로 O(1)이다.
- 요소의 저장 순서를 일반적으로 보장하지 않는다.
```java
Set<String> set = new HashSet<String>();
```
하지만
```java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample2 {
	public static void main(String[] args) {
		Set<Member> set = new HashSet<>();

		set.add(new Member("홍길동", 30));
		set.add(new Member("홍길동", 30));

		System.out.println("총 객체수: " + set.size());
	}
}
```
동일한 값을 가진 객체를 2개 hash set에 넣는다면 둘 다 들어가진다. 내부 데이터는 동일한데 인스턴스가 다르기 때문.
이 때는 Member클래스에서 `equals()`와 `hashCode()` 메소드를 오버라이딩해서 인스턴스가 달라도 내부 데이터가 동일하다면 동일한 객체로 간주하도록 코드를 바꿔줘야 한다.
```java
public class Member {
	String name;
	int age;

	public Member(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public int hashCode() {
		return name.hashCode() + age;
	}

	@Override
	public boolean equals(Object obj) {
		if (obj instanceof Member) {
			Member member = (Member)obj;
			return member.name.equals(this.name) && member.age == this.age;
		} else {
			return false;
		}
	}
}
```
## LinkedHashSet
- 해시 테이블과 이중 연결 리스트를 사용하여 요소를 저장한다.
- 이 때문에 HashSet보다 추가,삭제,탐색이 조금 느리다.
- 그런데 요소의 삽입 순서를 유지해준다.
```java
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("Apple");
linkedHashSet.add("Banana");
linkedHashSet.add("Orange");
System.out.println(linkedHashSet); // [Apple, Banana, Orange]

```

## TreeSet
- 레드-블랙 트리로 구현돼있다. [[Tree]] 참고
- 요소의 추가, 삭제, 탐색이 O(log n)이다.
- 요소를 정렬된 순서로 저장한다는 장점이 있다.
```java
Set<String> treeSet = new TreeSet<>();
treeSet.add("Apple");
treeSet.add("Banana");
treeSet.add("Orange");
System.out.println(treeSet); // [Apple, Banana, Orange] (알파벳 순)
```
- C++ 의 STL인 map이나 priority_queue 처럼 정렬 순서를 지정하고 싶을 떄가 있는데, 이땐 이렇게 하면 된다.
#### Comparable 인터페이스 구현
객체가 자연스러운 순서를 가지도록 할 때 사용한다.
```java
import java.util.TreeSet;

class Person implements Comparable<Person> {
    String name;
    int age;
	...
	
    @Override
    public int compareTo(Person other) {
        // 나이 순으로 정렬
        return Integer.compare(this.age, other.age);
    }

	 ...
}
```
이와 같이 compareTo를 오버라이딩 해주면 된다.
#### Comparator 인터페이스 구현
자연스러운 순서가 아닌 별도의 순서를 지정해주고 싶을 때 사용한다
```java
import java.util.Comparator;
import java.util.TreeSet;

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```
person 클래스가 있을때
```java
class NameComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p1.name.compareTo(p2.name);
    }
}

//람다식을 사용할거면 걍 사용하는 부분에서
Comparator<Person> NameComparator = () -> p1.name.compareTo(p2.name);
//이렇게 메인 메소드에서 쓰고 사용하면 됨
```
Comparator를 이식하는 NameComparator 함수를 만들고
```java 
public class Main {
```
```java
    public static void main(String[] args) {
```
e다음과 같이 treeSet의 생성자에 NameComparator 인스턴스를 넣어주면 된다.
```java
        TreeSet<Person> peopleByName = new TreeSet<>(new NameComparator());
        peopleByName.add(new Person("Alice", 30));
        peopleByName.add(new Person("Bob", 25));
        peopleByName.add(new Person("Charlie", 35));

        for (Person person : peopleByName) {
            System.out.println(person);
        }
    }
}
```


