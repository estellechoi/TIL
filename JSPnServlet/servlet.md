# 웹서버 (Web Server) 와 웹 어플리케이션 서버(Web Application Server)


## 자바 웹 어플리케이션 (Java Web Application)
- WAS 에 설치(deploy)되어 동작하는 어플리케이션이다.
- HTML, CSS, 이미지, 자바 클래스(Servlet, Interface 등 포함), 각종 설정파일 등이 포함된다.

## Context Path
- WAS(Web Application Server)에서 웹 어플리케이션을 구분하기 위한 path
- 이클립스에서는 디폴트로 프로젝트 이름이 설정된다. \/projectname

## 서블릿 (Servlet) 이란 ?
- WAS 에서 동작하는 Java 클래스이다.
- URL 요청을 처리하는 프로그램
	 - URL mappings : 서블릿 클래스가 WAS 에 배포될 때 사용할 이름 지정
- HttpServlet 클래스를 상속받아야 한다.
- 웹 페이지를 구성하는 화면(HTML)은 JSP로 표현하고, 복잡한 프로그래밍은 서블릿으로 구현한다.

## Servlet 작성법
- Servlet 3.0 이상
	 - web.xml 사용하지 않음
	 - 자바 어노테이션(Annotation) 사용
	 - `@WebServlet(“/url”)`
- Servlet 3.0 미만
	 - web.xml 에 서블릿 등록 및 매핑
	 - `<servlet>` `<servlet-mapping>``

## Servlet 의 생명주기(Life Cycle)
- HttpServlet 클래스의 3 가지 메소드를 Override
	 - init()
	 - service(request, response)
	 - destroy()
- 서블릿은 서버의 메모리에 1 개의 객체만 생성
	 1. 처음으로 서블릿이 호출되면 메모리에 서블릿 객체가 없으므로 객체를 생성
	 2. init 메소드 호출 - 서블릿 객체가 생성될 때 한 번만
	 3. service 메소드 호출 - 서블릿이 호출될 때마다
	 4. destroy 메소드 호출 - 서블릿 객체가 소멸할 때 한 번만
- service(request, response) 메소드
	 - 서블릿 클래스에서 service() 메소드를 오버라이딩 하지 않으면 부모인 HttpServlet 클래스의 service() 메소드가 호출됨
	 - HttpServlet 클래스의 service() 메소드는 템플릿 메소드 패턴으로 구현
		  - GET 요청일 때 doGet() 메소드 호출
		  - POST 요청일 때 doPost() 메소드 호출

## 요청(Request)과 응답(Response)
- WAS는 Servlet 요청을 받으면 요청 정보를 HttpServletRequest 객체를 생성하여 저장하고, 객체를 서블릿에 전달한다.
- 웹브라우저 응답을 보낼 때는 HttpServletResponse 객체를 생성하고 서블릿에 전달한다.
- HttpServletRequest
	 - HTTP 프로토콜의 요청 정보를 서블릿에 전달하는 객체이다.
	 - 헤더정보, 파라미터, 쿠기, URI, URL 등의 정보를 읽는 메소드를 가지고 있다.
	 - Body 의 Stream 을 읽어 들이는 메소드를 가지고 있다.
- HttpServletResponse
	 - WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에 응답하기 위해 HttpServletResponse 객체를 생성하여 서블릿에 전달한다.
	 - 서블릿은 HttpServletResponse 객체를 이용하여 ContentType, 응답코드, 응답 메세지 등을 전송한다.

## doGet() 메소드
- html 내 form 태그의 method 가 get 일 때 호출됨
- 웹브라우저의 주소창을 통해 서블릿을 요청할 때 호출됨
- 인코딩처리 : server.xml 에서 `<Connector>` 태그에 속성 추가 : `URIEncoding = “UTF-8”``

***

### Reference
  - edwith \[부스트코스\] 웹 프로그래밍 교육자료
