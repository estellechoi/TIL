# Visual Formatting Model

<br>

## Visual Formatting Model

Visual Formatting Model은 브라우저에서 어떻게 DOM 트리를 렌더링하여 시각 매체를 통해 보여주는지 설명합니다. Visual Formatting Model에서 DOM 트리의 각 요소는 박스 모델(Box Model)에 따라 박스들을 생성하며, 박스들의 레이아웃은 아래의 항목들에 의해 결정됩니다.

- 박스 타입(Box Type)과 높이/너비 등의 값

- 포지셔닝 규칙(Positioning Schemes)

  - Normal flow
  - Floats
  - Absolute positioning

- 문서 트리 내에서 요소들의 관계

- 외부 정보 (e.g., viewport size, intrinsic dimensions of images, etc.)

<br>

> 시각 매체는 컴퓨터 스크린(Continuous Media), 브라우저 인쇄 기능을 이용해 인쇄된 종이 문서(Paged Media)를 포함합니다.

<br>

### 박스 모델(Box Model)

> [설명 보기](https://github.com/estellechoi/TIL/blob/master/css/box.md)

<br>

### 포지셔닝 규칙(Positioning schemes)

> [설명 보기](https://github.com/estellechoi/TIL/blob/master/css/positioning.md)

<br>

## 뷰포트(Viewport)의 역할

### 뷰포트(Viewport)란?

뷰포트(Viewport)는 브라우저 창에서 내용이 보여지는 영역을 말합니다. 가령, 브라우저 창의 크기를 줄이거나, 모바일 기기의 방향을 바꾸면 뷰포트의 가로/세로 사이즈가 변경됩니다. 이렇게 변경되는 뷰포트의 사이즈는 페이지의 레이아웃에 영향을 줍니다.

### 역할

뷰포트의 크기가 HTML 문서의 크기보다 작더라도 브라우저는 스크롤(Scroll)과 같이 문서의 나머지 부분을 탐색할 수 있는 방법을 제공합니다. 보통은 위에서 아래로 스크롤 하게 되는데, 이는 블록(Block) 요소들이 배치되는 방향입니다. (Block dimension) 반면, 왼쪽에서 오른쪽, 오른쪽에서 왼쪽으로 스크롤이 되도록 디자인할 수 있는데, 인라인(Inline) 요소들이 배치되는 방향입니다. (Inline dimension)

<br>

---

### References

- [Visual formatting model | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Visual_formatting_model)
- [display | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display)
