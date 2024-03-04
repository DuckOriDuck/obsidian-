# 서블릿이란?
## 웹 서버에서 클라이언트의 요청을 처리하고 응답을 해주는 java 클래스이다.

## Servlet의 특징
- 클라이언트의 요청에 대해 동적으로 작용하는 웹 어플리케이션 컴포넌트이다.
- [[HTML]]을 사용하여 요청에 응답한다.
- Java thread를 이용하여 동작한다.
- [[MVC]] 패턴에서 Controller로 동작한다.
- HTTP 프로토콜  서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받음.
- [[UDP]]보다 느린 속도
- [[HTML]] 변경 시 Servlet을 재컴파일해야하는 단점이 있음.

![[Pasted image 20240228235909.png]]
이와 같은  요청/응답 메세지를 주고 받을때마다 개발자가
1. 코드로 요청 분석
2. 요청에 대한 알맞은 처리
3. 처리된 값을 규약에 맞게 응답으로 내보내준다.
요청 하나하나에 이런 설정들을 해주려면 너무 번거롭다. 이런 내부 동작을 Servlet 클래스에서 자동으로 지원해주는 것

# 서블릿 요청 처리 과정 
1. 클라이언트에서 URL을 입력해서 HTTP Request를 전달.
2. WAS에서 Servlet container로 request 전달.
3. 서블릿 컨테이너에서 Request 처리
	1. HttpServletRequest, HttpServletResponse 객체 생성.
	2. web.xml을 기반으로 사용자가 요청한 URL이 어떤 서블릿에 대한 요청인지 찾음.
	3. 해당 서블릿에서 service 메소드 호출한 후, 클라이언트의 요청이
		- GET일 때: doGet() 호출
		- Post일 때: doPost 호출
	4. doGet(), doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse 객체에 응답을 보냄.
	5. WAS에서는 HttpServletResponse객체에 담긴 정보를 HTML, JSON, XML 등의 형식으로 데이터를 클라이언트에게 응답함.
	6. 응답이 끝내면 HttpServletRequest, HttpServletResponse객체는 소멸.
4. 위에서 설명했던 Response 전달을 간략하게 만들면
	client 요청을 WAS에서 처리 -> 서블릿 컨테이너 -> 서블릿 객체 생성/호출 -> 클라이언트에게 response 반환
	![[Pasted image 20240229003601.png]]
## Servlet 컨테이너의 역할
- **Servlet의 생명주기를 담당한다.**
	
- **클라이언트에게서 [[HTTP]] 요청이 오면 Servlet을 생성, 응답을 주면서 종료한다. 웹 서버와 소켓으로 통신한다.**
	- - 원래는 소켓을 만들고 listen, accept 등을 해야하지만 서블릿 컨테이너는 이러한 기능을 API로 제공함.
- **멀티쓰레드 지원 및 관리**
	- 서블릿 컨테이너는 요청이 올 때 마다 새로운 자바 쓰레드를 하나 생성한다. HTTP 메소드를 실행하고 나면, 쓰레드는 자동으로 죽게 된다. 원래는 쓰레드를 관리해야하지만 서버가 다중 쓰레드를 생성/운영해주니 안정성에 대해서는 큰 걱정을 하지 않아도 된다.
- **선언적인 보안 관리**
	- 서블릿 컨테이너를 사용하면 보안에 관련된 내용을 서블릿이나 자바클래으세 구현하지 않아도 됨.
	- 일반적으로 보안관리는 XML 배포 서술자에 기록해서, 자바 소스 코드를 수정하고 다시 컴파일 할 필요 없이 보안에 대한 수정이 가능하다.
## Servlet 생명주기
- 내부 구현체를 참고해서 보여주자면 
![[Pasted image 20240304231043.png]]
- init(): 메모리에 서블릿 객체 없을 때 생성하기위해 호출. 
	- 처음 한번만 실행된다. 
	- 만약 실행중 서블릿이 변경될 경우, 기존 서블릿을 파괴하고 init()을 통해 새로운 내용을 메모리에 다시 적재한다.
- service(): 메모리에 서블릿 객체가 이미 있으면 호출
	- init()이 호출된 후 클라이언트의 요청에 따라 service()메소드를 통해 요청에 대한 응답이 doGet(), doPost()로 분기됨. 이때 HttpServletRequest, HttpServletResponse 객체가 제공된다.
- destroy(): 메모리에서 객체 제거 (WAS 종료)
	- 한 번만 실행됨.
	- 종료시에 처리해야하는 작업들은 destroy() 메소드를 오버라이딩해서 구현하면 됨.
- ![[Pasted image 20240229003900.png]]
- 서블릿 예제 코드.
	- HTTP 요청에 따라서 GET, POST, DELETE, PUT, PATCH 등등일 경우 각 메서드를 내부적으로 호출 가능하다.
```java
public class TestServlet extends HttpServlet {
   
  public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
		// GET 메소드 요청일 경우 호출

  } 
  
  public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
		 // POST 메소드 요청일 경우 호출
	}

}
```

## Servlet을 지원하는 웹 서버가 바로 [[Web Server와 WAS(Web Application Server)]]이다.

# 스프링부트와 WAS
- 스프링부트는 WAS의 별도 설치가 필요없다, 스프링부트로 만든 어플리케이션을 실행하면 자동으로 서버가 함께 실행된다.
- 다른 좋은 점은 개발자가 WAS에 서블릿을 직접 등록할 필요 없이 스프링부트가 자동으로 관리해줌. 만약 관리해주지 않으면 서블릿을 직접 등록해야 할것임.
## 스프링부트와 WAS가 요청을 처리하는 것을 그린 flow chart
![[Pasted image 20240304221607.png]]

추가 정보: JSP와 비슷한데, 
- JSP는 HTML안에 Java 코드를 포함한다.
- 서블릿은 Java 코드 안에 HTML을 포함하고 있다.

[참고 블로그 1: 망나니 개발자](https://mangkyu.tistory.com/14)
[참고 블로그 2:즐거운 개발일지](https://java-is-happy-things.tistory.com/23)
