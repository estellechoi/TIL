# How to stay semantic

> 시멘틱 마크업(Semantic Markup) 작성을 위한 팁들을 태그(Tag) 별로 나열하는 문서입니다.

<br>

## Content sectioning

다음은 콘텐츠를 구분할 때 사용하는 태그입니다.

- `<h1>`-`<h6>`

- `<hgroup>`

  > Multi-level heading. It groups a set of `<h1>`–`<h6>` elements.

- `<header>`

- `<footer>`

- `<main>`

- `<article>`

- `<section>`

- `<aside>`

- `<nav>`

- `<address>`

<br>

### 예시

```html
<body>
	<header>
		<nav>
			<ul>
				<li>Menu1</li>
				<li>Menu2</li>
				<li>Menu3</li>
			</ul>
		</nav>
	</header>

	<main role="main">
		<section>
			<h1>Section</h1>
			<article>
				<h2>Article1</h2>
				<p>..</p>
			</article>
			<article>
				<h2>Article2</h2>
				<p>..</p>
			</article>
		</section>

		<aside>Aside</aside>
	</main>

	<footer>
		<address>
			<a href="mailto:abc@gmail.com">abc@gmail.com</a>
			<a href="tel:+821000000000">010-0000-0000</a>
		</address>
	</footer>
</body>
```

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

이렇게요.

```html
<main role="main">
	...
</main>
```

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

### 예시

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

- 문서의 모든 링크가 `<nav>` 요소 안에 있을 필요는 없습니다. `<nav>` 요소는 주요 탐색 링크 블록을 위한 요소입니다.

  > 대개 `<footer>` 요소가 `<nav>`에 들어가지 않아도 되는 링크를 포함합니다.

- 스크린 리더(Screen Reader)는 최초 렌더링에서 탐색 전용 콘텐츠를 제외할지 결정할 때 `<nav>`를 참고합니다.

<br>

### 예시

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

## `<address>`

> 사람, 단체, 조직 등에 대한 연락처 정보를 나타냅니다.

- 연락처 외의 정보(출판일 등)를 담아서는 안됩니다.

- `<body>`, `<article>`, `<footer>` 등에서 `<address>` 요소를 포함하여 사용합니다.

<br>

### 예시

```html
<address>
	<a href="mailto:jim@rock.com">jim@rock.com</a><br />
	<a href="tel:+13115552368">(311) 555-2368</a>
</address>
```

> `<a href="mailto:...">` : 이 요소를 클릭하면, 각 플랫폼에서 지원하는 메일 쓰기 페이지/앱으로 이동합니다.

> `<a href="tel:...">` : 이 요소를 클릭하면, 각 플랫폼에서 지원하는 전화 앱으로 이동합니다.

<br>

## Text content

다음은 문자 콘텐츠를 나타내는 태그입니다.

- `<div>`

- `<ul>`/`<ol>`/`<li>`

<br>

## `<div>`

> 아무 의미가 없는 영역을 나타냅니다. 스타일링 목적으로 사용합니다. (Division)

<br>

## `<ul>`/`<ol>`/`<li>`

> 순서가 있는 목록(`<ol>`) / 순서가 없는 목록(`<ul>`)에 사용합니다. `<li>` 태그는 목록의 각 아이템입니다.

- 항목의 순서를 바꿨을 때 의미도 바뀐다면 `<ol>`을 사용하세요.

  > 예를 들어, 단계별 요리법, 내비게이션, 영양정보에서 비율의 내림차순으로 정렬한 원재료 목록 등에서요.

  > 목록의 넘버링이 (순서로서의 의미를 가지기 보다) 법/기술 문서 등에서 단순한 인덱스의 역할만 한다면, `<ol>` 태그보다는 CSS의 `list-style-type` 속성을 사용하세요.

<br>

### `<ol>`

- `type` : 넘버링 타입 (`<li>` 태그에 별도로 `type` 속성을 명시하지 않으면 `<ol>` 태그의 `type` 속성이 적용됨)

  - `a` : lowercase letters
  - `A` : uppercase letters
  - `i` : lowercase Roman numerals
  - `I` : uppercase Roman numerals
  - `1` : numbers (default)

- `reversed` : 역순 정렬 (`true`/`false` 또는 `reversed`)

- `start` : 아이템에 넘버링을 할 때 첫 번째 순서를 나타내는 숫자
  > `type` 값이 `A`와 같이 문자이더라도, `start` 속성 값은 `3`과 같이 숫자만 지정할 수 있습니다.

<br>

#### 예시

```html
<ol type="1" reversed>
	<li>Mix flour, baking powder, sugar, and salt.</li>
	<li>In another bowl, mix eggs, milk, and oil.</li>
	<li>Stir both mixtures together.</li>
	<li>Fill muffin tray 3/4 full.</li>
	<li>Bake for 20 minutes.</li>
</ol>
```

<br>

### `<li>`

- `<li>` 태그의 `value` 속성은 해당 아이템을 시작으로 넘버링하는데 사용합니다.

