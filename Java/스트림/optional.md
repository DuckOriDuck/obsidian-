# Optional?
- null을 포함한 모든 데이터를 저장할 수 있는 wrapper 클래스 \
-  NPE를 방지하기 위해 도입
- 데이터를 일단 Optional로 감싸고 봄

이런 경우 스트림에서 NPE 발생
```java
List<Integer> list = null;

list.stream().forEach(System.out::println);   // NPE
```

이렇게 optional로 nullable을 체크하면 에러를 방지 가능
```java
List<Integer> list = null;

Optional.ofNullable(list)
	.orElseGet(Collections::emptyList)
	.forEach(System.out::println);
```

| 메소드                                          | 설명                                                                                        |
| -------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `static <T> Optional<T> empty()`             | 아무런 값도 가지지 않는 비어있는 Optional 객체를 반환함.                                                      |
| `static <T> Optional<T> of(T value)`         | null이 아닌 명시된 값을 가지는 Optional 객체를 반환함.                                                     |
| `static <T> Optional<T> ofNullable(T value)` | 명시된 값 != null ->명시된 값을 가지는 Optional 객체를 반환하며, <br>명시된 값 == null -> 비어있는 Optional 객체를 반환함. |

## Optional 객체 반환
get()으로만 객체를 반환받으면 그 객체가 null일때 NPE가 발생해서 orElse(), orElseGet() 메소드를 사용해서 null 값을 처리해준ㄷ 
T get() : Optional 객체에 저장된 값을 반환함.
T orElse(T other) : 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 값을 반환함.
`T orElseGet(Supplier<? extends T> other)`: 저장된 값  존재 -> 그 값을 반환, 값이 존재x -> 인수로 전달된 람다 표현식의 결과값을 반환.
`<X extends Throwable> T orElseThrow(Supplier<? extends X>  exceptionSupplier)`:  저장된 값  존재 -> 그 값을 반환, 값이 존재x -> 인수로 전달된 예외를 발생.
boolean isPresent() : 저장된 값의 존재 유무를 boolean으로 알려줌