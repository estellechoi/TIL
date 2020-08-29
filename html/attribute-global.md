# 전역 속성(Global Attributes)

전역 속성(Global Attributes)은 모든 HTML 요소에서 공통적으로 사용 가능한 속성입니다. 아래는 전역 속성 목록입니다.

- `class`

- `id`

- `style`

- `title`

- `lang`

- `data-`

- `draggable`

- `hidden`

- `tabindex`

- `itemprop`

<br>

## `class`

요소에 접근할 수 있는 식별자 입니다. 한 요소에 여러 `class`를 지정할 수 있으며, 공백으로 구분합니다.

<br>

예를 들어,

```html
<div class="greeting hi">Hi</div>
```

위 요소는 2 개의 클래스 `greeting`, `hi`를 가지고 있습니다.

<br>

## `id`

요소를 나타내는 유일한 식별자 입니다. (Identifier)

<br>

## `style`

요소에 지정할 CSS를 작성합니다.

```html
<span style="color: tomato">Tomato</span>
```

<br>

## `title`

요소에 대한 설명(정보)를 제공합니다.

<br>

## `lang`

요소에서 사용하는 주요 언어를 지정합니다.

```html
<p lang="en">English section</p>
<p lang="ko">한글입니다.</p>
<p lang="fr">Ce paragraphe est défini en français.</p>
```

<br>

## `data-` (Prefix)

사용자 정의 데이터 속성을 지정할 때 사용하는 Prefix 입니다. 보통, JavaScript 코드에서 사용할 데이터를 HTML 요소에 저장하기 위해 사용합니다.

이렇게 사용합니다.

```html
<div id="paris" data-city-code="001" data-city-label="Paris">Paris</div>
```

```javascript
const $paris = document.getElementById("paris");
console.log($paris.dataset.cityCode); // "001"
console.log($paris.dataset.cityLabel); // "Paris"
```

<br>

## `draggable`

Boolean 속성입니다. 이 속성을 지정하면 해당 요소를 드래그(Drag)할 수 있습니다. 기본값은 `auto` 이므로, 드래그 하기를 원하면 아래와 같이 `draggable="true"`라고 명시해야 합니다.

```html
<div draggable="true">Drag me</div>
```

<br>

> [draggable | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable) 페이지를 참고하세요.

<br>

## `hidden`

해당 요소가 아직, 또는 더 이상 관련이 없음을 나타내는 Boolean 속성입니다. 브라우저는 `hidden` 속성을 가진 요소를 렌더링 하지 않습니다. 시각적 방식 외에도 스크린 리더 등 다른 모든 표시 방식에서 숨겨집니다.

<br>

## `tabindex`

