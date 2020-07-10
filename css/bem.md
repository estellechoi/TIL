# CSS 방법론: BEM(Block, Element, Modifier)

<br>

## CSS 방법론(CSS Methodology)

CSS 방법론이란, CSS 작성시 클래스(`class`) 작명을 일관되고 구조적으로 할 수 있는 방법에 대한 가이드입니다. BEM과 같은 CSS 방법론을 적용함으로써 달성하고자 하는 골은 아래와 같습니다.

- HTML 요소의 모듈화 (재사용성, 확장성)

- 쉬운 유지보수 (시멘틱 클래스(Semantic Class))
  > Semantic class naming is a style in which we use class names that describe what an element is, or it’s intended purpose, rather than how it looks. - Ukiah Smith's Blog

<br>

> [여기](https://2019.stateofcss.com/technologies/methodologies/)에서 BEM, Atomic CSS, OOCSS 등 CSS 방법론 동향을 확인하세요.

<br>

## BEM(Block, Element, Modifier)

BEM은 CSS 방법론 중 하나입니다. `header__nav--secondary`와 같은 클래스명은 본적이 있다면, 이것이 바로 BEM 방법론을 적용한 작명입니다. BEM 방법론은 어떤 HTML 요소에 대해 클래스 작명을 할 때 그 요소를 3 가지 관점으로 나누어 구분합니다.

- Block

- Element

- Modifier

<br>

위의 3 가지 관점은 클래스 작명에 아래와 같이 반영됩니다.

```
.block__element--modifier { .. }
```

<br>

> 중요

BEM 방법론을 사용할 때 `id` 셀렉터나 CSS 태그를 사용해서는 안되며, 오직 `class` 셀렉터만을 사용해야 합니다.

<br>

## Block

> 다음은 BEM 공식 문서에 정의된 Block의 정의입니다.

<strong>A functionally independent page component that can be reused.</strong>

기능적으로 독립적이며, 재사용이 가능한 페이지의 한 컴포넌트.

<br>

이때 Block의 이름은 Block의 상태가 아닌, 목적과 기능을 나타내야 합니다. 가령, `menu`, `button`은 Block의 이름으로 적합하지만, `red`, `block`과 같이 상태를 나타내는 이름은 적합하지 않습니다.

예를 들면,

```html
<div class="header">
	<div class="menu">..</div>
	<div class="search">..</div>
</div>
```

위의 `header`, `menu`, `search`는 모두 Block을 위한 클래스 이름입니다. 또, 다음과 같은 것들이 Block의 클래스 이름으로 사용될 수 있습니다.

`login-form` / `menu` / `search-form` / `content` / `footer`

<br>

### Block 사용 가이드라인

- Block은 외부 환경에 영향을 미쳐서는 안됩니다. 가령, `margin` 속성으로 외부 레이아웃에 영향을 주거나, `position` 속성으로 이 블록의 위치를 지정하지 마십시오.

- Block은 서로 중첩될 수 있습니다. 예를 들어, 이렇게요. (Nesting)

```html
<!-- `header` block -->
<header class="header">
	<!-- Nested `logo` block -->
	<div class="logo"></div>

	<!-- Nested `search-form` block -->
	<form class="search-form"></form>
</header>
```

<br>

## Element

> BEM 공식 문서에 정의된 Element

<strong>A composite part of a block that can't be used separately from it.</strong>

각 Block을 구성하는 부분 요소. 자신이 속해 있는 블록과 독립적으로 사용될 수 없습니다.

<br>

Block과 마찬가지로 Element의 클래스 이름은 요소의 상태가 아닌, 목적과 기능을 나타내야 합니다. 예를 들어, `input`, `button` 등은 Element의 클래스 이름으로 적절합니다.

```html
<!-- block -->
<form class="search-form">
	<!-- element in the `search-form` block -->
	<input class="search-form__input" />

	<!-- element in the `search-form` block -->
	<button class="search-form__button">Search</button>
</form>
```

<br>

위의 마크업 예시에서 클래스 이름을 짓는 규칙을 발견하셨나요? Element 요소의 클래스 이름은 아래의 규칙을 따릅니다.

```
block-name__element-name
```

- Block과 Element 각각의 이름을 정할 때 띄어쓰기는 하이픈(`-`)으로 나타냅니다.

- Block - Element 관계는 2 개의 밑줄(`__`)로 나타냅니다.

<br>

### Element 사용 가이드라인

- 모든 Element는 한 Block의 하위 요소이며, Block과 분리하여 독립적으로 사용할 수 없습니다. (Membership)

- 모든 Block이 Element를 가질 필요는 없습니다. (Optionality)

- Block과 마찬가지로 중첩이 가능합니다. (Nesting)

<br>

다만,

> In the BEM methodology, you can't create elements of elements.

Element는 다른 Element의 하위 요소가 될 수 없습니다. 가령, `block__elem1__elem2`와 같은 계층 구조로 클래스 작명을 해서는 안됩니다.

예를 들어, 실제 마크업을 할 때 아래와 같이 클래스 이름을 지으면 됩니다. Element가 중첩 관계에 있더라도 클래스명은 Element간의 계층 구조를 반영해서는 안됩니다. 이는 우리의 목표인 확장성과 재사용성을 높이는데 도움이 됩니다. 각 Element에 대한 CSS 코드를 변경하지 않고도 Block 내에서 DOM 구조를 언제든지 바꿀 수 있도록 하기 때문입니다.

```html
<form class="search-form">
	<!-- element -->
	<div class="search-form__content">
		<!-- element -->
		<input class="search-form__input" />
		<!-- element -->
		<button class="search-form__button">Search</button>
	</div>
</form>
```

<br>

## Modifier

> BEM 공식 문서에 정의된 Modifier

<strong>An entity that defines the appearance, state, or behavior of a block or element.</strong>

Block/Element의 외형, 상태, 행동 등을 정의하는 객체.

<br>

Modifier 이름은 요소의 외형(`size-s`, `theme-islands`) 또는 요소의 상태(`disabled`, `focused`), 요소의 행동(`directions-left-top`)을 나타냅니다. Block/Element와는 하이픈(`--`) 2 개로 구분합니다.

```
block-name__element-name--modifier-name
```

<br>

### Modifier 사용 가이드라인

- Modifier 클래스는 Block/Element 클래스와 반드시 함께 사용해야 됩니다. Modifier는 기존 Block/Element의 외형, 행동, 상태를 변경하는 역할만 해야하며, Block/Element의 전체 스타일을 대체해서는 안되기 때문입니다.

<br>

예를 들어, 아래의 마크업은 적합합니다. Modifier 클래스인 `search-form--theme-islands`가 그 대상인 기존의 Block 클래스 `search-form`와 함께 쓰였기 때문입니다.

```html
<form class="search-form search-form--theme-islands">
	<input class="search-form__input" />

	<button class="search-form__button">Search</button>
</form>
```

<br>

반면 아래 마크업은 적합하지 않습니다.

```html
<!-- Incorrect. The modified class `search-form` is missing -->
<form class="search-form--theme-islands">
	<input class="search-form__input" />

	<button class="search-form__button">Search</button>
</form>
```

<br>

### Modifier의 종류

#### Boolean

Boolean Modifier는 존재 자체로 그 값이 `true`로 간주되는 Modifier 입니다. `disabled`, `focused` 등이 그 예입니다.

```html
<!-- `focused` Boolean modifier -->
<form class="search-form search-form--focused">
	<input class="search-form__input" />

	<!-- `disabled` Boolean modifier -->
	<button class="search-form__button search-form__button--disabled">
		Search
	</button>
</form>
```

예를 들어, 위의 클래스 `search-form--focused`는 그 자체로 해당 요소가 focused 되었음을 나타냅니다.

<br>

#### Key-value

Key-value Modifier는 이름과 값으로 구성됩니다. Modifier의 '이름 - 값' 관계는 하이픈(`-`) 1 개로 구분합니다.

```
block-name__element-name--modifier-value
```

아래는 예시 마크업 입니다.

```html
<!-- `theme` modifier with the value `islands` -->
<form class="search-form search-form--theme-islands">
	<input class="search-form__input" />

	<!-- `size` modifier with the value `m` -->
	<button class="search-form__button search-form__button--size-m">
		Search
	</button>
</form>
```

<br>

## Mix

Mix는 서로 다른 레벨의 클래스를 섞어 사용하는 기술입니다. 예를 들어,

```html
<!-- `header` block -->
<div class="header">
	<div class="search-form header__search-form"></div>
</div>
```

위와 같이 Block 클래스인 `search-form`과 Element 클래스 `header__search-form`을 함께 사용할 수 있습니다. 그런데, Mix는 왜 필요할까요?

<br>

Block 클래스인 `search-form`에는 `margin`, `position` 등의 속성을 이용해 Block의 위치나 마진을 지정할 수 없습니다. Block 클래스는 어디에서든 재사용할 수 있어야 하므로 외부 환경에 영향을 주면 안되기 때문입니다. 그럼, `search-form` Block의 위치나 마진은 어디에 지정해야 할까요? 바로 이런 경우에 Mix를 사용합니다. 함께 쓰인 Element 클래스인 `header__search-form`에 지정하면 됩니다. 이를 통해 `search-form` Block 클래스는 재사용성과 독립성을 유지하면서, 실제 DOM의 노드에는 위치나 마진을 지정할 수 있게 됩니다.

<br>

## 파일 구조

위에서 살펴본 Block, Element, Modifier 클래스를 작성하는 파일은 어떻게 나누고, 디렉토리 구조는 어떻게 하는게 좋을까요? 이에 대해서도 몇 가지 방법론이 있습니다. 아래는 가장 추천되는 Nested 파일 구조를 구성하기 위한 항목들입니다.

- Block마다 디렉토리가 필요하며, 디렉토리 이름은 Block 이름과 동일하게 합니다. 예를 들어, `header` Block은 `header/` 디렉토리 내에 있어야 합니다.

- Element/Modifier 클래스의 디렉토리는 자신이 속한 Block 디렉토리 하위에 위치합니다.

- Element 디렉토리의 이름은 2 개의 밑줄(`__`)과 Element 이름을 붙여서 만듭니다. 예를 들면, `header/__logo/` 이렇게요.

- Modifier 디렉토리의 이름은 1 개의 밑줄(`_`)과 Modifier 이름을 붙여서 만듭니다. 예를 들면, `menu/_theme_islands/` 이렇게요.

- Block, Element, Modifer 각각의 디렉토리에는 실제 요소를 구현하기 위한 CSS 파일과 JavaScript 파일이 위치합니다. 예를 들어, `header/` 디렉토리에는 `header.css` 파일과 `header.js` 파일이 있겠죠.

<br>

이 외에도 [Flat](https://en.bem.info/methodology/filestructure/#flat), [Flex](https://en.bem.info/methodology/filestructure/#flex) 파일 구조가 있습니다.

<br>

---

### References

- [BEM](https://en.bem.info/methodology/quick-start/)
- [Get BEM](http://getbem.com/)
- [Semantic Class Naming](https://ukiahsmith.com/blog/semantic-class-naming/)
