# 관점 지향 프로그래밍이란?
프로그래밍에 대한 관심을 
- 핵심 관심 사항, 
	ex: 회원 가입
		- 인증
		- id/pw 체크
		- ...
- 공통 관심 사항
	ex: 서버 최적화
	- 처리 시간 최적화
으로 나눠서 관심 기준으로 **모듈화** 하는 것을 의미한다.

만약 해야 하는 작업이 
‘회원 관리’ 서비스의 모든 메소드 호출 시간을 측정하여 각 메소드에 로그를 남기는 것

이라면, 존재하는 회원 가입 서비스의 모든 메소드를 찾아 시간을 하나하나 계산하고 그 값을 로그로 남겨야 한다.

## AOP 미적용

이 작업을 핵심 관심 사항과 공통 관심 사항으로 분리해보면
- 핵심 관심 사항: 회원 관리 로직
- 공통 관심사항: 호출 시간 로깅
이렇게 된다.

만약 기존의 방식대로 개발을 했다면 
```java
public class MemberService {

    public Long join(Member member) {  // 회원가입
        long startTimeMs = System.currentTimeMillis();
				try {
						validateDuplicateMember(member); //중복 회원 검증
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finishTimeMs = System.currentTimeMillis();
            long timeMs = finishTimeMs - startTimeMs;
            System.out.println("join " + timeMs + "ms");
				} 
		}

    public List<Member> findMembers() {   // 전체 회원 조회
        long startTimeMs = System.currentTimeMillis();
        try {
            return memberRepository.findAll();
        } finally {
            long finishTimeMs = System.currentTimeMillis();
            long timeMs = finishTimeMs - startTimeMs;
            System.out.println("findMembers " + timeMs + "ms");
			  }
		}
}
```
- 호출시간 로깅 작업과 핵심 비즈니스 로직이 혼재돼있어 유지보수가 어렵다.
- 오출시간을 측정하는 로직이 반복되면서 별도의 공통 로직으로 만들기 어렵다.
- 만약 호출시간을 측정하는 로직을 변경해야할 때,  예를 들어 ms 단위 ->ns 단위 로 바꿔야할 때 모든 코드의 호출시간 계산 로직을 찾아서 바꿔줘야 함.

## AOP 적용
공통 관심사항에 해당하는 호출시간 로깅 로직을 모듈화해서 핵심 관심사항과 분리할 수 있다.
호출 시간을 로깅하는 공통로직을 별도로 만들어서 모듈화 하고, 핵심 로직에 끼워넣는 방식.
- 공통 관심사항(Cross-cutting concern) VS 핵심 관심사항 (core concern) 분리
![[Pasted image 20240305152714.png]]
TimeLoggingAop라는 클래스로 호출 시간을 로깅하는 AOP를 적용

```JAVA
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class TimeLoggingAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
    
        long startTimeMs = System.currentTimeMillis();
        
        System.out.println("START: " + joinPoint.toString());
        
        try {
            return joinPoint.proceed();
        } finally {
        
            long finishTimeMs = System.currentTimeMillis();
            long timeMs = finishTimeMs - startTimeMs;
            System.out.println("END: " + joinPoint.toString()+ " " + timeMs + "ms"); 
				}
		}

}

```
이렇게 AOP를 적용하면 프로그램의 변경과 확장에도 유연하게 대응할 수 있어서 좋다.
장점:
- 핵심 관심사항과 공통 관심사항을 분리하여 추후에 수정사항이 생겼을 때 각각의 로직만 변경하면 됨.
- 공통 관심사항인 호출시간 로깅 로직이 모듈화가 돼있어서 원하는 적용 대상을 선택할 수 있음.
- 핵심 관심사항인 회원관리(가입, 목록조회 등) 로직에만 집중할 수 있어서 코드를 깔끔하게 유지 가능.
## AOP 동작 방식
AOP가 내부적으로 어떻게 구성됐길래 핵심 로직에 끼워넣을 수 있도록 동작이 가능한지?

### AOP 적용하기 전 의존관계
AOP를 적용하기 전에는 memberservice 로직에서 모든 관심사항을 처리하고 있음
![[Pasted image 20240305154403.png]]
### AOP 적용 후 의존관계
memberService에서 직접 공통관심사항을 처리하는게 아닌, TimeLoggingAop 클래스를 따로 빼고 AOP 관련 어노테이션을 선언해서`@Aspect`, `@Around`, `joinPoint` 를 사용해서 공통 로직을 프록시 객체를 통해 호출하도록 처리.
![[Pasted image 20240305154810.png]]