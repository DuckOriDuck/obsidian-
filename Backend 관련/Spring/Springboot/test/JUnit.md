## 스프링부트 3과 테스트
spring-boot-starter-test 스타터에에서는 테스트를 위한 여러가지 도구들을 지원한다.
스프링 부트 스타터 테스트 목록
- JUnit : 자바 프로그래밍 언어용 단위 테스트 프레임워크
   - Spring Test & Spring Boot Test : 스프링 부트 애플리케이션을 위한 통합 테스트 지
   - AssertJ : 검증문인 assertion을 작성하는 데 사용하는 라이브러리
   - Hamcrest : 표현식을 이해하기 쉽게 만드는 데 사용되는 Matcher 라이브러리
   - Mockito : 테스트에 사용할 가짜 객체인 목 객체를 쉽게 만들고, 관리하고, 검증할 수 있게 지원하는 테스트 프레임워크
   - JSONassert : JSON용 assertion 라이브러리
   - JsonPath : JSON 데이터에서 특정 데이터를 선택하고 검색하기 위한 라이브러리
등이 있다고 한다.

# JUnit?
Junit은 자바 언어를 위한 단위 테스트 프레임워크.
	- 단위란? 작성한 코드가 의도대로 동작하는지 작은 단위로 검증하는 것.
## JUnit의 특징:
- 테스트 방식을 구분할 수 있는 어노테이션 제공.
	- @test 어노테이션으로, 메소드를 호출할 때 마다 새 인스턴스를 생성하고, 독립테스트가 가능하다.
- 예상 결과를 검증하는 **Asertion**메소드 제공
- 사용 방법이 단순, 테스트 코드 작성 시간이 적음
- 자동 실행, 자체 결과를 확인하고 즉각적인 피드백을 제공.

## JUnit으로 단위 테스트 코드 만들기
```java
public class JUnitTest {
		@DisplayName("1 + 2는 3이다")  // 테스트 이름
		@Test
		public void test() {
				int a = 1;
				int b = 2;
				int sum = 3;
		
				Assertions.assertEquals(sum, a + b);  // 값이 같은지 검증
		}
}
```
- @DisplayName()은 티스트의 이름을 명시한다.
- @Test 어노테이션을 붙인 메소드는 테스트를 수행하는 메소드가 된다.
	- JUnit은 테스트끼리 영향을 주지 않도록 각 테스트를 실행할 때 마다 테스트를 위한 실행 객체를 만들고, 종료되면 실행 객체를 삭제한다.
- \Assertions.assertEquals() 를 통해 a+b와 num의 값이 같은지 확인한다.
	- 첫 번째 인수에는 기댓값, 두 번째에는 실제로 검증할 값을 넣는다.
#### 성공 시
![[Pasted image 20240321005135.png]]
#### 실패 시
![[Pasted image 20240321005146.png]]
기대값과 실제값의 차이를 알려준다.

## 자주 사용하는 JUnit 어노테이션
```java
import org.junit.jupiter.api.*;

public class JUnitTotalTest {		
		@BeforeAll			// 전체 테스트를 시작하기 전 1회 실행
		static public void beforeAll() {
				System.out.println("@BeforeAll");
		}
		
		@BeforeEach			// 각각의 테스트 케이스를 실행하기 전마다 실행
		public void beforeEach() {
				System.out.println("@BeforeEach");
		}
		
		@Test
		public void test1() {
				System.out.println("test1");
		}
	
		@Test
		public void test2() {
				System.out.println("test2");
		}
	
		@Test
		public void test3() {
				System.out.println("test3");
		}
		
		@AfterAll			// 전체 테스트를 마치고 종료하기 전에 1회 실행
		static public void afterAll() {
				System.out.println("@AfterAll");
		}
		
		@AfterEach			// 각각의 테스트 케이스를 종료하기 전마다 실행
		public void afterEach() {
				System.out.println("@AfterEach");
		}
}
```
- @BeforeAll
	- 전체 테스트를 실행하기 전 한 번만 실행된다. 데이터베이스에 연결해야하거나, 테스트 환경을 초기화 할 때 사용한다.
	- 한 번만 실행해야 하므로 static이어야 한다.
- @BeforeEach
	- 테스트 케이스를 시작하기 전에 매번 실행한다.
- @AfterAll
	- 전체 테스트를 마치고 종료하기 전에 한 번만 실행된다.
	- 한 번만 실행해야 하므로 static이어야 한다.
- @AfterEach
	- 각 테스트 케이스를 종료하기 전 매번 실행된다.
	- 테스트케이스 이후 특정 데이터를 삭제해야 하는 경우 사용한다.
#### flow를 보면 이렇다.
![[Pasted image 20240321010102.png]]
## AssertJ로 검증문 가독성 높이기.
AssertJ는 JUnit과 함꼐 사용해 검증문의 가독성을 확 높여주는 라이브러리이다.
위에서 사용했던
```java
Assertions.assertEquals(sum, a + b);
```
는 기댓값과 비교값이 어떤것인지 명시해주지 않아 헷갈리는 단점이 있는 반면에,
AssertJ를 사용하면
```java
assertThat(a + b).isEqualTo(sum);
```
이 경우 a와 b를 더한 값이 sum과 같아야 한다는 의미로 명확하게 읽힌다.
## 이 외 AssertJ에서 지원해주는 다양한 메소드

|메소드 이름|설명|
|---|---|
|isEqualTo(A)|A 값과 같은지 검증|
|isNotEqualTo(A)|A 값과 다른지 검증|
|contains(A)|A 값을 포함하는지 검증|
|doesNotContains(A)|A 값을 포함하지 않는지 검증|
|startsWith(A)|A 값으로 시작하는지 검증|
|endsWith(A)|A 값으로 끝나는지 검증|
|isEmpty()|비어있는 값인지 검증|
|isNotEmpty()|비어있지 않은 값인지 검증|
|isPositive()|양수인지 검증|
|isNegative()|음수인지 검증|
|isGreaterThan(1)|1보다 큰 값인지 검증|
|isLessThan(1)|1보다 작은 값인지 검증|
