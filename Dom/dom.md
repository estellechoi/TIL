# DOM ( Document Object Model)

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

---

### Reference

- [DOM 소개 | MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/%EC%86%8C%EA%B0%9C)
- [Document Object Model (DOM) | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [WHATWG DOM](https://dom.spec.whatwg.org/)
- [Using the W3C DOM Level 1 Core | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Using_the_W3C_DOM_Level_1_Core)
- [The HTML DOM API | MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)
- [JavaScript technologies overview | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/JavaScript_technologies_overview)
