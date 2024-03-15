###### API란?
- 네트워크에서 API란 프로그램간에 상호작용하기 위한 매개체이다.
# REST?
Representational State Transfer의 약자이다.

자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미함.

**Resource를 이름으로 구분하여 해당 resource의 상태를 주고 받는 모든 것을 의미한다**
- resource란: 통신을 통해 주고 받은 데이터이다.
## REST 란
- HTTP URI(Uniform Resource Identifier)를 통해 resource를 명시한다
- [[HTTP Method]](POST, GET, PUT, DELETE, PATCH) 를 통해서
- 해당 자원(URI)에 대한 [[CRUD operation]]을 적용하는것을 의미
## REST의 특징
1. Server-Client 구조
2. Stateless
	- 무슨 의미냐: 서버는 클라이언트의 요청을 처리하기 위한 모든 정보를 서버에 가지고 있어야 한다. 이렇게 함으로써 서버는 클라이언트에 대한 어떠한 state(상태)도 저장하고 있지 않으면서, 요청된 세션은 오직 유저의 상호작용에만 의지한다.
3. Cacheable
	- 가증하면 리소스를 클라이언트 or 서버측에서 캐싱할 수 있어야 한다. 그리고 전달된 리소스에 대해 캐싱이 허용되는지에 대한 여부에 대한 정보도 포함돼야한다.
4. Layered System
5. Uniform Interface
	- 요청이 어디에서 오던, 동일한 리소스에 대한 모든 API 요청은 동일하게 보여야 한다. 특정 데이터 조각에 대해서는 오직 하나의 URI만 있어야 한다.
6. code on demand(optional) 
	- 일반적으로 REST API는 정적 리소스를 전송하지만 특정한 경우에는 응답에 실행 코드(ex:Java 에플릿)을 포함할 수도 있다. 이 경우에 코드는 요청 시에만 실행돼야 한다.
		Java applet이란?: 자바 언어로 작성된 소프트웨어, 크기가 작아서 네트워크에서의 전송에 적합하고 WWW를 사용하여 배포할 수 있다. 실행시키기 위해서는 자바 가상머신을 사용하는 웹 브라우저가 필요하다.
## REST의 장점
- HTTP 프로토콜의 인프라를 그대로 사용해서 REST API 사용을 위한 별도의 인프라 구축 필요 X
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장.
- 여러가지 서비스 디자인에서 생길 수 있는 문제 최소화
- 서버와 클라이언트 역할을 명확하게 분리
## REST의 단점
- 표준 자체가 존재하지 않아 정의가 필요
- [[HTTP Method]]의 형태가 너무 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야해서 전문성이 많이 요구된다.
	- Header정보는 HTTP 요청과 응답에 추가 정보를 제공하는데 사용된다. 
	- 주로 사용자 인증, 캐싱 지지사항, 요청 본문의 형식, 요청에 대한 응답 형식 등을 포함한다.
	- 위의 정보들은 보통 URL에 직접 표시되지 않고 HTTP 헤더에 포함된다.
- 익스플로어와 같은 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.

# 그래서 REST API가 뭔데??
REST API란 REST의 원리를 따르는 API다. 이 REST API를 사용하기 위한 규칙이 있는데 웬만해서 따르는 게 좋을 것.

## 규칙 1. URL에는 동사를 쓰지 말고, 명사로 자원을 표시해야한다.  그리고 대분자보다는 소문자를 사용해야 한다.
예시로 ID가 1인 학생의 정보를 가져오는 URL은 이렇게 설계 가능하다.
```java
/students/1 //올바른 방법
/get-student?student_id=1 //올바르지 않은 방법.
```

왜 두번째가 올바르지 않은 방법이냐면, 학생의 정보를 가져올 때 쓸 수 있는 동사가 너무나도 많다. show, get, fetch, update 등등등.. 그래서 첫번째 URL처럼 정하는 것이 제일 적합하다.
## 규칙 2. 동사는 [[HTTP Method]]로.
EX)

| 설명 | 적합한 HTTP 메소드와 URL |
| ----- | -----------------|
| id가 1인 블로그 글을 조회하는 API | GET /articles/1|
|블로그 글을 추가하는 API|POST /articles|
|블로그 글을 수정하는 API|PUT /articles/1|
|블로그 글을 삭제하는 API|DELETE /articles/1|
## 규칙 3. 마지막에 슬래시를 포함하지 않는다
## 규칙 4.  `_` 언더바 대신 `-`하이폰을 사용한다
## 규칙 5. 파일 확장자는 URI에 포함하지 않는다.
## 규칙 6. 행위를 포함하지 않는다.
Bad Example `http://Oriduckduck.com/delete-post/1`
Good Example `http://Oriduckduck.com/post/1`

#### 정보 출처:
오르미 노션 강의자료 - REST API
[REST API by IBM](https://www.ibm.com/kr-ko/topics/rest-apis)- REST API
[허진쓰의 서버사이드 기술 블로그](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80) -REST API
[준환이형님쩜넷](https://topnanis.tistory.com/104) -java applet