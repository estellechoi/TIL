# CSS Architecture: BEM(Block, Element, Modifier)

<br>

## CSS 방법론(CSS Architecture)

CSS 방법론이란, CSS 작성시 클래스(`class`) 작명을 일관되고 구조적으로 할 수 있는 방법에 대한 가이드입니다. BEM과 같은 CSS 방법론을 적용함으로써 달성하고자 하는 골은 아래와 같습니다.

- HTML 요소의 모듈화 (재사용성, 확장성)

- 쉬운 유지보수 (시멘틱(Semantic) 클래스명)
  > 시멘틱 클래스명 : 클래스 이름만으로 무슨 의미인지 예측 가능

<br>

## BEM(Block, Element, Modifier)

BEM은 CSS 방법론 중 하나입니다. `header__nav‐‐secondary`와 같은 클래스명은 본적이 있다면, 이것이 바로 BEM 방법론을 적용한 작명입니다. BEM 방법론은 어떤 HTML 요소에 대해 클래스 작명을 할 때 그 요소를 3 가지 관점으로 나누어 구분합니다.

- Block에

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

다만, Element는 다른 Element의 하위 요소가 될 수 없습니다. 가령, `block__elem1__elem2`와 같은 계층 구조로 클래스 작명을 해서는 안됩니다.

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

---

### References

- [BEM Documentation](https://en.bem.info/methodology/quick-start/)