```html
<ol>
	<li value="1">Mix flour, baking powder, sugar, and salt.</li>
	<li>In another bowl, mix eggs, milk, and oil.</li>
	<li>Stir both mixtures together.</li>
	<li>Fill muffin tray 3/4 full.</li>
	<li>Bake for 20 minutes.</li>
</ol>
```

<br>

## `<dl>`/`<dt>`/`<dd>`

> `<dl>` 태그는 설명 그룹의 목록을 나타냅니다. 설명 그룹은 용어(`<dt>`) + 정의(`<dd>`)를 한 그룹으로 합니다. 주로 용어사전 구현이나 메타데이터(키-값 쌍 목록)를 표시

> Definition Term / Definition Details / Description List

- 페이지에서 들여쓰기를 하기 위한 목적으로 `<dl>` (또는`<ul>`) 요소를 사용하지 마세요. 원래 목적을 흐립니다.

- 용어의 들여쓰기를 수정하려면 CSS의 `margin` 속성을 사용하세요.

<br>

### Styling

용어 그룹을 묶어서 스타일링할 때 `<div>` 태그를 사용할 수 있습니다.

<br>

```html
<dl>
	<div>
		<dt>Beast of Bodmin</dt>
		<dd>A large feline inhabiting Bodmin Moor.</dd>
	</div>

	<div>
		<dt>Morgawr</dt>
		<dd>A sea serpent.</dd>
	</div>

	<div>
		<dt>Owlman</dt>
		<dd>A giant owl-like creature.</dd>
	</div>
</dl>
```

<br>

### 웹 접근성

[각각의 스크린 리더는 `<dl>`을 다르게 표현합니다](https://cdpn.io/aardrian/debug/NzGaKP). iOS VoiceOver 등 일부 스크린 리더는 `<dl>` 요소를 목록으로 표현하지 않습니다. 따라서, 각 아이템의 콘텐츠는 리스트 그룹 내 다른 항목과의 관계를 표현할 수 있는 방식으로 작성해야 합니다.

<br>

웹 접근성을 고려하여 `<ul>`과 `<dfn>` 태그를 사용할 수 있겠네요.

> `<dfn>`은 용어 하나를 정의하는 태그입니다.

```html
<ul>
	<li>
		<dfn>Beast of Bodmin</dfn>
		<p>A large feline inhabiting Bodmin Moor.</p>
	</li>

	<li>
		<dfn>Morgawr</dfn>
		<p>A sea serpent.</p>
	</li>

	<li>
		<dfn>Owlman</dfn>
		<p>A giant owl-like creature.</p>
	</li>
</ul>
```

<br>

## `<p>`

> 하나의 문단을 나타냅니다. HTML에서 문단은 이미지, 입력 폼 등 서로 관련있는 콘텐츠 무엇이나 될 수 있습니다.

- 콘텐츠를 문단으로 나누면 페이지의 접근성을 높입니다. 스크린 리더 등의 보조 기기는 `<p>` 태그를 사용하여 다음 문단으로 넘어갈 수 있는 단축키 등을 제공합니다.

- 문단 사이에 여백을 추가하기 위해 빈 `<p>` 요소를 사용하지 마세요. 스크린 리더 사용자가 혼란스러울 수 있습니다. 스크린 리더가 문단의 존재는 알려주지만, 읽을 문단의 내용이 없기 때문입니다.

<br>

### 닫는 태그 생략

자신의 닫는 태그(`</p>`) 이전에 다른 블록 레벨 태그가 분석되면 자동으로 닫힙니다.

> 닫는 태그는 `<p>` 요소의 바로 뒤에 `<address>`, `<article>`, `<aside>`, `<blockquote>`, `<div>`, `<dl>`, `<fieldset>`, `<footer>`, `<form>`, `<h1>`-`<h6>`, `<header>`, `<hr>`, `<menu>`, `<nav>`, `<ol>`, `<pre>`, `<section>`, `<table>`, `<ul>`, `<p>` 요소가 위치하는 경우 생략할 수 있습니다. 또는 부모 태그의 콘텐츠가 더 존재하지 않고 부모가 요소가 `<a>`가 아닐 때 생략할 수 있습니다.

<br>

## `<hr />`

> 문단을 분리합니다. 이야기 장면 전환, 구획 내 주제 변경 등 문단 레벨에서 주제의 분리를 나타냅니다.

> (Horizontal Rule)

- 아무 의미없이 스타일링을 위해 가로줄을 그리고 싶다면 `<hr>` 태그가 아닌 적절한 CSS를 사용해야 합니다.
  > `<hr>`은 브라우저에서 가로줄로 그려지는 시각적 기능이 있지만, 시각적 표현에 그치지 않고 의미를 가지게 됐습니다.

<br>
<br>
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
- [\<ol\>: The Ordered List element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol)
- [\<dl\>: The Description List element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl)
- [\<p\>: The Paragraph element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p)
