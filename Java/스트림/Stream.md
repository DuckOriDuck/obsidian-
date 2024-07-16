# Stream?
- 데이터의 흐름. 배열이나 컬렉션을 가공하여 원하는 결과를 얻을 수 있음.
- 자바에서 지공해주는 Collection 인터페이스의 메서드로 스트림을 제공해준다.
- 반복문 대신 함수형으로 처리한느데, 데이터 처리의 과정을 filter, foreach와 같이 사람이 읽기 쉬운 단어로 나타내어 가독성이 높아짐. 람다도 스트림 활용 가능

## 스트림의 3가지 부분 
- 생성 : 스트림 인터페이스 생성
- 가공: 원하는 결과를 ㄴ만들어가는 중간작업
- 결과: 최종 결과를 만들어내는 작업
## 스트림의 종류
- ### BaseStream
	- Stream: 객체 요소를 처리
	- IntStream: int 형을 처리
	- LongStream : Long 형을 처리
	- DoubleStream: double 형을 처리
## 스트림 만드는 방법:
컬렉션 타입으로 만들기:
```java
List<String> list = Arrays.asList("a", "b", "c", "d", "e");

**Stream<String> stream = list.stream();**
stream.forEach(System.out::println);
```
배열로부터 생성:
```java
String[] arr = new String[]{"a", "b", "c", "d", "e"};

**Stream<String> stream = Arrays.stream(arr);**
stream.forEach(System.out::println);
```
숫자 범위로부터 스트림 생성
```java
IntStream intStream = IntStream.range(1, 5);	// [1, 2, 3, 4]
LongStream longStream = LongStream.rangeClosed(1, 5);	// [1, 2, 3, 4, 5]
```
난수도 가능
```java
DoubleStream doubleStream = new Random().doubles(5); // 난수 5개 생성
```
파일로부터 생성
```java
// File 클래스
Stream<String> fileStream = Files.lines(Paths.get("file.txt"), Charset.forName("UTF-8"));
fileStream.forEach(System.out::println);

// BufferedReader 클래스
FileReader fileReader = new FileReader(Paths.get("file.txt").toFile());
BufferedReader br = new BufferedReader(fileReader);
stream = br.lines();
stream.forEach(System.out::println);
```
스트림 연결
```java
Stream<Integer> stream1 = Stream.of(1, 2, 3);
Stream<Integer> stream2 = Stream.of(4, 5, 6);

Stream<Integer> newStream = Stream.concat(stream1, stream2);
newStream.forEach(System.out::println); // 1, 2, 3, 4, 5, 6
```
