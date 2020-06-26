# Some Tips for REST

> 이 페이지 내용의 대부분은 [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html) 블로그 글에서 그대로 발췌한 것이다.

<br>

## 입력 Form은 어떻게 받아오게 하지?

새로운 아이템을 작성하거나 기존의 아이템을 수정할 때 작성/수정 Form은 어떻게 제공할까.

정답은 <strong>Form 자체도 정보로 취급해야 한다</strong>는 것입니다. 서버로부터 `새로운 아이템을 작성하기 위한 Form을 GET한다`고 생각하시면 됩니다.

Rails 에선 기본적인 CRUD를 제공할 때 아래와 같은 REST 인터페이스를 구성해줍니다.

| HTTP Method | Path             | used for                                     |
| ----------- | ---------------- | -------------------------------------------- |
| GET         | /photos          | display a list of all photos                 |
| GET         | /photos/new      | return an HTML form for creating a new photo |
| POST        | /photos          | create a new photo                           |
| GET         | /photos/:id      | display a specific photo                     |
| GET         | /photos/:id/edit | return an HTML form for editing a photo      |
| PUT         | /photos/:id      | update a specific photo                      |
| DELETE      | /photos/:id      | delete a specific photo                      |

<br>

## 모바일 환경에 따라 다른 정보를 보여줘야 한다면?

일부 애플리케이션은 모바일용 웹서비스가 필요한 경우 PC용 서비스와 독립적인 서비스를 개발한 후 단지 이를 이동시켜주기만 할 때가 있다. 이는 좋지 못한 사용성을 제공한다. 모바일 뷰와 일반 웹페이지 뷰의 URI가 다르기 때문이다. 가령, 사용자가 모바일 뷰의 URI를 복사해서 PC에서 접근하는 경우 모바일에 최적화된 정보를 받을 수밖에 없게 된다.

REST 하게 만든다면 URI는 플랫폼 중립적이어야 한다. 정보를 보여줄 때 여러 플랫폼을 구별해야 한다면 Request Header의 `User-Agent` 값을 참조하는 것이 좋다. 예를 들어 iPhone에서 보내주는 `User-Agent` 값은 아래와 같다.

```
Mozilla/5.0 (iPhone; U; CPU like Mac OS X; en) AppleWebKit/420+ (KHTML, like Gecko) Version/3.0 Mobile/1A543a Safari/419.3
```

대부분의 브라우저, OS 플랫폼은 HTTP 요청을 보낼 시 보내는 주체에 대한 설명을 `User-Agent`에 상세하게 포함하여 통신하고 있기 때문에 요청자의 환경을 정확히 알 수 있다.

<br>

## 버전과 정보 포맷을 지정할 수 있게 해야 한다면?

오픈 API를 제공하거나, 클라이언트가 항상 최신 버전을 유지할 수 없는 환경이라면 서버에서 버전 협상을 지원해야 한다. 서버가 버전 협상을 지원한다면 최신 버전으로 업데이트가 되더라도 구 버전의 정보 요청에 하위 호환하게 하여 서비스 적용범위를 넓게 유지할 수 있다. 이와 함께 클라이언트가 html로 정보를 받을지, json으로 받을지, xml로 받을지 선택할 수 있다면 더욱 좋다.

Request Header의 `Accept` 헤더를 이용해서 요청 환경에서 정보의 버전과 포맷을 지정할 수 있게 합니다. 아래는 Github API에 요청 시 쓰는 `Accept` 헤더이다.

```
application/vnd.github+json
```

`vnd.`는 Vendor MIME Type으로, 서비스 개발자가 자신의 독자적인 포맷을 규정할 수 있게 HTTP 표준에서 제공하는 접두어입니다. `vnd.` 이후에 서비스 제공자 이름을 쓴 후, `+`로 문서의 기본 포맷을 표현해준다.

`Accept` 헤더는 파라미터도 받을 수 있다. 많은 REST 지지자들은 이 파라미터를 이용해 버전 명을 지정하는 것을 권장한다.

```
vnd.example-com.foo+json; version=1.0
```

<br>

---

### References

- [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)
