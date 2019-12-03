# 브라우저 (Browser)


## 브라우저 (Browser)
- 브라우저는 월드와이드웹(www)에서 정보를 검색, 표현하고 탐색하기 위한 소프트웨어이다.
- 브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것이다.
- 자원은 보통 HTML 문서이지만 PDF나 이미지 또는 다른 형태일 수 있다. 자원의 주소는 URI(Uniform Resource Identifier)에 의해 정해진다.
- 브라우저는 HTML과 CSS 명세에 따라 HTML 파일을 해석해서 표시하는데 이 명세는 웹 표준화 기구인 W3C(World Wide Web Consortium)에서 정한다.

## 브라우저 (Browser) 의 구성 요소
- 사용자 인터페이스(User interface) : 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등. 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분
- 브라우저 엔진(Browser engine) : 사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어
- 렌더링 엔진(Rendering engine) : 요청한 콘텐츠를 표시, 예를 들어 HTML을 요청하면 HTML과 CSS를 파싱하여 화면에 표시
* 통신(Networking) : HTTP 요청과 같은 네트워크 호출에 사용됨. (플랫폼 독립적인 인터페이스, 각 플랫폼 하부에서 실행)
* UI 백엔드(UI backend) : 콤보 박스와 창 같은 기본적인 장치를 그림
* 자바스크립트 해석기(JavaScript interpreter) : 자바스크립트 코드를 해석하고 실행
* 자료 저장소(Data storage) : 이 부분은 자료를 저장하는 계층, 쿠키 저장과 같이 모든 종류의 자원을 하드 디스크에 저장할 때 사용

## 브라우저 (Browser) 렌더링엔진 처리 과정
- HTML을 해석해서 DOM Tree를 만들고,CSS를 해석해서 역시 CSS Tree(CSS Object Model)을 만든다.
- DOM Tree와 CSS Tree, 이 두 개는 연관되어 있으므로 Render Tree로 다시 조합된다.
- 이렇게 조합된 결과는 화면에 어떻게 배치할지 크기와 위치 정보를 담고 있다.
- 이후에 이렇게 구성된 Render Tree정보를 통해서 화면에 어떤 부분에 어떻게 색칠을 할지 Painting과정을 거치게 된다.

## 브라우저의 렌더링을 방해하지 않기 위한 팁
- JavaScript 코드는 <body> 태그 닫히기 직전에 위치하는 것이 렌더링을 방해하지 않아서 좋고, css코드는 <head> 안에 위치해서 렌더링 처리 시에 브라우저가 더 빨리 참고할 수 있게 하는 것이 좋다.

***

###Reference
- [How Browsers Work: Behind the scenes of modern web browsers - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
- [브라우저는 어떻게 동작하는가](https://d2.naver.com/helloworld/59361)
