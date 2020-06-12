# Object Model: DOM and BOM)

## What is Object Model ?

- 웹브라우저의 모든 구성 요소들은 객체화되어 있다.
- JavaScript와 같은 스크립트 언어를 이용해서 이 객체들에 접근, 조작할 수 있다.
- 객체들은 계층적인 관계이며, 트리 구조이다.
- DOM과 BOM은 각각 `window` 객체의 속성 중 하나이다.

![DOM/BOM](./../img/domBom.png =100x)

### What is `window` object?

- A global root obejct.
- Means Browser itself. For example, `window.close();` closes the active browser window.

<br/>

## DOM, Document Object Model

- 문서에 대한 모든 내용을 담고있는 객체
- 문서 즉 열려진 페이지에 대한 정보를 따로 객체화 시켜서 관리
- `document` 객체

<br/>

## BOM, Browser Object Model

- 브라우저에 대한 모든 내용을 담고있는 객체
- 뒤로가기, 북마크, 즐겨찾기, 히스토리, URL정보 등
- 브라우저가 가지고 있는 정보를 따로 객체화 시켜서 관리
- `history`, `location` 객체

<br/>

---

### Reference

- [JavaScript Tutorial](https://www.w3schools.com/js/)
