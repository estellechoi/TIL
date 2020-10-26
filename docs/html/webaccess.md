# How to stay with web-accessiblity highly achieved

> 웹 접근성(Web Accessibility) 준수를 위한 팁들을 태그(Tag) 별로 나열하는 문서입니다.

<br>

## Labeling section content

Another common navigation technique for users of screen reading software is to generate a list of sectioning content and use it to determine the page's layout.

Sectioning content can be labeled using a combination of the aria-labelledby and id attributes, with the label concisely describing the purpose of the section. This technique is useful for situations where there is more than one sectioning element on the same page.

```html
<header>
	<nav aria-labelledby="primary-navigation">
		<h2 id="primary-navigation">Primary navigation</h2>
		<!-- navigation items -->
	</nav>
</header>

<!-- page content -->

<footer>
	<nav aria-labelledby="footer-navigation">
		<h2 id="footer-navigation">Footer navigation</h2>
		<!-- navigation items -->
	</nav>
</footer>
```

<br>

In this example, screen reading technology would announce that there are two <nav> sections, one called "Primary navigation" and one called "Footer navigation". If labels were not provided, the person using screen reading software may have to investigate each nav element's contents to determine their purpose.

<br>

---

### References

- [\<h1\>–\<h6\>: The HTML Section Heading elements | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)
- [HTML elements reference - Content sectioning | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Content_sectioning)