`Tab` 키를 이용해 요소를 순차적으로 포커스(Focus) 탐색할 순서를 지정합니다. `tabindex="0"`인 요소들은 HTML 마크업 코드 순서대로 `Tab` 이동 순서가 지정됩니다. 보통 [대화형 콘텐츠](https://developer.mozilla.org/ko/docs/Web/Guide/HTML/Content_categories#%EB%8C%80%ED%99%94%ED%98%95_%EC%BD%98%ED%85%90%EC%B8%A0)들은 `tabindex="0"` 입니다. `tabindex` 속성이 없는 요소는 탐색할 수 없고요.

예를 들어, 아래의 4 개의 `<input />` 요소들은 `tabindex="0"` 값을 기본으로 가지고 있습니다. 따라서 마크업 순서대로 Tab 탐색이 가능합니다.

```html
<input type="text" placeholder="Name" />
<input type="text" placeholder="Age" />
<input type="text" placeholder="Gender" />
<input type="text" placeholder="Nationality" />
```

<br>

### `tabindex="-1"`

어떤 대화형 콘텐츠 요소를 Tab 탐색에서 제외하고 싶으면, `tabindex="-1"`을 지정합니다. (Focus는 여전히 가능)

<br>

### `tabindex="1"`

`tabindex="1"` 이상의 양수 값을 가진 요소들을 가장 먼저 탐색합니다. 양수 값을 가진 요소들의 탐색이 끝나면 `tabindex="0"`인 요소로 이동하게 됩니다. 이는 논리적 흐름을 방해하기 때문에 되도록 사용하지 마세요.

예를 들어, 아래와 같이 `tabindex` 값을 지정하면 마크업 순서와 전혀 맞지 않게 중구난방으로 Tab 탐색이 됩니다.

```html
<!-- 4th -->
<input type="text" placeholder="Name" />
<!-- 1st -->
<input type="text" placeholder="Age" tabindex="1" />
<!-- 3rd -->
<input type="text" placeholder="Gender" tabindex="3" />
<!-- 2nd -->
<input type="text" placeholder="Nationality" tabindex="2" />
```

<br>

## `itemprop`

> [저의 다른 글](https://github.com/estellechoi/TIL/blob/master/html/microdata.md)과 함께 읽어보세요. [Google Search](https://developers.google.com/search/docs/data-types/article)에서 키워드별 예제도 확인할 수 있습니다.

<br>

마이크로데이터(Microdata)에 속성(Property)을 가진 아이템(Item)을 추가하기 위해 사용합니다. 더 직관적으로 말하자면, 마크업 문서의 내용들을 데이터화하기 위해 사용하는 속성이고요, 검색엔진최적화(SEO)와 관련이 있습니다. 대소문자를 구분하며, 각 데이터 아이템은 `이름 - 값` 쌍으로 이루어진 속성(Itemprop)들로 구성됩니다. 속성의 값으로는 일반적인 문자열(String) 또는 URL을 지정할 수 있습니다.

> 주의 : `itemprop` 속성이 마크업된 문서의 실제 콘텐츠와 아이템 속성들로 구성된 데이터 구조는 서로 아무런 관계가 없습니다.

<br>

예를 들어 아래의 마크업을 이용해 데이터 구조를 만들고 싶다면,

```html
<h1>Avatar</h1>
```

<br>

아래와 같이 하세요.

```html
<h1 itemprop="title">Avatar</h1>
```

내부 텍스트인 `Avatar`가 영화 제목을 의미하므로, `title`을 아이템의 이름으로 지정해봅시다. `itemprop` 속성(Attribute)을 이용하여 아이템의 이름을 지정하면 됩니다. 이 아이템의 속성(Itemprop) 값은 내부 텍스트인 `Avatar`가 됩니다. 이로써 `이름 - 값`에 대응하는 `title - Avatar`라는 속성이 만들어졌고요, 이렇게 하나 이상의 속성으로 묶인 그룹을 아이템 이라고 합니다.

<br>

### `itemscope`: 속성 그루핑

아이템은 하나 이상의 속성으로 구성된 그룹을 의미합니다. 즉, 여러 속성(Itemprop)들을 하나의 그룹으로 묶을 수 있습니다. 그루핑하려는 마크업 요소들의 상위 요소에 `itemscope` 속성을 지정하세요. 아래와 같이 `itemscope` 속성이 지정된 요소의 하위 요소들에 기반하여 만들어진 모든 속성(Itemprop)들이 하나의 스코프(Itemscope)로 그루핑됩니다.

```html
<div itemscope itemtype="http://schema.org/Movie">
	<h1 itemprop="title">Avatar</h1>
	<span
		>Director:
		<span itemprop="director">James Cameron</span>
		(born August 16, 1954)</span
	>
	<span itemprop="genre">Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html" itemprop="trailer"
		>Trailer</a
	>
</div>
```

<br>

하나의 그룹에 속하게 된 속성들로부터 아래와 같은 데이터 구조를 도출할 수 있습니다. 참고로, `<a>` 태그와 같이 URL을 포함하는 요소의 경우 내부 텍스트가 아닌 `href`의 값이 속성(Itemprop)의 `값`에 해당하고요.

| 이름     | 값                                       |
| -------- | ---------------------------------------- |
| title    | Avatar                                   |
| director | James Cameron                            |
| genre    | Science fiction                          |
| trailer  | ../movies/avatar-theatrical-trailer.html |

<br>

`itemscope`으로 묶인 그룹 자체가 다른 아이템의 속성(Itemprop)이 될 수도 있습니다.

```html
<div itemscope>
	<p>Name: <span itemprop="name">Amanda</span></p>
	<p>
		Band:
		<span itemprop="band" itemscope>
			<span itemprop="name">Jazz Band</span>
			(<span itemprop="size">12</span> players)</span
		>
	</p>
</div>
```

위 마크업에서 가장 상위의 아이템(Item)은 2 개의 속성(Itemprop)들로 이루어진 속성 그룹입니다. 2 개의 속성 중 `band` 속성은 단순한 값이 아닌, 2 개 속성들(`name`, `size`)로 구성된 또 다른 아이템 입니다. 위의 데이터 구조 전체를 객체 형태로 정리해보면 아래와 같습니다.

```json
{
	"name": "Amanda",
	"band": {
		"name": "Jazz Band",
		"size": 12
	}
}
```

<br>

### 토큰(Tokens)

아이템은 순서가 없는 고유한 토큰들(Tokens)로 이루어진 집합입니다. 토큰은 `이름 - 값`으로 구성된 하나의 속성을 나타내며, 사실상 `이름`에 해당하는 값을 가리킵니다. 토큰은 문자열이거나 URL일 수 있는데요, 토큰이 URL인 아이템을 타입이 있는 아이템(Typed Item)이라고 말합니다. 참고로, 동일한 이름의 토큰이 2 개 이상 존재하면 해당 이름의 토큰들은 순서를 가지게 됩니다. (토큰들 간에는 순서가 없음)

> 주의 : URL 토큰(Typed Item) 외에는, 토큰의 값에 콜론(`:`)과 점(`.`)을 사용할 수 없습니다. URL 토큰과 혼동될 수 있기 때문입니다. 또한, 스페이스(` `) 역시 포함할 수 없습니다. 여러 개의 토큰으로 잘못 파싱될 수 있기 때문입니다.

<br>

### `itemref` 속성을 사용해서 외부에 있는 속성들을 포함하기

이는 `<input>`과 `<label>` 요소를 연결하는 방식과 거의 똑같습니다. 자신의 하위에 있지 않은 아이템 요소의 속성을 포함하려면 해당 요소의 `id`를 자신의 `itemref` 속성 값에 지정하는 방법으로 연결하세요.

```html
<div itemscope id="amanda" itemref="a b"></div>

<p id="a">Name: <span itemprop="name">Amanda</span></p>
<div id="b" itemprop="band" itemscope itemref="c"></div>
<div id="c">
	<p>Band: <span itemprop="name">Jazz Band</span></p>
	<p>Size: <span itemprop="size">12</span> players</p>
</div>
```

<br>

### 같은 이름의 속성을 여러 개 포함하기

아래의 예시와 같이 이름이 완전이 똑같은 속성을 여러 개 가질 수 있습니다.

```html
<div itemscope>
	<p>Flavors in my favorite ice cream:</p>
	<ul>
		<li itemprop="flavor">Lemon sorbet</li>
		<li itemprop="flavor">Apricot sorbet</li>
	</ul>
</div>
```

<br>

### 같은 값을 가지는 속성들을 한 번에 지정하기

아래와 같이 `orange`라는 같은 값을 가진 속성을 2 개 지정할 수 있습니다.

```html
<div itemscope>
	<span
		itemprop="favorite-color
    favorite-fruit"
		>orange</span
	>
</div>
```

<br>

객체 형태로 나타내면 아래와 같습니다.

```json
{
	"favorite-color": "orange",
	"favorite-fruit": "orange"
}
```

<br>

### `<meta>` 태그와 함께 사용하기

`content` 속성 값을 아이템의 속성 값으로 사용합니다.

<br>

### `<audio>`, `<embed>`, `<iframe>`, `<img>`, `<source>`, `<track>`, `<video>` 태그와 함께 사용하기

`src` 속성 값을 문자열(String)로 파싱한 값을 아이템의 속성 값으로 사용합니다.

<br>

### `<a>`, `<area>`, `<link>` 태그와 함께 사용하기

`href` 속성 값을 문자열(String)로 파싱한 값을 아이템의 속성 값으로 사용합니다.

<br>

### `<object>` 태그와 함께 사용하기

`data` 속성 값을 문자열(String)로 파싱한 값을 아이템의 속성 값으로 사용합니다.

<br>

### `<data>` 태그와 함께 사용하기

`value` 속성 값을 아이템의 속성 값으로 사용합니다.

<br>

### `<meter>` 태그와 함께 사용하기

`value` 속성 값을 아이템의 속성 값으로 사용합니다.

```html
<div
	itemprop="aggregateRating"
	itemscope
	itemtype="http://schema.org/AggregateRating"
>
	<meter itemprop="ratingValue" min="0" value="3.5" max="5">Rated 3.5/5</meter>
	(based on <span itemprop="reviewCount">11</span> reviews)
</div>
```

<br>

### `<time>` 태그와 함께 사용하기

`datetime` 속성 값을 아이템의 속성 값으로 사용합니다.

```html
<div itemscope>
	I was born on
	<time itemprop="birthday" datetime="2009-05-10">May 10th 2009</time>.
</div>
```

<br>

---

### References

- [draggable | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable)
- [hidden | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden)
- [한눈에 보는 HTML 요소(Elements & Attributes) 총정리 | HEROPY Tech](https://heropy.blog/2019/05/26/html-elements/)
- [itemprop | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/itemprop)
- [WHATWG HTML Microdata feature](https://html.spec.whatwg.org/multipage/microdata.html#microdata)
- [Getting started with schema.org using Microdata](https://schema.org/docs/gs.html)
- [HTML Microdata | W3C Working Draft 26 April 2018](https://www.w3.org/TR/microdata/)
