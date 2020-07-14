# 박스 모델(Box Model)

<br>

## 박스 트리(Box Tree)

웹 브라우저는 HTML 문서와 CSS 파일의 내용을 해석하여 캔버스에 그림을 그리듯 문서를 렌더링 합니다. 이 작업을 하기 위해서는 렌더링된 문서와 서식 구조를 나타내는 임시 구조물 같은 것을 형성해야 하는데, 이를 박스 트리(Box Tree)라고 합니다.

HTML 마크업에 따라 모든 요소들은 자신을 참조하는 박스(Box)를 생성하는데요, 이때 CSS에서 지정한 `display` 속성 값에 따라 박스의 타입(Type)이 결정됩니다. 또한, 이 속성 값에 따라 박스를 생성하지 않는 경우도 있습니다. HTML 요소들이 담고 있는 글자들은 박스 내부의 텍스트 노드(Text Node)를 형성합니다.

<br>

### 최상위 박스(Principal Box)

하나의 엘리먼트가 여러 개의 박스들을 생성할 수 있는데, 그 중 하나의 박스는 반드시 최상위 박스(Principal Box)가 됩니다. 예를 들어, `display: list-item` 속성 값을 가지는 요소는 최상위 박스와 그 하위의 자식 박스들을 생성합니다. 한편, `none`/`contents` 속성 값을 가지는 요소와 그 하위의 모든 요소들은 최상위 박스를 포함한 박스를 일절 생성하지 않습니다.

<br>

### 익명 박스(Anonymous Box)

익명 박스(Anonymous Box)는 HTML 요소를 기반으로 하지 않는 박스입니다. HTML 요소를 기반으로 하지 않고 어떻게 박스가 생성될 수 있을까요? 자, 아래와 같이 텍스트만 담고 있는 HTML 요소가 있다고 상상해봅시다.

```html
<div class="box">
	Lorem Ipsum is simply dummy text of the printing and typesetting industry.
	Lorem Ipsum has been the industry's standard dummy text ever since the 1500s,
	when an unknown printer took a galley of type and scrambled it to make a type
	specimen book. It has survived not only five centuries, but also the leap into
	electronic typesetting, remaining essentially unchanged. It was popularised in
	the 1960s with the release of Letraset sheets containing Lorem Ipsum passages,
	and more recently with desktop publishing software like Aldus PageMaker
	including versions of Lorem Ipsum.
</div>
```

<br>

이 요소를 기반으로 하는 Flex 박스를 생성하려면 아래와 같은 CSS가 필요합니다.

```css
.box {
	display: flex;
}
```

원래 이 박스의 내부에는 텍스트 밖에 없기 때문에 하위 박스가 존재하지 않아야 합니다. 하지만, 이 박스는 Flex 박스이기 때문에 내부의 텍스트 하나 하나를 담고 있는 익명 박스들이 생겨납니다. 그리고 이 익명 박스들이 Flex 아이템의 역할을 하면서 Flex 박스의 정렬 규칙에 따라 내부 텍스트들을 정렬할 수 있게 됩니다.

<br>

> Flex Box에 대한 자세한 내용은 [이 블로그 글](https://heropy.blog/2018/11/24/css-flexible-box/)에서 매우 잘 설명하고 있습니다.

<br>

참고로, 익명 박스는 일반적인 박스와 달리 참조할 수 있는 HTML 요소가 없기 때문에 독립적으로 스타일을 지정할 수 없습니다. 익명 박스는 자신이 속한 부모 박스의 스타일을 상속받게 됩니다.

<br>

### 라인 박스(Line Box)

라인 박스(Line Box)는 한 줄의 텍스트들을 감싸는 박스입니다.

> 라인 박스에 대한 자세한 설명은 [이 글](https://github.com/estellechoi/TIL/blob/master/css/formattingContext.md)의 Inline Formatting Context 부분에 있습니다.

<br>

---

### References

- [Visual Formatting Model | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Visual_formatting_model)
