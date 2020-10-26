# HTTP Cookies

> This markdown page is Korean/English mixed entirely depending on my personal need, not readers' possible expectation. Just the reorderd, added, and removed from reference documents, or some parts are the English-Korean translated version. But, maybe helpful for some people.

<br>

## What is Cookie ?

An HTTP cookie is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with later requests to the same server. Typically, it's used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the `stateless` HTTP protocol.

### Cookies are mainly used for:

- Session management
- Personalization
- Tracking

<br>

## Other ways to store information in the browser

> 이 주제는 쿠키가 아닌 것들에 대한 소개이다. 하지만 이 글을 진지하게 읽는 사람이 있다면, 쿠키를 알기 전에 여기서 언급하는 것이 좋겠다.

Now recommended to use modern storage APIs. Modern APIs for client storage are the [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) and [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API).

### Web Storage API

[`window.localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) and [`window.sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) properties correspond to session and permanent cookies in duration, but have larger storage limits than cookies, and are never sent to a server.

### IndexedDB API

More structured and larger amounts of data can be stored using the IndexedDB API.

<br>

## Creating cookies

After receiving an HTTP request, a server can send a `Set-Cookie` header with the response. The cookie is usually stored by the browser, and then the cookie is sent with requests made to the same server inside a `Cookie` request header.

> Set-Cookie 헤더의 여러 옵션들을 보려면 [여기](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie).

<br>

### `Set-Cookie` Response Header

서버에서 사용자 브라우저로 쿠키를 보내고 싶다면, 응답 헤더 중 `Set-Cookie` 헤더에 담아 보낸다. 이 헤더는 이렇게 생겼다. `Set-Cookie: name=value`

```
Set-Cookie: cookie1=choco
Set-Cookie: cookie2=strawberry
```

<br>

### `Cookie` Request Header

클라이언트 단에서는 서버로부터 쿠키를 받으면 브라우저에 저장해놓았다가, 똑같은 서버로 요청을 보낼 때 저장하고 있는 모든 쿠키 정보를 그대로 회신한다. 어떻게 쿠키 정보를 보내냐면, 요청 헤더 중 `Cookie` 헤더에 담아서.

```
Cookie: cookie1=choco; cookie2=strawberry
```

<br>

> How to use the `Set-Cookie` header in various server-side applications

