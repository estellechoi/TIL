# Image Sprites

<br>

## Image Sprites

이미지 스프라이트(Image Sprite)는 여러 개의 이미지를 묶어 놓은 하나의 이미지를 말합니다. 웹 페이지에서 여러 이미지를 일일이 요청하지 않고, 하나의 이미지 파일로 묶어 한 번에 받아서 사용하는 방법입니다. 아이콘이나 버튼과 같은 독립된 여러 이미지들을 하나의 이미지로 합쳐놓고 CSS의 `background-position` 속성 값을 바꾸어 이미지를 하나씩 보여줍니다. 이를 통해 얻는 혜택은 아래와 같습니다.

- HTTP 요청 횟수를 줄여 로딩 속도를 높이고,

- 트래픽을 절약하고,

- 내려받는 이미지의 전체 크기까지 줄일 수 있으며,

- 많은 이미지 파일들을 관리하는 대신 몇 개의 Sprite 이미지들만 관리하면 되기 때문에 간편합니다.

<br>

아래와 같은 이미지가 Sprite 이미지 입니다.

<br>

![sprite1](./../img/sprite1.gif)

<br>

## Image Sprites 구현하기

Image Sprites 기법을 사용해봅시다. 이 기법은 Sprite 이미지를 배경 이미지로 사용하여 구현합니다. 먼저, 아래와 같은 마크업이 필요합니다.

```html
<img class="home" src="img_trans.gif" />
```

<br>

- `<img>` 태그의 `src` 속성 값은 비어있을 수 없으므로, 투명한 의미없는 이미지를 넣어주세요.

<br>

그 다음, `home` 클래스에 대한 CSS 코드를 아래와 같이 작성합니다.

```css
.home {
	width: 46px;
	height: 44px;
	background: url(img_navsprites.gif) 0 0;
}
```

> [MDN web docs의 `background` 속성 문법](https://developer.mozilla.org/ko/docs/Web/CSS/background)을 참고하세요. `background` 속성은 단축 표기를 위한 속성입니다.

<br>

- `width: 46px; height: 44px;` 실제 보여줄 이미지의 크기를 지정합니다.

- `url(img_navsprites.gif)` 사용할 이미지를 지정합니다.

- `0 0` 배경 이미지를 보여줄 위치를 지정합니다. `left` - `top` 순서입니다.
  > [MDN web docs의 `background-position` 속성 문법](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)을 참고하세요.

<br>

## 예제

```html
<ul class="navlist">
	<li class="home">
		<a></a>
	</li>
	<li class="next">
		<a></a>
	</li>
	<li class="prev">
		<a></a>
	</li>
</ul>
```

```css
.navlist {
	position: relative; /* allow absolute position inside */
}

.navlist li {
	margin: 0;
	padding: 0;
	list-style: none;
	position: absolute; /* all list items are absolute positioned */
	top: 0;
}

.navlist li,
.navlist a {
	height: 44px;
	display: block;
}

.home {
	left: 0px;
	width: 46px;
	background: url("img_navsprites.gif") 0 0;
}

.next {
	left: 129px;
	width: 43px;
	background: url("img_navsprites.gif") -91px 0;
}

.prev {
	left: 63px;
	width: 43px;
	background: url("img_navsprites.gif") -47px 0; /* home width 46px + line 1px */
}
```

<br>

## Hover 효과 사용하기

Sprite 이미지를 사용하면 로딩 딜레이 없이 Hover 효과를 구현할 수 있습니다. 아래의 이미지를 사용해 구현해봅시다.

<br>

![sprite2](./../img/sprite2.gif)

<br>

```css
.home a:hover {
	background: url("img_navsprites_hover.gif") 0 -45px;
}

.next a:hover {
	background: url("img_navsprites_hover.gif") -91px -45px;
}

.prev a:hover {
	background: url("img_navsprites_hover.gif") -47px -45px;
}
```

<br>

---

### References

- [CSS Image Sprites | w3schools](https://www.w3schools.com/css/css_image_sprites.asp)
- [CSS Sprites: Image Slicing’s Kiss of Death](https://alistapart.com/article/sprites/)
- [HTTP 요청 횟수를 줄여줄 수 있는 CSS Sprites 기법](https://appletree.or.kr/blog/web-development/css/http-%EC%9A%94%EC%B2%AD-%ED%9A%9F%EC%88%98%EB%A5%BC-%EC%A4%84%EC%97%AC%EC%A4%84-%EC%88%98-%EC%9E%88%EB%8A%94-css-sprites-%EA%B8%B0%EB%B2%95/)
- [background-position | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)
