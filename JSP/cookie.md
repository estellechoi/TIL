## 웹에서 쿠키(Cookie) 사용하기
 #### 쿠키(Cookie)란 ?
 - 웹 서버가 브라우저에게 지시하여 사용자 로컬 PC의 파일/메모리에 저장하는 작은 기록 정보 파일이다.
 - 파일에 담긴 정보는 인터넷 사용자가 같은 웹사이트를 방문할 때마다 읽히고 수시로 새로운 정보로 바뀔 수 있다.
 - 웹 서버가 브라우저로부터 요청을 받으면 쿠키를 생성하여 브라우저 응답(response)에 추가하고(정보 저장), 이후 브라우저로부터 온 요청에 포함된 쿠키를 읽어서 이전에 요청했던 브라우저인지 판단한다.

 #### 배경
 - 정보를 유지할 수 없는 Connectionless, Stateless의 성격을 가진 HTTP의 단점을 해결하기 위해 쿠키라는 개념이 도입되었다.

 #### 쿠키의 작동 원리
 - javax.servlet.http 패키지에 있는 Cookie 클래스의 객체를 생성한다. 이렇게 생성된 쿠키에는 각각의 웹 브라우저를 판별할 수 있는 정보가 포함되어 있다. 쿠키는 웹 서버가 웹 브라우저의 요청에 응답할 때 response 객체에 실려서 사용자의 웹 브라우저에 저장된다.
 - 웹 브라우저에 저장된 쿠키는 사용자가 다시 웹 서버에 요청을 할 때 request 객체에 실려서 다시 웹 서버에 전달된다. 이때 웹 서버는 전달된 쿠키의 값을 읽어서 같은 웹 브라우저로부터 온 요청인지를 판별하게 된다.

 #### 쿠키 구성요소
 - Name : 쿠키의 이름
 - Value : 쿠키에 저장된 값
 - Expires : 쿠키 만료 시각/기간
 - Domain : 쿠키가 사용되는 도메인 지정
 - Path : 쿠키를 반환할 경로 설정
 - Secure : 보안 연결 설정
 - HttpOnly : Http외 다른 통신 사용 가능 설정

 #### 쿠키의 단점
 - 쿠키에 대한 정보를 매 헤더(Http Header)에 추가하여 보내기 때문에 상당한 트랙픽을 발생시킨다.
 - 결제정보등을 쿠키에 저장하였을때 쿠키가 유출되면 보안에 대한 문제점도 발생할 수 있다.

 #### 쿠키를 사용해보자 !
 1) 쿠키 생성하기
 ```JSP
  <%
      // 쿠키 생성 ("이름", "값")
      Cookie cookie = new Cookie("name", "youjin");

      // 쿠키 만료 시간 설정 (초단위)
      cookie.setMaxAge(60*60);

      // 클라이언트 응답에 쿠키 추가 (생성한 쿠키 객체)
      response.addCookie(cookie);
  %>
 ```

 2) 저장된 쿠키 정보 읽어오기
 ```JSP
  <%
      // 요청정보에 포함된 쿠키 가져오기
      // 리턴 자료형은 Cookie 배열
      Cookie cookies[] = request.getCookies();

      // 쿠키의 개수를 출력해보자
      out.print(cookies.length + "<br>");

      // 쿠키의 이름과 값을 모두 출력해보자
      for (int i = 0; i < cookies.length; i++) {
        out.print(cookies[i].getName() + ", " + cookies[i].getValue() + "<br>");
      }
  %>
 ```

 3) 쿠키를 모두 삭제해보자
 ```JSP
  <%
      // 쿠키 가져오기
      Cookie cookies[] = request.getCookies();

      // 쿠키 만료시간을 0으로 설정하여 삭제한다
      // 응답에 만료시간이 0으로 설정된 쿠키를 추가!
      for (int i = 0; i < cookies.length; i++) {
        cookies[i].setMaxAge(0);
        response.addCookie(cookies[i]);
      }
  %>
 ```

 ***
 ###     * 참고문서

   * [웹에서 쿠키(Cookie)의 사용](https://hyeonstorage.tistory.com/114)
   * [쿠키(Cookie) 그리고 세션(Session)](https://nesoy.github.io/articles/2017-03/Session-Cookie)
