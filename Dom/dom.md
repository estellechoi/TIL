# DOM ( Document Object Model)

> This markdown page is Korean/English mixed entirely depending on my personal need, not readers' possible expectation. Just the reorderd, added, and removed from reference documents, or some parts are the English-Korean translated version.

## What is the DOM?

- The Document Object Model(DOM) represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects. That way, programming languages can connect to the page.

- The DOM is an object-oriented representation of the web page, which can be modified with a scripting language such as JavaScript.

- The DOM connects web pages to scripts or programming languages by representing the structure of a document—such as the HTML representing a web page—in memory. Usually, that means JavaScript, although modeling HTML, SVG, or XML documents as objects are not part of the core JavaScript language, as such.

- The DOM represents a document with a logical tree. Each branch of the tree ends in a node, and each node contains objects. DOM methods allow programmatic access to the tree. With them, you can change the document's structure, style, or content.

  > Nodes can also have event handlers attached to them. Once an event is triggered, the event handlers get executed.

- The modern DOM is built using multiple APIs that work together. For example, the [HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API) adds support for representing HTML documents to the core DOM. The core DOM defines the objects that fundamentally describe a document and the objects within it.

<br>

## DOM and JavaScript

- JavaScript uses the DOM to access the document and its elements. The DOM is not a programming language, but without it, the JavaScript language wouldn't have any model or notion of web pages, HTML documents, XML documents, and their component parts (e.g. elements).

- The page content is stored in the DOM and may be accessed and manipulated via JavaScript. Every element in a document can all be accessed and manipulated using the DOM and a scripting language like JavaScript.

- The DOM was designed to be independent of any particular programming language, making the structural representation of the document available from a single, consistent API. Implementations of the DOM can be built for any language.

<br>

## Accessing the DOM

- you can immediately begin using the API for the [`document`](https://developer.mozilla.org/en-US/docs/Web/API/Document) or [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window) elements to manipulate the document itself or to get at the children of that document, which are the various elements in the web page.

- A global variable, `window`, representing the window in which the script is running, is exposed to JavaScript code. In a tabbed browser, each tab is represented by its own `Window` object.

  > Even in a tabbed browser, some properties and methods still apply to the overall window that contains the tab, such as `resizeTo()` and `innerHeight`. Generally, anything that can't reasonably pertain to a tab pertains to the window instead.

- The `Document` interface represents any web page loaded in the browser and serves as an entry point into the web page's content, which is the [DOM tree](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Using_the_W3C_DOM_Level_1_Core). The DOM tree includes elements such as `<body>` and `<table>`, among many others.

<br>

## Fundamental data types

> Note: Because the vast majority of code that uses the DOM revolves around manipulating HTML documents, it's common to refer to the nodes in the DOM as elements, although strictly speaking not every node is an element.

| Data type (Interface) | Description                                                                                                                                                                                                                                                                                                       |
| :-------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       Document        | This object is the root `document` object itself.                                                                                                                                                                                                                                                                 |
|         Node          | Every object located within a document is a node of some kind. In an HTML document, an object can be an `element` node but also a text node or attribute node.                                                                                                                                                    |
|        Element        | It refers to an element or a node of type `element`.테스트2                                                                                                                                                                                                                                                       |
|       NodeList        | An array of elements, like the kind that is returned by the method document.`getElementsByTagName()`. Items in a nodeList are accessed by index in either of two ways: `list.item(0)`, `list[0]`. In the first, `item()` is the single method on the `nodeList` object. The latter uses the typical array syntax. |
|       Attribute       | It is an object reference that exposes a special (albeit small) interface for attributes. Attributes are nodes in the DOM just like elements are, though you may rarely use them as such.                                                                                                                         |
|     NamedNodeMap      | It is like an array, but the items are accessed by name or index. A namedNodeMap has an `item()` method.                                                                                                                                                                                                          |

<br>

## DOM interfaces

- This guide is about the objects and the actual things you can use to manipulate the DOM hierarchy.

### Interfaces and Objects

- DOM 객체들은 몇가지 서로 다른 인터페이스에서 비롯된다. 예를 들어, `table` 객체는 테이블에 특화된 [`HTMLTableElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableElement) 인터페이스를 사용한다. 그렇기 때문에 `createCaption`, `insertRow` 등과 같은 테이블 조작에 사용할 법한 메소드를 제공한다. 한편, `table`은 HTML 요소 중 하나이기도 하다. 따라서 일반적인 [`Element`](https://developer.mozilla.org/en-US/docs/Web/API/Element) 인터페이스를 함께 사용할 수 있다. 또 한편 DOM의 관점에서는 HTML이나 XML과 같은 문서의 Node 트리를 구성하는 하나의 노드이다. 따라서 `Node` 인터페이스 역시 사용할 수 있다. 사실 `Element` 인터페이스도 `Node` 인터페이스에서 파생되었다.

```javascript
const table = document.getElementById("table");
const tableAttrs = table.attributes; // attributes ->  Node/Element interface

// border attribute -> HTMLTableElement interface
for (let i = 0; i < tableAttrs.length; i++) {
	if (tableAttrs[i].nodeName.toLowerCase() == "border") table.border = "1";
}

// summary attribute -> HTMLTableElement interface
table.summary = "note: increased border";
```

<br>

### Core Interfaces in the DOM

- The `document` and `window` objects are the objects whose interfaces you generally use most often in DOM programming. In simple terms, the `window` object represents something like the browser, and the `document` object is the root of the document itself. `Element` inherits from the generic `Node` interface, and together these two interfaces provide many of the methods and properties you use on individual elements.

- The following is a brief list of common APIs in web and XML page scripting using the DOM.

  - `document.getElementById(id)`
  - `document.getElementsByTagName(name)`
  - `document.createElement(name)`
  - `parentNode.appendChild(node)`
  - `element.innerHTML`
  - `element.style.left`
  - `element.setAttribute()`
  - `element.getAttribute()`
  - `element.addEventListener()`
  - `window.content`
  - `window.onload`
  - `window.scrollTo()`

<br>

## What is a content tree?

> The W3C's DOM Level 1 Core is a powerful object model for changing the content tree of documents. It is supported in all major browsers including Mozilla Firefox and Microsoft Internet Explorer. It is a powerful base for scripting on the web.

- Any HTML document (or for that matter any SGML document or XML document) is a tree structure.
- When Mozilla parses a document, it builds a content tree and then uses it to display the document.
- For example,

```html
<html>
	<head>
		<title>My Document</title>
	</head>
	<body>
		<h1>Header</h1>
		<p>Paragraph</p>
	</body>
</html>
```

![Content Tree](./../img/contentTree.jpg)

Each of the boxes in the tree above is a node. The line above a node expresses a parent-child relationship: the node on top is the parent, and the node on the bottom is the child. Two children of the same parent are therefore siblings. Similarly, one can refer to ancestors and descendants. (Cousins are too messy, though.)

<br>

---

### Reference

- [DOM 소개 | MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/%EC%86%8C%EA%B0%9C)
- [Document Object Model (DOM) | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [WHATWG DOM](https://dom.spec.whatwg.org/)
- [Using the W3C DOM Level 1 Core | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Using_the_W3C_DOM_Level_1_Core)
- [The HTML DOM API | MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)
- [JavaScript technologies overview | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/JavaScript_technologies_overview)
