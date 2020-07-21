# 폰트(Font)의 모든 것

<br>

## CSS 기본 속성

CSS 기본 속성을 이용하며 폰트를 스타일링할 수 있습니다. 아래는 속성 목록입니다.

- `font-style` : 폰트 기울기

- `font-weight` : 폰트 두께

- `font-family` : 폰트

- `font-size` : 기본값 `medium`(`16px`)

- `line-height` : 라인 박스의 높이

<br>

### 1) `font-style`

- `normal` : 기본값

- `italic`

- `oblique`

- `oblique <angle>`

<br>

### 2) `font-weight`

- `normal` : `400` / 기본값

- `bold` : `700`

- `bolder` : 부모 요소보다 두껍게

- `lighter` : 부모 요소보다 얇게

- `100`-`900` : 사이의 숫자로 지정

<br>

위의 값들 중 하나로 폰트의 두께가 지정되면, 브라우저는 해당 두께를 가진 폰트를 찾아서 적용합니다. 이 말을 풀어보면, 하나의 글꼴(`font-family`)을 여러 두께로 표현한 폰트 리소스들이 어딘가에 존재하고, 여러 리소스 중 두께가 일치하는 폰트를 가져와서 적용한다는 의미입니다. 따라서 `font-family` 값을 숫자로 지정하더라도, 정확하게 두께가 일치하는 폰트가 없으면 접근 가능한 리소스 중에서 가장 비슷한 두께의 폰트를 찾게 됩니다.

<br>

![font-weight](./../img/font-weight.jpeg)

<br>

#### 가변 폰트(Variable fonts)

대부분의 폰트는 `100`-`900` 사이에 해당하는 고유한 두께를 가집니다. 이는 폰트 이름에 반영되어 있는데요, `100`-`900` 숫자들은 일반적으로 폰트 이름에서 아래와 같이 표현됩니다. 예를 들어, `Arial Black` 폰트는 두께 값이 `900`이라고 볼 수 있습니다.

| Weight | Font Name Suffix              |
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

하지만 모든 폰트가 고유한 두께를 가지는 것은 아닙니다. 직접 두께를 지정할 수 있는 폰트를 Variable fonts라고 합니다.

<br>

### `bolder`/`lighter`

`100`/`400`/`700`/`900` 중 현재 폰트의 두께보다 더 두껍거나 얇은 값이 적용됩니다.

<br>

## 단축 속성

기본 속성들을 한 번에 지정할 수 있는 단축 속성입니다.

- `font`

<br>

값은 아래와 같이 작성하되, `font-size`, `font-family` 값은 필수로 포함하고 다른 값들은 생략할 수 있습니다. 또한, `line-height` 값은 `/`를 사용하여 `font-size` 값과 명확하게 구분하는 것이 문법입니다.

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

<br>

---

### Reference

- [font-style | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)
- [font-weight | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-weight)
