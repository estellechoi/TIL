# JSP (JavaServer Pages)
> JSP 에 대한 기본 내용들을 정리했다.

<br>

## JSP (JavaServer Pages)
- 모든 JSP 는 서블릿으로 바뀌어 동작한다.
- WAS 는 브라우저로부터 JSP 에 대한 요청을 받으면, JSP 코드를 서블릿 코드로 변환한 후 컴파일하여 실행한다.
- hello.jsp -> hello\_jsp.java (서블릿으로 변환) -> hello\_jsp.class (컴파일)
	 - 이클립스를 사용한다면 아래의 경로에서 .java .class 파일이 생성된 것을 확인할 수 있다.
	   `.metadata/.plugins/org.eclipse.wst.server.core/tmp0/work/Catalina/localhost/project/org/apache/jsp/`

- hello_jsp.java 서블릿 파일에 \_jspService() 메소드 내에 JSP 코드가 변환되어 들어가 있다.
- 실행순서
	 - JSP 최초 요청인 경우 (WAS 메모리에 서블릿 객체가 없을 때)
		  1.  .jsp -> .java (서블릿)
		  2. .java -> .class (컴파일)
		  3. 서블릿 클래스 로딩 및 인스턴스 생성
		  4. 서블릿이 실행되어 응답결과 생성

	 - JSP 최초 요청이 아닌 경우 (WAS 메모리에 서블릿 객체 존재)
		  1. 서블릿이 실행되어 응답결과 생성

<br>

## JSP 문법
- \<\%\@ \%\> : 지시문
	 - \<\%\@ page \%\> : page 지시문
	 - JSP 를 실행하는 WAS 에 지시문의 내용이 전달된다.

- \<\% \%\> : 스크립트릿 (Scriptlet)
	 - 서블릿의 service() 메소드에 추가되는 프로그램 Java 코드 작성
	 - 스크립트릿에서 선언된 변수는 service() 메소드의 지역변수

- \<\%\= \%\> : 표현식 (Expression)
	 - 브라우저 응답 결과에 출력할 Java 코드 (값) 작성
	 - 스크립트릿 \<\% \%\> 에서도 내장객체인 out 의 print() 메소드를 사용해서 응답 결과에 출력할 부분 작성 가능

- \<\%\! \%\> : 선언문 (Declaration)
	 - 서블릿의 service() 메소드 외부에 전역변수 및 메소드를 선언하는 Java 코드 작성

- \<\%\— \—\%\> : 주석 (Comment)
	 - 서블릿으로 변환되지 않는 부분


<br>

## JSP 내장객체
- 서블릿의 \_jspService() 메소드에 미리 선언된 객체들이 있는데, 이 객체들은 JSP 에서 바로 사용 가능하다.
- response, request, application, session, out 과 같은 변수들이 내장객체이다.

<br>

## 리다이렉트 (Redirect)
- HTTP 로 정해진 규칙이다.
- 서버는 클라이언트의 요청에 대해 응답을 할 때, 특정 URL로 이동하도록 할 수 있는데, 이것을 리다이렉트(Redirect)라고 한다.
- 서버는 응답할 때 응답코드 302와 함께 특정 URL 정보를 Location Header 에 담아 클라이언트에게 전달한다. 클라이언트는 서버로부터 응답코드 302를 받으면, Location Header 값으로 재요청을 보낸다. 이때 브라우저의 주소창은 Location Header 에 저장된 URL 주소로 바뀌게 된다.
- 서블릿은 HttpServletResponse 객체의 sendRedirect(\“url\”) 메소드를 사용한다.

<br>

## 포워드 (Forward)
-  요청을 받은 서블릿이 요청을 처리한 후 바로 클라이언트에게 응답하지 않고, 처리 결과를 HttpServletRequest 객체에 저장하여 다른 서블릿에게 요청을 포워딩 하는 것이다.
- 요청을 포워딩 받은 서블릿은 HttpServletRequest, HttpServletResponse 객체를 이용하여 요청을 처리한 후 응답 결과를 클라이언트에 전송한다.
- 클라이언트 \-\> 서블릿 1 \-\> 서블릿 2 \-\> 클라이언트
- 리다이렉트와 다르게 포워드가 실행될 때는 브라우저 주소창의 url 이 바뀌지 않는다. (1 번의 요청)

<br>

## 서블릿과 JSP의 연동
- 서블릿은 프로그램 로직 수행에 유리하고, 결과 출력은 JSP 가 유리하다.
- 서블릿에서 프로그램 로직이 수행되고, 그 결과를 JSP로 포워딩(forward) 한다. (서블릿과 JSP의 연동)

<br>

<hr>

### Reference
- edwith [부스트코스] 교육 자료