- [Node.js](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_response_setheader_name_value)
- [Python](https://docs.python.org/3/library/http.cookies.html)
- [Ruby on Rails](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html)

<br>

## Defining lifetime of a cookie

먼저, 쿠키에는 두 종류가 있다. `Session` 쿠키와 `Permanent` 쿠키.

- `Session` 쿠키는 현재 세션이 종료될 때 사라진다. 현재 세션의 종료시점은 브라우저가 결정한다.

  > some browsers use session restoring when restarting, which can cause session cookies to last indefinitely long.

- `Permanent` 쿠키는 개발자가 정하는 일시에 만료된다. 쿠키의 만료시점은 쿠키를 생성할 때 지정하며, `Set-Cookie` 헤더에서 관련 속성(`Expires`, `Max-Age`)을 이용해 설정한다. 아래와 같이.

<br>

#### `Expires` 속성

쿠키가 만료되는 일시를 지정한다.

> 주의 : `Expires` 일시는 서버가 아닌 사용자 브라우저의 시간을 기준으로 한다.

```
Set-Cookie: cookie1=orange; Expires=Wed, 31 Oct 2021 07:28:00 GMT;
```

<br>

#### `Max-Age`속성

쿠키의 생성 시점으로 부터 얼마 후 종료시킬지 지정한다. (초 단위)

```
Set-Cookie: cookie1=orange; Max-Age=60 * 60;
```

If both Expires and Max-Age are set, Max-Age has precedence.

<br>

## Restricting access to cookies

#### `Secure` 속성

Sent to the server only with an encrypted request over the HTTPS protocol, never with unsecured HTTP, and therefore can't easily be accessed by a `man-in-the-middle` attacker. Insecure sites (with http: in the URL) can't set cookies with the `Secure` attribute.

<br>

#### `HttpOnly` 속성

Inaccessible to the JavaScript `Document.cookie` API. This precaution helps mitigate [cross-site scripting (XSS)](<https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Cross-site_scripting_(XSS)>) attacks.

For example,

```
Set-Cookie: cookie1=milk; Secure; HttpOnly
```

<br>

## Defining where cookies are sent

#### `Path` 속성

Indicates a URL path that must exist in the requested URL in order to send the Cookie header.

For example, if `Path=/docs` is set, these paths match:

- `/docs`
- `/docs/Web/`
- `/docs/Web/HTTP`

<br>

#### `Domain` 속성

Specifies which hosts(domains) are allowed to receive the cookie.

- If unspecified, it defaults to the same origin that set the cookie, <strong>excluding</strong> subdomains.

- If specified, then subdomains are always included.

<br>

#### `SameSite` 속성

Lets servers require that a cookie shouldn't be sent with cross-origin requests. It provides protection against [cross-site request forgery attacks (CSRF)](https://developer.mozilla.org/en-US/docs/Glossary/CSRF).

Possible values are:

- `Strict` : only to the same URL.

- `Lax` : with an exception for when the user navigates to a URL from an external site, such as by following a link.

- `None` : no restrictions on cross-site requests.

최근 동향을 보면, 브라우저에서 쿠키를 받을 때 `SameSite=Lax` 속성 값을 기본으로 부여하고 있다. Cross Origin 요청시 쿠키를 요청에 포함시키고 싶으면, 서버에서 쿠키를 응답할 때 `None` 값을 명시적으로 지정해주어야 한다.

> [Browsers that support SameSite](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#Browser_compatibility)

For example,

```
Set-Cookie: cookie1=candy; SameSite=Strict
```

<br>

## Security

### Regenerate and resend session cookies whenever the user authenticates

If your site authenticates users, it should regenerate and resend session cookies, even ones that already exist, whenever the user authenticates. This helps prevent [session fixation attacks](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Session_fixation), where a third party can reuse a user's session.

<br>

### Use an opaque identifier

Information should be stored in cookies with the understanding that all cookie values are visible to, and can be changed by, the end-user. Depending on the application, it may be desirable to use an opaque identifier which is looked-up by the server or to investigate alternative authentication/confidentiality mechanisms such as JSON Web Tokens.

<br>

### Should have a short lifetime

Cookies for sensitive information (such as indicating authentication) should have a short lifetime.

<br>

### Use with `SameSite` attribute set to `Strict` or `Lax`

This has the effect of ensuring that the authentication cookie is not sent with cross-origin requests, so such a request is effectively unauthenticated to the application server.

<br>

### Use Cookie prefixes

`이 쿠키는 보안상 안전한 서버에서 생성되었습니다`와 같은 정보는 서버에서 제공할 수 없다. 심지어 쿠키가 생성된 서버의 주소도 제공할 수 없다. 보안상 이유로 쿠키 매커니즘이 그렇게 고안되었기 때문이다. 예로, `Domain` 속성을 이용하면 모든 서브 도메인에서 쿠키에 접근할 수 있도록 설정을 변경함으로써 해킹 공격이 가능하다. ([Session fixation attack](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Session_fixation))

쿠키에 대한 정보를 제공하고 싶으면 어떻게 해야하나. `Cookie prefixes`를 이용하면 안전하게 쿠키에 대한 정보를 제공할 수 있다. 다만, `Cookie prefixes`를 포함하는 쿠키들은 규정된 조건을 정확하게 지켜 속성 값들을 지정해야 한다. 그렇지 않으면 브라우저에서 해당 쿠키들을 거절해버린다.

### Cookie prefixes

- `__Host-` : If a cookie name has this prefix, it is accepted in a `Set-Cookie` header only if it is also marked with the `Secure` attribute, was sent from a secure origin, does not include a `Domain` attribute, and has the `Path` attribute set to `/`. In this way, these cookies can be seen as "domain-locked".

- `__Secure-` : If a cookie name has this prefix, it is accepted in a `Set-Cookie` header only if it is marked with the `Secure` attribute and was sent from a secure origin. Weaker than the `__Host-` prefix, though.

<br>

## Tracking: Third-party cookies

A web page may contain images or other components stored on servers in other domains (for example, ad banners), which may set third-party cookies. A third party server can build up a profile of a user's browsing history and habits based on cookies sent to it by the same browser when accessing multiple sites. Cookie blocking can cause some third-party components (such as social media widgets) to not function as intended.

> See also [types of cookies used by Google](https://policies.google.com/technologies/types).

<br>

## Privacy: Cookie-related regulations

Legislation or regulations that cover the use of cookies include:

- The General Data Privacy Regulation (GDPR) in the European Union
- The ePrivacy Directive in the EU
- The California Consumer Privacy Act

These regulations have global reach, because they apply to any site on the World Wide Web that is accessed by users from these jurisdictions.

These regulations include requirements such as:

- Notifying users that your site uses cookies.
- Allowing users to opt out of receiving some or all cookies.
- Allowing users to use the bulk of your service without receiving cookies.

There may be other regulations governing the use of cookies in your locality. The burden is on you to know and comply with these regulations. There are companies that offer "cookie banner" code that helps you comply with these regulations.

<br>

---

### Refereces

- [Using HTTP cookies | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
