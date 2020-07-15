# 블록(Block) / 인라인(Inline) 요소

> 블록(Block) vs. 인라인(Inline) 요소 개념은 HTML 4.01 이하 버전에서 계속 사용되었습니다. HTML 5 부터는 조금 더 복잡한 [Content categories](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories) 개념으로 대체되었습니다. 하지만 블록/인라인 개념은 여전히 페이지 레이아웃을 이해하는데 유효한 중요한 개념입니다.

<br>

## 블록(Block) 요소

- `display: block`

- 부모 요소의 너비 전체를 차지합니다. 반면, `height` 값은 필요한 만큼만 사용합니다. 따라서 `width: auto; height: auto;`의 실제 값은 `width: 100%; height: 0;`을 기준으로 하되, 외부 요소에 영향을 받으며 정해집니다.

  > 관련 MDN 원문 : A block-level element always starts on a new line and takes up the full width available (stretches out to the left and right as far as it can).

- 예) `<div>`, `<h1>`, `<p>`, `<section>`, `<article>`, `<nav>` 태그

<br>

> [블록 태그 모두 보기](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements#Elements)

<br>

## 인라인(Inline) 요소

- `display: inline`

- `width`/`height` 값을 지정할 수 없습니다. 필요한 만큼의 너비/높이만 자동으로 (최소한) 사용합니다.

  > 관련 MDN 원문 : By default, block-level elements begin on new lines, but inline elements can start anywhere in a line.

- `margin`/`padding` 속성의 위/아래(`top`/`bottom`) 값을 지정할 수 없습니다. 무조건 `0` 입니다.

  > 주의 : `padding` 속성의 경우 `bottom` 값을 지정하고 시각적으로 구현할 수도 있습니다. 하지만 외부의 다른 요소와의 거리를 구현하기 위해 사용하지 마십시오. 요소간의 거리를 형성하는 것은 `margin` 속성의 역할일 뿐더러, 인라인 요소의 `padding-bottom` 값은 아래에 위치한 요소와의 거리를 형성하지 못해 요소가 겹치는 문제가 발생합니다.

- 예) `<span>`, `<img>`, `<a>` 태그

<br>

> [인라인 태그 모두 보기](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements#Elements)

<br>

---

### References

- [display | MDN](https://developer.mozilla.org/ko/docs/Web/CSS/display)
- [Block-level elements | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements)
- [Inline elements | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements)
- [Content categories | MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories)
- [Block and inline layout in normal flow | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flow_Layout/Block_and_Inline_Layout_in_Normal_Flow)
