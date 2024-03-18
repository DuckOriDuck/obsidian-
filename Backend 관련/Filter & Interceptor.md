# 필터와 인터셉터란?

클라이언트가 요청한 데이터가 로그인을 해야하거나 특정 접근 권한을 가져야만 확인할 수 있는것이라면, 이를 감지해 정보에 대한 접근을 차단하거나 로그인/회원가입 페이지로 이동시키는 작업을 생각해볼 수 있다.
## 필터와 인터셉터의 흐름
![[Pasted image 20240318233214.png]]
필터, 인터셉터 모두 스프링으 ㅣ컨트롤러가 실행되기 전/후에 실행된다.

클라이언트의 요청이 처리되는 순서는

HTTP 요청 -> 필터 -> 인터셉터 -> 컨트롤러 ->인터셉터 ->필터 -> 응답
순서대로 이루어진다. 필터가 먼저 요청을 처리한 후 인터셉터가 그 다음에 처리한다
# 필터와 인터셉터의 역할 차이

|            | Filter                                                              | Interceptor                                                              |
| ---------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **정의와 용도** | 서블렛 사양의 일부이다.<br>모든 종류의 Request(서블릿 요청 뿐만 아니라 정적 리소스까지도) 가로채 처리 가능. | Spring MVC의 일부로, Spring의 [[DispatcherServlet]]이 처리하는 컨트롤러 요청에만 영향을 준다.   |
| **작동 시점**  | - 요청이 서블릿에 도달하기 전과 클라이언트로 반환되기 전에 작동한다.<br>- 서블릿 컨테이너 수준에서 관리된다.    | - 컨트롤러가 호출되기 직전, 직후, 요청 처리가 완료된 후(뷰 렌더링 후) 처리를 담당한다.                     |
| **적용 범위**  | 어플리케이션에 걸쳐 모든 요청에 적용 가능하다. 특정 URL 패턴에 대해서만 적용도 가능하다.                | 특정 컨트롤러에 대해 적용시킬수도, 모든 컨트롤러에 대해 적용시킬수도 있다.                               |
| **사용처**    | - 인증 체크<br>- 요청 로깅<br>- 요청 데이터의 합축 해제<br>- 캐릭터 인코딩 설정이 포함된다.        | - 로그인 사용자의 세션 검증<br>- 컨트롤러 실행 시간 로깅<br>- 사용자의 권한 검사<br>- 모델 객체에 공통 속성 추가 |
## Filter
- [J2EE](https://docs.oracle.com/cd/E19159-01/820-4902/abfar/index.html) 표준 스택으로 스프링의 [[DispatcherServlet]]에 요청이 전달되기 전/후에 실행된다.
- [WAS]([[Web Server와 WAS(Web Application Server)]])(서블릿 컨테이너) 에서 실행되서 스프링 프레임워크 밖에서 처리가 된다고 보면 된다.
- 모든 HTTP 요청에 대해 실행된다.
- HTTP 요청 및 응답을 수정, 로깅, 인코딩 변경 등의 작업을 처리한다.

필터는 `jajarta.servlet` 패키지의 `Filter` 인터페이스를 구현해야 한다.
`Filter` 인터페이스
```java
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}
    
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;
            
    public default void destroy() {}

}
```
- `init()`
	- 필터를 초기화하기 위한 메소드.  최초 1회만 실행된다.
- `doFilter()`
	- 클라이언트의 오쳥이 올 때마다 실행된다. 매개변수로 request와 response를 받아 chain.doFilter를 통해 값을 변경하여 넘길 수 있음.

```java
chain.doFilter(request, response);
```
- `destroy()`
	- 필터 객체를 제거하고 자원을 반환하기 위한 메소드.
	- 서블릿 컨테이너가 종료될 때 1회만 실행된다.

### Filter Chain
여러 개의 필터를 만들어놓고 이를 순서대로 연결해서 사용 가능.
필터 3개가 있다면
	Client Request -> 필터1 -> 필터2 -> 필터3 -> controller
	Controller ->필터3 -> 필터2 -> 필터3 -> Client Response

# Interceptor
- 인터셉터는 스프링 MVC가 제공하는 기능이다. Controller가 실행되기 전/후 실행된다.
- 주로 개별 컨트롤러에 대한 로직을 처리하고자 할 때 사용된다.
- `org.springframework.web.servlet` 패키지의 `HandlerInterceptor` 인터페이스를 구현해야한다.
`HandlerInterceptor` 인터페이스
```java
public interface HandlerInterceptor 

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
	throws Exception {
        return true;
    }
    
    default void postHandle(HttpServletRequest request, HttpServletResponse response,
	Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }
        
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response,
	Object handler, @Nullable Exception ex) throws Exception {
    }
}
```
- `preHandle()`
	- 컨트롤러 호출 전에 실행되고 반환 타입이 boolean type으로 true일 경우 계속 진행하고 false면 진행을 멈춘다.
- `postHandle()` 
	- 컨트롤러 실행 직후에 실행됨. 컨트롤러가 반환하는 ModelANdView를 받아볼 수 있다. 만약 컨트롤러에서 exception이 발생하면 실행되지 않는다.
- `afterCompletion()`
	- `afterCompletion`은 항상 호출된다. 컨트롤러가 요청 처리가 완료된 후에 실행된다.
### Interceptor's flow
![[Pasted image 20240318235503.png]]

### Interceptor Chain
여러개의 Interceptor를 만들어놓고 순서대로 연결해서 사용할 수 있다.
	3개의 필터와 3개의 인터셉터가 있다고 할 떄
		Client 요청 → 필터 → 인터셉터1 → 인터셉터2 → 인터셉터3 → Controller
	와 같은 순서로 실행 될 것이다.



# Filter 실습 예시

### 1. `UrlPrintFilter.java` : Servlet의 HTTP 요청 URL을 로깅하는 역할
```java
package com.example.blogjpa.filter;  
  
  
import jakarta.servlet.*;  
import jakarta.servlet.http.HttpServletRequest;  
import lombok.extern.slf4j.Slf4j;  
import java.io.IOException;  
  
@Slf4j  
public class UrlPrintFilter implements Filter {  
  
@Override  
public void init(FilterConfig filterConfig) throws ServletException {  
//초기화 메세지
log.info("1. url print filter init() - {}", filterConfig.getFilterName()); 

}  
  
@Override  
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {  

HttpServletRequest httpServletRequest = (HttpServletRequest) request;  
//현재 Request의 URL을 받아 로깅한다.
log.info("1. UrlPrintFilter {}", httpServletRequest.getRequestURL().toString());  
log.info("1. UrlPrintFilter before");  
//로깅이 끝난 후 FilterChain을 호출하여 다음 필터 또는 서블릿을 실행한다.
chain.doFilter(request, response);  
log.info("1. UrlPrintFilter After");  
}  
  
@Override  
public void destroy() {  
log.info("1. url print filter destroy");  
}  
}
```

### 2. `AddTraceIdFilter.java` : HTTP 요청에 고유한 tracdId를 추가하는 역할을 합니다.
```java
package com.example.blogjpa.filter;  
import jakarta.servlet.*;  
import lombok.extern.slf4j.Slf4j;  
import java.io.IOException;  
import java.util.UUID;  
  
  
@Slf4j  
public class AddTraceIdFilter implements Filter {  
@Override  
public void init(FilterConfig filterConfig) throws ServletException {  
//filter 초기화시 호출됨.
log.info("2. AddTraceIdFilter init() - {}", filterConfig.getFilterName());  
}  
  
@Override  
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {  
//전달받은 request 속성에 traceid라는  이름으로 UUID를 생성한다. 
request.setAttribute("traceId", UUID.randomUUID().toString());  
log.info("2. AddTraceIdFilter before");  
chain.doFilter(request, response);  
log.info("2. AddTraceIdFilter After");  
}  
  
@Override  
public void destroy() {  
log.info("2. AddTraceIdFilter destroy()");  
}  
}
```

- [[UUID]]란? (Universally Unique Identifier) 고유한 식별자를 생성하는데 사용된다.
- 
### 3. `FilterConfiguration.java` :  구현한 Filter를 등록하고 우선순위를 정해준다.
```java
package com.example.blogjpa.config;  
import com.example.blogjpa.filter.AddTraceIdFilter;  
import com.example.blogjpa.filter.UrlPrintFilter;  
import jakarta.servlet.Filter;  
import org.springframework.boot.web.servlet.FilterRegistrationBean;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class FilterConfiguration {  
@Bean  
public FilterRegistrationBean<Filter> filterOne() {  
//URL 필터를 등록하는 bean을 생성한다.
FilterRegistrationBean<Filter> filter = new FilterRegistrationBean<>();  
filter.setFilter(new UrlPrintFilter());  
filter.setOrder(1); //Filter 우선순위 설정
return filter;  
}  
  
@Bean  
public FilterRegistrationBean<Filter> filterTwo() {

FilterRegistrationBean<Filter> filter = new FilterRegistrationBean<>();  
filter.setFilter(new AddTraceIdFilter());  
filter.setOrder(2);  //Filter 우선순위 설정
filter.addUrlPatterns("/filter/*");  
return filter;  
}  
}
```





# Interseptor 실습 예시


### 1. `MethodPrintInterceptor.java`
```java
package com.example.blogjpa.Interceptor;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import lombok.extern.slf4j.Slf4j;  
import org.springframework.web.method.HandlerMethod;  
import org.springframework.web.servlet.HandlerInterceptor;  
import org.springframework.web.servlet.ModelAndView;  
  
@Slf4j  
public class MethodPrintInterceptor implements HandlerInterceptor {  
@Override  
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
//로깅을 통해 인터셉터의 실행을 나타낸다.
log.info("MethodPrintInterceptor prehandle()");  
// 여기서 true를 반환하면 계속 진행, false를 반환하면 요청 중단
return true; 
}  
  
@Override  
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {  

HandlerMethod handlerMethod = (HandlerMethod) handler;  
//호출된  메소드 실행 후, 뷰가 렌더링 되기 전에 호출된 메소드의 리름을 로깅한다.
log.info("MethodPrintInterceptor postHandle() - {}", handlerMethod.getMethod().getName());  
}  
  
@Override  
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
//로깅을 통해 인터셉터의 실행 완료를 나타낸다.
log.info("MethodPrintInterceptor afterCompletion()");  
}  
}
```


### 2.`InterceptorConfiguration`:  
- `addInterceptors()` : WebMvcContigurer 인터페이스를 구현해서 인터셉터를 등록한다.
- `addInterceptor()` 메서드를 사용하여 MethodPrintInterceptor를 등록한다.
```java
package com.example.blogjpa.config;  
  
  
import com.example.blogjpa.Interceptor.MethodPrintInterceptor;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;  
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;  
// Interceptor 등록을 위한 Configuration 클래스이다.
@Configuration  
public class InterceptorConfiguration implements WebMvcConfigurer {  

@Override  
public void addInterceptors(InterceptorRegistry registry) {  
// MethodPrintInterceptor를 인터셉터로 추가한다.
registry.addInterceptor(new MethodPrintInterceptor());  
}  
}
```