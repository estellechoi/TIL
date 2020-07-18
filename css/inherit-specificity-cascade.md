# 상속(Inherit), 명시도(Specificity), 계단식 적용(Cascade)

<br>

## 상속(Inherit)

상위 요소에 지정한 CSS 속성 값을 후손 요소들이 상속 받아 자동으로 적용되는 개념입니다.

<br>

### 상속되는 속성들(Properties)

다음은 대표적으로 스타일 상속이 일어나는 속성들입니다. 보통 Text 스타일을 위한 속성들이네요.

- `font`

  - `font-size`
  - `font-weight`
  - `font-size`
  - `font-family`

- `line-height`

- `color`

- `text-align`

- `text-indent`

- `text-decoration`

- `letter-spacing`

- `opacity`

<br>

### 강제 상속: `inherit`

상속되지 않는 속성의 값들을 강제로 상속받게 할 수 있습니다. 어떤 속성에 대한 값으로 `inherit` 키워드를 사용하면 부모 요소로부터 해당 속성의 계산값([Computed value](https://developer.mozilla.org/en-US/docs/Web/CSS/computed_value))을 받아와서 사용합니다. CSS의 [all](https://developer.mozilla.org/ko/docs/Web/CSS/all) 단축 속성을 포함한 모든 속성에서 사용할 수 있습니다.

<br>

```css
.parent {
	position: absolute; /* non-inherited property */
}

.child {
	position: inherit;
}
```

<br>

## 스타일 충돌(Conflicting rules)

같은 요소에 서로 충돌하는 CSS 속성 값을 선언하면 어떤 규칙에 따라 최종적으로 가장 우선하는 값이 적용됩니다. 어떤 스타일을 우선할지 결정하는 규칙에는 3 가지가 있는데요, 위에서 보았던 상속(Inherit), 계단식(Cascade), 명시도(Specificity)입니다.

<br>

## 계단식(Cascade)

> CSS stands for Cascading Style Sheets.

아주 간단합니다. 명시도(Specificity)가 같다면 나중에 선언된 스타일이 우선합니다. 이를 폭포식, 계단식(Cascading) 스타일이라고 합니다.

<br>

## 명시도(Specificity)

CSS를 명시적으로 선언하였을 때, 어떤 방식의 명시도가 더 높다고 할 수 있을까요?

```html
<div class="block" id="font-yellow" style="color: tomato;">
	A Block
</div>
```

예시 마크업을 봅시다. 이제 이 요소의 글자 색을 지정할 건데요, 여러 방법으로 CSS를 선언했을 때 무엇이 가장 우선할까요(명시도가 높을까요)? 아래에 명시도가 낮은 방식부터 나열하였습니다. 즉, 아래로 갈수록 우선 적용되는 스타일 선언 방식이며, 위에서 적용한 스타일이 덮어쓰기 된다고 보면 됩니다.

같은 레벨의 명시도를 가지고 있다면, 나중에 작성한 스타일이 기존의 스타일을 덮어쓰면서 우선합니다. (Cascade)

<br>

### 1) Inherit

```css
body {
	color: BurlyWood;
}
```

<br>

### 2) Wildcard selector (`*`)

```css
* {
	color: Coral;
}
```

<br>

### 3) Tag selector

```css
div {
	color: Crimson;
}
```

<br>

### 4) Class selector

```css
.block {
	color: CadetBlue;
}
```

<br>

### 5) Id selector

```css
#font-yellow {
	color: yellow;
}
```

<br>

### 6) Inline style

```html
<div style="color: tomato;">..</div>
```

<br>

### 7) `!important`

모든 선언방식보다 중요합니다. 무조건 이 스타일을 적용합니다.

```css
div {
	color: lightyellow !important;
}
```

<br>

---

### References

- [Cascade and inheritance | MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)
- [inherit | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/inherit)
