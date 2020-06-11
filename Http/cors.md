# CORS (Cross Origin Resource Sharing)

> This markdown page is Korean/English mixed entirely depending on my personal need, not readers' possible expectation. Just the reorderd, added, and removed from reference documents, or some parts are the English-Korean translated version. But, maybe helpful for some people.

<br>

## SOP (Same Origin Policy)

- 브라우저의 보안 정책 중 하나이다. 어떤 [출처(Origin)](https://developer.mozilla.org/ko/docs/Glossary/Origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 중요한 보안 방식이다.

  > 샌드박스(Sandbox)라고도 한다. 샌드박스는 보호된 영역 안에서만 프로그램을 동작시킬 수 있도록 하며, 외부에 의해 영향을 받지 않도록 하는 모델을 말한다. 단어에서 유추할 수 있듯이 어린아이들이 뛰어놀 때, 다치지 않고, 그 안에서만 놀 수 있도록 만든 '모래 놀이통'에서 왔다.

- JavaScript의 [XMLHttpRequest](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest)와 [Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)는 SOP를 따른다.

- 이 정책은 초기에 웹사이트 보안을 위한 좋은 방법으로 생각되었으나, 여러 도메인에 걸쳐 구성되는 대규모 웹 프로젝트가 늘어나고 REST API 등을 이용한 외부 호출이 많아지면서 어떤 상황에서는 거추장스러운 기술이 되었다. (Cross Origin Issue)

> 동일출처 정책(SOP)에 대한 자세한 설명은 [여기](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)

### 출처(Origin)에 해당하는 것

웹 애플리케이션에서 요청하는 URL(프로토콜, 도메인, 포트)가 웹 애플리케이션의 프로토콜, 도메인, 포트와 다르면 Cross Origin 요청이다.
요청하는 URL의 스킴(프로토콜), 호스트(도메인), 포트이다. <strong>프로토콜, 도메인, 포트가 모두 일치하는 경우 같은 출처를 가졌다고 한다.</strong>

> 물리적으로 동일한 서버에서도 여러 도메인을 사용할 수 있는데, 이 때에도 동일하게 Cross Origin 이슈가 발생한다.

<br>

## What is CORS(Cross Origin Resource Sharing) ?

CORS는 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 리소스에 접근할 수 있도록 브라우저에 알려주는 체제이다. SOP를 회피하여 Cross Origin 이슈를 해결할 수 있다.

<br>

## CORS 작동방식

- 요청하는 서버 URL이 외부 도메인일 경우 브라우저는 실제 요청을 보내기 전에 Preflight 요청을 보낸다.
- Preflight 요청은 해당 URL에 `OPTIONS` 메소드로 요청을 미리 날려보고, 접근 권한이 있는지 여부를 확인한다.

![preflight](./../image/preflight_correct.png)

<br>

## 서버에서 CORS 핸들링하기

- 서버로 보내진 Preflight 요청을 처리하여 브라우저에서 실제 요청을 보낼 수 있도록 허용한다.

### 모든 외부 도메인에 대해 요청 허용하기

```java
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
// Preflight 요청을 받기 위해 OPTIONS 메소드를 허용한다.
Access-Control-Max-Age: 3600
Access-Control-Allow-Headers: Origin, Accept, X-Requested-With, Content-type, Access-Control-Request-Method, Access-Control-Request-Headers, Authorization
```

#### Credentialed requests and wildcards

- Don't use `Access-Control-Allow-Origin: *` if your server is trying to set cookie and you use `withCredentials: true`. When responding to a credentialed request, the server must specify an origin in the value of the `Access-Control-Allow-Origin` header, instead of specifying the `*` wildcard. Read "Credentialed requests and wildcards" part of [this document](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

<br>

### 외부 도메인 요청을 선별적으로 허용하기

- 아래의 Request Headers 값을 보고, 해당 출처(Origin)로부터의 접근을 허용하는 요청 스펙을 Response Headers에 넣어주는 구현을 하면 된다.
- Filter나 Interceptor 등을 통해 구현해야 한다.
  > 최근 Spring 프레임워크에 CORS Supprt 기능이 추가되었다. (4.2 ver +)

#### Request Headers

- Origin : 요청을 보내는 페이지의 출처 (도메인)
- Access-Control-Request-Method : 실제 요청하려는 메소드
- Access-Control-Request-Headers : 실제 요청에 포함되어 있는 헤더 이름

#### Response Headers

- Access-Control-Allow-Origin : 요청을 허용하는 출처를 지정한다. `*` 이면 모든 곳에 공개되어 있음을 의미한다.

- Access-Control-Allow-Credentials : <b>클라이언트 요청이 쿠키를 통해서 자격 증명을 해야 하는 경우에 `true`.</b> `Access-Control-Allow-Credentials: true`를 응답받은 클라이언트는 실제 요청 시 서버에서 정의된 규격의 인증값이 담긴 쿠키를 같이 보내야 한다.

  > Preflight 요청에 대한 응답에 포함시키면, 실제 요청에서 자격증명을 이용할 수 있는지에 대해 알려준다.
  > 심플한 `GET` 요청의 경우, Preflight 요청을 생략하고 바로 실제 요청을 보낸다. 이때 Response Headers에 이 속성이 없다면 브라우저는 응답이 있더라도 무시하고 웹 콘텐츠가 전달 되지 않는다.
  > 자격증명은 Requet Headers의 Cookie, Authorization와 TLS 클라이언트 인증서를 말한다.
  > `XMLHttpRequest.withCredentials` 속성이나 Fetch API 생성자의 `Request()`의 `credentials` 옵션과 함께 작동한다.

- Access-Control-Expose-Headers : 클라이언트 요청에 포함되어도 되는 사용자 정의 헤더를 지정한다.

- Access-Control-Max-Age : 클라이언트에서 Preflight 의 요청 결과를 저장할 기간을 지정한다. 클라이언트에서 Preflight 요청의 결과를 저장하고 있을 시간이다. 해당 시간 동안은 Preflight 요청을 다시 하지 않는다.

- Access-Control-Allow-Methods : 요청을 허용하는 메소드를 지정한다. <b>기본값은 `GET`, `POST` 이다. 이 헤더가 없으면 `GET`과 `POST` 요청만 가능하다.</b> 만약 이 헤더가 지정되어 있으면, 클라이언트에서는 헤더 값에 해당하는 메소드일 경우에만 실제 요청을 시도하게 된다.

- Access-Control-Allow-Headers : 요청을 허용하는 헤더를 지정한다.

<br>

## 클라이언트에서 CORS 핸들링하기

- 개발자가 테스트 혹은 개발 단계에서 임시로 핸들링 : 웹 브라우저 실행 옵션이나 플러그인을 통한 SOP 회피

- 서버 쪽 컨트롤이 불가능한 경우 : JSONP방식으로 요청 (GET 요청만 가능)

- 서버를 클라이언트와 같이 개발하거나 서버 개발 쪽 수정이 가능한 경우 : 서버에서 CORS 요청이 허용되도록 구현

---

### Reference

- [교차 출처 리소스 공유 (CORS) | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- [Cross-Origin Resource Sharing (CORS) | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Access-Control-Allow-Credentials](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)
- [Same Origin Policy | W3C](https://www.w3.org/Security/wiki/Same_Origin_Policy)
- [javascript ajax 크로스도메인 요청 - CORS](https://brunch.co.kr/@adrenalinee31/1)
- [Using CORS](https://www.html5rocks.com/en/tutorials/cors/)
- [Origin <origin> is not allowed by Access-Control-Allow-Origin | Stack Overflow](https://stackoverflow.com/questions/18642828/origin-origin-is-not-allowed-by-access-control-allow-origin)
- [enable cross-origin resource sharing](https://enable-cors.org/)
- [[Vue.js Quick Start] axios를 이용한 서버통신](https://mkki.github.io/vue.js/2018/05/09/start-vuejs-10.html)
- [API Proxying During Development](http://vuejs-templates.github.io/webpack/proxy.html)
