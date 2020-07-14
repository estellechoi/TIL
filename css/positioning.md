# 포지셔닝 규칙(Positioning schemes)

<br>

## 포지셔닝 규칙(Positioning schemes)

HTML 요소들의 레이아웃은 아래의 3 가지 포지셔닝 규칙(Positioning schemes)에 따라 결정됩니다.

- Normal flow

- Floats

- Absolute positioning

<br>

> 기본적으로 Normal flow를 따르지만, `float`/`position` 속성 값에 따라 Floats 모델과 Absolute 포지셔닝을 따릅니다.

<br>

## Normal flow

Normal flow는 박스들이 포지셔닝 되는 가장 기본적인 방식입니다. Floats 모델을 따르거나 Absolute 포지셔닝을 따르지 않는 모든 요소들은 Normal flow에 따라 레이아웃 되는데요, 이런 요소들은 자신이 블록(Block)인지 인라인(Inline) 요소인지에 따라 각각 블록/인라인 서식 컨텍스트(Formatting Context)에 속하게 됩니다.

- 1. 블록 서식 컨텍스트(Block Formatting Context)

- 2. 인라인 서식 컨텍스트(Inline Formatting Context)

- 3. 상대적 포지셔닝(Relative Positioning)

<br>

> 서식 컨텍스트(Formatting Context)란 HTML 요소들이 시각적으로 레이아웃/포지셔닝 되는 흐름, 환경을 나타내는 추상적인 개념입니다.

<br>

### 1) 블록 서식 컨텍스트(Block Formatting Context)

> [설명 보기](https://github.com/estellechoi/TIL/blob/master/css/bfc.md)

<br>

### 2) 인라인 서식 컨텍스트(Inline Formatting Context)

> [설명 보기](https://github.com/estellechoi/TIL/blob/master/css/ifc.md)

<br>

### 3) Relative 포지셔닝

Normal flow를 따르거나 Float 요소인 박스는 Relative 포지셔닝이 가능합니다. Relative 포지셔닝을 위해서는 `position` 속성 값을 `relative`로 지정합니다. Relative 포지셔닝에 따라 배치된 박스는 다음 박스의 위치에 영향을 주지 않습니다.

<br>

## Floats

`float` 모델을 따르는 박스들은 먼저 Normal flow에 따라 레이아웃되고, 그 후 Normal flow에서 빠져나와 왼쪽 혹은 오른쪽으로 옮겨집니다. 다른 요소들은 이 `float` 요소들의 사이사이에 흐르듯 레이아웃 됩니다.

<br>

## Absolute 포지셔닝

`absolute` 포지셔닝을 따르는 박스는 애초에 Normal flow로부터 분리되어 완전히 독립적으로 레이아웃 됩니다. 따라서 형제 박스들의 레이아웃에 전혀 영향을 주지 않게 됩니다. `absolute` 박스는 부모 요소의 위치를 기준으로 지정된 위치에 레이아웃 됩니다.

> 여기에서 부모 요소는 `position` 속성을 가진 가장 가까운 상위 요소를 말합니다.

<br>

### `position`

> 자세한 내용은 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/CSS/position)를 참고하세요.

<br>

`position` 속성의 값은 다음 중 하나입니다. (`position` 속성을 지정하지 않은 경우) 기본값은 `static`입니다.

- `static`
- `relative`
- `absolute`
- `fixed`
- `sticky`
- `inherit`

<br>

`position` 속성 값에 의해 해당 요소의 포지셔닝이 결정됩니다. 다음은 포지셔닝의 종류입니다.

- Positioned : `position` 속성 값이 `static`이 아닌 모든 요소

  > Positioned 요소는 4 개의 속성 값 `top`, `right`, `bottom`, `left`에 따라 포지셔닝 됩니다.

- Relatively positioned : `position` 속성 값이 `relative`인 요소

- Absolutely positioned : `position` 속성 값이 `absolute`/`fixed`인 요소

  > 해당 요소가 마진을 가지면, 마진 값은 they 박스 오프셋(Box offsets)에 포함됩니다. 또한, 이러한 요소들은 자신을 위한 새로운 블록 서식 컨텍스트를 생성합니다.

- Stickily positioned : `position` 속성 값이 `sticky`인 요소

<br>

Positioned 요소가 아니거나, Relatively positioned 요소이면 Normal flow에 따라 배치됩니다. 간단하게, `position` 속성 값이 `static`/`relative`이면 Normal flow를 따릅니다.

<br>

### 박스 오프셋(Box offsets)

`top`, `right`, `bottom`, `left` 이 4 개의 속성들은 박스와 박스가 겹쳐서 포지셔닝 될 때 박스의 위치를 나타내기 때문에 박스 오프셋(Box offsets) 속성이라고 부릅니다.

가령 `absolute` 박스의 `top` 속성 값이 `10px` 이라면, 이 박스의 `top margin`은 부모 박스의 가장 위에서부터 `10px` 만큼 오프셋 됩니다. 한편 `relative` 박스는 Normal flow에 따라 자신이 원래 위치해야 할 가상의 좌표를 가질 것입니다. 그 위치를 기준으로 가장 위에서 부터 `10px` 만큼 오프셋 됩니다.

<br>

---

### References

- [Positioning schemes | W3C Recommendation](https://www.w3.org/TR/CSS2/visuren.html#positioning-scheme)
- [Visual formatting model | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Visual_formatting_model)
- [CSS Positioned Layout | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning)
- [position | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
