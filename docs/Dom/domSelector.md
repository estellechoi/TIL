# Locating DOM elements using selectors

> This markdown page is Korean/English mixed entirely depending on my personal need, not readers' possible expectation. Just the reorderd, added, and removed from reference documents, or some parts are the English-Korean translated version. But, maybe helpful for some people.

The Selectors API provides methods that make it quick and easy to retrieve `Element` nodes from the DOM by matching against a set of selectors.

<br>

## The NodeSelector interface

- This specification adds two new methods to any objects implementing the [`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document), [`DocumentFragment`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment), or [`Element`](https://developer.mozilla.org/en-US/docs/Web/API/Element) interfaces:

  - `querySelector()` : returns the first matching `Element` node within the node's subtree. If no matching node is found, `null` is returned.

  - `querySelectorAll()` : returns a `NodeList` containing all matching `Element` nodes within the node's subtree, or an empty `NodeList` if no matches are found.

  > Note: The NodeList returned by `querySelectorAll()` is not live, which means that changes in the DOM are not reflected in the collection. This is different from other DOM querying methods that return live node lists.

<br>

### What is DocumentFragment?

- `DocumentFragment` 인터페이스는 하나의 문서를 이루고있는 `document` 객체의 노드 트리에서 특정 부분을 떼내기 위해 사용된다. 여기에 중요한 점이 있다. 떼내어진 조각 객체는 `document` 객체로부터 생겨났지만 부모를 가지지 않는다. 또한 문서의 한 부분으로 생각해서는 안된다. 조각 객체에 변경 사항이 생기더라도 문서에 영향을 주지 않고 독립적으로 존재하기 때문이다.

- A common use for `DocumentFragment` is to create one, assemble a DOM subtree within it, then append or insert the fragment into the DOM using `Node` interface methods such as `appendChild()` or `insertBefore()`.

- An empty `DocumentFragment` can be created using the `document.createDocumentFragment()` method or the constructor.

- For example,

```html
<ul id="listContainer"></ul>
```

```javascript
var container = document.querySelector("listContainer");
var array = ["Barcelona", "Seoul", "Porto", "Paris"];

var frag = new DocumentFragment();

array.forEach((item) => {
	var li = document.createElement("li");
	li.innerHTML = item;
	frag.appendChild(li);
});

container.appendChild(frag);
```

<br>

## Selectors

- The selector methods accept one or more comma-separated selectors
- You may use any CSS selectors with the `querySelector()` and `querySelectorAll()` methods.

```javascript
const els = document.querySelectorAll("p.warning, p.note");

// returns the first element whose ID is one of main, basic, or exclamation.
const els2 = document.querySelector("#main, #basic, #exclamation");
```

<br>

## `Element.querySelector()` and `Document.querySelector()`

- The `querySelector()` method of the `Element` interface returns the first element that is a descendant of the element on which it is invoked that matches the specified group of selectors.

- The `Document` method `querySelector()` returns the first `Element` within the document that matches the specified selector, or group of selectors.

<br>

---

### Reference

- [Locating DOM elements using selectors | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements_using_selectors)
- [DocumentFragment | MDN](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)
- [Document.querySelector() | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [Element.querySelector() | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector)
