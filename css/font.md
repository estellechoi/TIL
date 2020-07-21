# 폰트(Font)의 모든 것

<br>

## CSS 기본 속성

CSS 기본 속성을 이용하며 폰트를 스타일링할 수 있습니다. 아래는 속성 목록입니다.

- `font-style` : 기본값 `normal` / 폰트 기울기 지정

- `font-weight` : 기본값 `normal`

- `font-family`

- `font-size` : 기본값 `medium`(`16px`)

- `line-height` : 기본값 `normal`

<br>

### 1) `font-style`

- `normal`

- `italic`

- `oblique`

- `oblique <angle>`

<br>

### 2) `font-weight`

- `normal` : 보통 (`400`)

- `bold` : 두껍게 (`700`)

- `bolder` : 부모 요소보다 두껍게

- `lighter` : 부모 요소보다 얇게

- `100`-`900` : 사이의 숫자로 지정

<br>

`font-weight`의 값으로 사용되는 `100`-`900` 숫자들은 일반적으로 폰트 이름에서 아래와 같이 사용됩니다. 예를 들어, `Arial Black` 폰트는 두께 값이 `900`인 `Arial` 폰트라고 볼 수 있습니다.

| Number | Font Name Suffix              |
| ------ | ----------------------------- |
| `100`  | `Thin` (`Hairline`)           |
| `200`  | `Extra Light` (`Ultra Light`) |
| `300`  | `Light`                       |
| `400`  | `Normal`                      |
| `500`  | `Medium`                      |
| `600`  | `Semi bold` (`Demi Bold`)     |
| `700`  | `Bold`                        |
| `800`  | `Extra Bold` (`Ultra Bold`)   |
| `900`  | `Black` (`Heavy`)             |

<br>

## 단축 속성

기본 속성들을 한 번에 지정할 수 있는 단축 속성입니다.

- `font`

<br>

값은 아래와 같이 작성하되, `font-size`, `font-family` 값은 필수로 포함하고 다른 값들은 생략할 수 있습니다. 또한, `line-height` 값은 `/`를 사용해서 `font-size` 값과 명확하게 구분하는 것이 문법입니다.

```css
font: font-style font-weight font-size / line-height font-family;
```

<br>

### 예시

```css
.box {
	font: italic bold 20px / 1.5 "Arial", sans-serif;
}
```

---

### Reference

- [font-style | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)
