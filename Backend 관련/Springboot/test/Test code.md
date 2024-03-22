# Test code?
서비스 기능 검증에 필요한 과정이다. 개발자가 작성한 코드가 의도한 대로 잘 동작하고 예쌍치 못한 문제가 없는지를 확인할 목적으로 테스트코드를 작성한다.

## test 디렉토리에 작성한다.
![[Pasted image 20240321001133.png]]
## 테스트 코드에는 given-wehn-then 패턴이 존재한다.
1. given: 테스트 실행을 위해 필요한 데이터를 준비하는 단계
2. when: 테스트를 진행하는 단계
3. then: when에서 수행한 테스트 결과를 검증하는 단계
### 예시:
```java
@DisplayName("새로운 메뉴를 저장한다.")
@Test
public void saveMenuTest() {
		// given : 메뉴를 저장하기 위한 준비 과정
		String name = "카페라떼";
		int price = 5500;
		
		Menu latte = new Menu(name, price);
		 
		// when : 실제로 메뉴를 저장
		long saveId = menuService.save(latte);
		
		// then : 메뉴가 잘 추가되었는지 검증 
		Menu savedMenu = menuService.findById(saveId).get();
		assertThat(savedMenu.getName()).isEqualTo(name);
		assertThat(savedMenu.getPrice()).isEqualTo(price);		
}
```