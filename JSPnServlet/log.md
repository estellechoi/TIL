## 로그 파일
 #### application 객체의 log 메소드
 - 로그 메시지를 기록하는 JSP 파일을 만들어보자.
 - 톰캣 디렉토리의 logs 폴더에서 로그 기록을 확인할 수 있다.
 ```JSP
    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
    <!DOCTYPE html>
    <html>
    <head>
    </head>
    <body>
    <%
        application.log("key");
    %>
    </body>
    </html>
 ```
