# How to stay semantic

> 시멘틱 마크업(Semantic Markup) 작성을 위한 팁들을 태그(Tag) 별로 나열하는 문서입니다.

<br>

## `<h1>` ~ `<h6>`

- 웹 브라우저가 제목의 정보를 사용해 자동으로 문서 콘텐츠의 표를 만드는 등의 작업을 수행할 수 있습니다.

- 글씨 크기를 크게 만들기 위해 `<h1>`과 같은 제목 태그를 사용하지 마세요. 대신 CSS의 `font-size` 속성을 사용하세요.

- 제목 단계를 건너뛰는 것을 피하세요. 항상 `<h1>`으로 시작해서, `<h2>`,.. 순차적으로 사용하세요.

- 페이지 당 하나의 `<h1>` 태그만 사용하세요. 여러 개를 써도 오류는 나지 않겠지만, `<h1>` 태그는 하나만 사용하는 것이 모범적입니다. 논리적으로 생각했을 때도, `<h1>`은 가장 중요한 제목이므로 전체 페이지의 목적을 설명하는데 사용해야 합니다. 두 개의 제목을 가진 책이나, 여러 개의 이름을 가진 영화는 볼 수 없죠! 또한 스크린 리더(Screen Reader) 사용자와 검색엔진 최적화(SEO)에도 더 적합합니다.

<br>

## `<header>`

<br>

## `<footer>`

<br>

## `<main>`

> IE 지원 X

> 주요 콘텐츠를 나타냅니다. 주요 콘텐츠란 문서의 핵심 주제, 앱의 핵심 기능에 직접적으로 연결되거나 기능을 확장하는 콘텐츠입니다.

- (`hidden` 속성 없이는) 문서 전체에 하나의 `<main>` 요소만 존재해야 합니다.

- 해당 문서만의 유일한 내용을 담아야 합니다. 사이드바, 탐색 링크, 저작권 정보, 사이트 로고, 검색 Form 등 여러 문서에 걸쳐 반복되는 콘텐츠는 포함해선 안됩니다. 그러나 검색 폼이 페이지의 주요 기능이라면 예외로 둘 수 있습니다.

- 개요에 영향을 주지 않습니다. `<body>` 등의 요소나 `<h2>`와 같은 제목 요소와 달리, `<main>`은 페이지의 개념적 구조를 바꾸지 않으며 온전히 정보 제공용입니다.

<br>

### 브라우저 호환성 이슈

IE 11 이하의 브라우저를 지원해야 한다면 `main` [role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques#Landmark_roles) [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques)를 사용하세요.

<br>

## `<article>`

> 문서, 페이지, 애플리케이션, 또는 사이트 안에서 독립적으로 구분해 배포하거나 재사용할 수 있는 구획을 나타냅니다. (게시판, 블로그 글, 매거진, 뉴스 기사 등)

- 예를 들어, 사용자가 스크롤하면 계속해서 다음 글을 보여주는 블로그의 경우, 각각의 글이 `<article>` 요소가 되며, 그 안에는 또 여러 개의 `<section>`이 존재할 수 있습니다.

- 주로 제목 요소(`<h1>` ~ `<h6>`)를 포함하여 각각의 `<article>`을 식별합니다.

- `<article>` 요소 안에 `<article>` 요소를 중첩할 수 있습니다.

> 독립성, 재사용성 !

<br>

## `<section>`

> 문서의 독립적인 구획을 나타내며, 더 적합한 의미를 가진 요소가 없을 때 사용합니다.

- 요소의 콘텐츠를 외부와 구분하여 단독으로 묶는 것이 나아보인다면 `<article>` 요소가 더 좋은 선택일 수 있습니다.

- `<section>` 요소를 일반 컨테이너로 사용하지 마세요. 특히 단순한 스타일링이 목적이라면 `<div>` 요소를 사용해야 합니다. 대개, 문서 요약에 해당 구획이 논리적으로 나타나야 하면 `<section>`이 좋은 선택입니다.

  > 아무런 의미가 없는 `<div>` 요소와 달리, `<section>` 요소는 의미를 가진다는 점에 주의하세요.

- 주로 제목 요소(`<h1>` ~ `<h6>`)를 포함하여 각각의 `<section>`을 식별합니다. (`<article>` 요소와 마찬가지로)

<br>

```html
<h1>How to Fish</h1>
<section>
	<h2>Introduction</h2>
	<p>People have been catching fish for food since before recorded history…</p>
</section>
```

<br>

## `<aside>`

> 문서의 주요 내용과 직접적인 관련이 없는 부분을 나타냅니다. (사이드바 혹은 콜아웃 박스 - 광고, 배너 영역에 많이 사용합니다.)

![semantic](./../img/semantic.jpg)

<br>

## `<nav>`

> 현재 페이지 내부, 또는 다른 페이지로의 링크를 보여주는 영역입니다. 보통 메뉴, 목차, 색인 등에 사용합니다. (Navigation)

```html
<nav>
	<ul>
		<li>Home</li>
		<li>About</li>
		<li>Contact</li>
	</ul>
</nav>
```

<br>

- 문서의 모든 링크가 `<nav>` 요소 안에 있을 필요는 없습니다. `<nav>` 요소는 주요 탐색 링크 블록을 위한 요소입니다.

  > 대개 `<footer>` 요소가 `<nav>`에 들어가지 않아도 되는 링크를 포함합니다.

- 스크린 리더(Screen Reader)는 최초 렌더링에서 탐색 전용 콘텐츠를 제외할지 결정할 때 `<nav>`를 참고합니다.

<br>

## `<address>`

> 사람, 단체, 조직 등에 대한 연락처 정보를 나타냅니다.

```html
<address>
	<a href="mailto:jim@rock.com">jim@rock.com</a><br />
	<a href="tel:+13115552368">(311) 555-2368</a>
</address>
```

> `<a href="mailto:...">` : 이 요소를 클릭하면, 각 플랫폼에서 지원하는 메일 쓰기 페이지/앱으로 이동합니다.

> `<a href="tel:...">` : 이 요소를 클릭하면, 각 플랫폼에서 지원하는 전화 앱으로 이동합니다.

<br>

- 연락처 외의 정보(출판일 등)를 담아서는 안됩니다.

- `<body>`, `<article>`, `<footer>` 등에서 `<address>` 요소를 포함하여 사용합니다.

<br>

## `<div>`

> 아무 의미가 없느 영역을 나타냅니다. 스타일링 목적으로 사용합니다. (Division)

<br>

<br>
<br>
<br>
<br>

---

### References

- [\<h1\>–\<h6\>: The HTML Section Heading elements | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)
- [HTML elements reference - Content sectioning | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Content_sectioning)
- [\<article\>: The Article Contents element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article)
- [\<section\>: The Generic Section element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section)
- [\<aside\>: The Aside element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)
- [\<nav\>: The Navigation Section element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav)
