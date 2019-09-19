## 출력 버퍼와 응답
### 1. JSP 출력 버퍼
 - JSP 페이지는 출력 결과를 곧바로 웹 브라우저에 전송하지 않는다.
 - 대신 출력 버퍼(buffer)에 임시로 출력 결과를 저장했다가 한 번에 웹 브라우저에 전송한다.
      > JSP → 출력버퍼 → WAS(Web Application Server) → 클라이언트



 #### 출력버퍼 사용의 장점
  - 데이터 전송 성능 향상
      > 네트워크를 비롯한 모든 데이터 교환에서는 작은 단위를 여러 차례 보내는 것보다, 큰 단위로 한 번에 묶어 보내는 것이 더 높은 성능을 발휘한다.

  - JSP 실행 도중 버퍼를 비우고 새로운 내용 전송 가능
  - 버퍼가 다 차기 전에 헤더 변경 가능


 #### page 디렉티브에서 버퍼 설정하기
  - page 디렉티브에서 제공하는 buffer 속성을 사용하여 JSP 페이지가 사용할 버퍼를 설정할 수 있다.
  - 사용할 버퍼의 크기는 킬로바이트(kb) 단위로 지정한다.
```JSP
  <%@ page buffer="4kb"%>
```

  - buffer 속성값에 kb 단위를 붙이지 않으면 JSP 페이지를 자바 코드로 변환하는 과정에서 에러가 발생한다.
  - JSP 규약은 buffer 속성을 지정하지 않으면 최소 8kb 이상 크기의 버퍼를 사용하도록 규정하고 있다.
  - buffer 를 사용하고 싶지 않다면 속성값을 "none" 으로 지정하고, 곧바로 전송되기 때문에 출력내용을 취소할 수 없다.
```JSP
  <%@ page buffer = "none"%>
```


#### autoFlush 속성 설정하기
 - 플러시(flush) : 버퍼가 다 찼을 때, 버퍼에 쌓인 데이터를 전송하고 버퍼를 비우는 것
 - autoFlush = "true" 버퍼가 다 차면 버퍼를 플러시하고 계속 작업을 진행한다.
 - autoFlush = "false" 버퍼가 다 차면 예외를 발생시키고 작업을 중단한다.
    > java.io.IOException: 오류: JSP 버퍼 오버플로우

```JSP
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <%@ page buffer="1kb" autoFlush="false"%>

  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  	안녕하세요
  	<%
  		for(int i=0; i<1000; i++) {
  			out.print(i);
  		}
  	%>
  </body>
  </html>
```
