# Dispatcher Servlet??
디스패처 서블렛이란 HTTP로 들어오는 모든 요청을 맨 앞에 가장 먼저 받아, 적합한 컨트롤러에 연결, 위임 하는 역할을 한다. 
# 그렇다면 DispatcherServlet과 [[Servlet]]의 관계는 어떻게 될까?
- 서블릿 컨테이너는 DispatcherServlet을 포함한 모든 서블릿의 인스턴스를 생성하고 생명 주기를 관리한다. 스프링부트는 이를 위해 자동 구성 매커니즘을 제공하여 개발자가 서블릿 컨테이너를 쉽게 설정하고 사용할 수 있게 해줌. 
- DispatcherServvlet은 서블릿 컨테이너 내에서 작동하며 스프링 MVC 애플리케이션의 중심점으로써 모든 웹 요청을 처리한다.
## DispatcherServlet의 flow
![[Pasted image 20240315194718.png]]

1. HTTP 요청이 들어오면
2. HandlerMapping을 통해  적합한 컨트롤러와 메소드를 찾는다.
3. 컨트롤러로 위임할 핸들러 어댑터를 찾아서 어댑터를 결정한다.
4. 핸들러 어댑터가 컨트롤러로 요청을 위임하고
5. 컨트롤러에서는->service->repository->... 개발자가 구현한 로직에 대한 결과를 ModelAndView로 가져온다.
6. View 정보가 있을 때는 DispatcherServlet이 적절한 ViewResolver를 찾아 호출한다.
7. ViewResolver는 View를 찾아서 반환해준다.
8. 해당 VIew는 전달받은 model 값으로 SSR을 하고([[CSR 과 SSR]] 참고)
9. HTTP 응답을 HTML로 반환한다.

```Java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {

  HttpServletRequest processedRequest = request;
  HandlerExecutionChain mappedHandler = null;
  ModelAndView mv = null;

// 1. 핸들러 조회
  mappedHandler = getHandler(processedRequest);
  
  if (mappedHandler == null) {
    noHandlerFound(processedRequest, response);
    
    return;
}

// 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
  HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

// 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
  mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
  processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);

}

private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {

// 뷰 렌더링 호출
  render(mv, request, response);
}

protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {

  View view;
  String viewName = mv.getViewName();

// 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
  view = resolveViewName(viewName, mv.getModelInternal(), locale, request);

// 8. 뷰 렌더링
  view.render(mv.getModelInternal(), request, response);
}
```

