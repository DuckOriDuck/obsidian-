# WSGI(Web Server Gateway Interface)
- Java의 [[Servlet]]이라고 보면 된다
- 위스키라고 읽고 CAllable Object를 통해 Web server가 요청에 대한 정보를 Application에 전달한다.
- Callable Object는 Function이나 object의 형태가 될 수 있으며, Web Server는 Callable Object를 통해 2가지 정보를 전해줘야 함.
	- HTTP Request에 대한 정보(Method, URL, Data, ...)
	- Callback 함수
environ이 HTTP Request에 대한 정보를 담고 있고, start_response가 web Server에게 결과를 돌려주기 위한 콜백함수다.