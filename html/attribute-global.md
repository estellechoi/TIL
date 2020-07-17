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

해당 요소가 아직, 또는 더 이상 관련이 없음을 나타내는 Boolean 속성입니다. 브라우저는 `hidden` 속성을 가진 요소를 렌더링 하지 않습니다. 그 시각적 방식 외에도 스크린 리더 등 다른 모든 표시 방식에서 숨겨집니다.

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

---

### References

- [draggable | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable)
- [hidden | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden)
- [한눈에 보는 HTML 요소(Elements & Attributes) 총정리 | HEROPY Tech](https://heropy.blog/2019/05/26/html-elements/)
