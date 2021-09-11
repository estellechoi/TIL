# 개발자를 위한 디자인 시스템 Foundation 3 - 타이포그래피(Typography)

<br>

1. 타입페이스(Typeface) 정하기
2. 여러 개의 타입페이스를 함께 사용하기
3. 줄높이(Line Height): 정의, 웹접근성, 4포인트 그리드 시스템
4. 폰트 사이즈(Font Size): 웹접근성, Type Scales
5. 폰트 사이즈와 줄높이를 그리드 시스템에 맞추기
6. 폰트 굵기(Font Weight)
7. 자간(Letter Spacing)
8. 줄 길이(Paragraph Width)
9. 폰트 스타일(Font Style)

<br>

## 1. 타입페이스(Typeface) 정하기

### 1-1. 타입페이스(Typeface)

타입페이스(Typeface)를 정하는 것으로 타이포그래피(Typography) 구축을 시작합니다. 타입페이스는 흔히 말하는 폰트(서체)를 의미합니다. 보통 1 ~ 2 개의 타입페이스를 선정한 후 크기, 굵기 등을 확장하여 타이포그래피 시스템을 구축하고요, 타입페이스는 브랜드의 톤앤매너와 가독성을 고려하여 선정해야합니다. [Wikipedia List of typefaces](https://en.wikipedia.org/wiki/List_of_typefaces)에서 존재하는 거의 모든 타입페이스 목록을 확인할 수 있습니다.

<br>

다음은 타입페이스 선택을 도와주는 도구들입니다.

- [FontBase](https://fontba.se/)
- [Font Playground](https://play.typedetail.com/)
- [Pair & Compare](https://www.pairandcompare.net/)
- [FontPair](https://www.fontpair.co/)
- [FontSpark](https://fontspark.app/)
- [Better Font Finder](https://jmattthew.github.io/better-font-finder/better-font-finder.html)

<br>

### 1-2. `Serif` vs `Sans Serif`

많고 많은 타입페이스 중 어떤 것을 선택해야할까요? 보통 `Serif`와 `Sans Serif` 중에서 사용할 Primary 폰트 계열을 정하는 것으로 시작합니다. `Serif`(세리프)는 인쇄된 H나 I 같은 활자에서 아래·위에 가로로 나 있는 가는 선을 의미하고요, 따라서 `Serif` 계열 폰트의 특징은 활자 끝에 확장된 선이 있다는 것입니다. `Sans Serif`는 그 반대입니다. 한글에서 `Serif`는 명조체, `Sans Serif`는 고딕체로 부릅니다.

<br>

호흡이 긴 문장을 읽을 때는 `Serif` 계열 폰트가 가독성이 좋습니다. 반면, 활자에 익숙하지 않은 어린이들이나 장애가 있는 사람들에게는 `Sans Serif` 계열 폰트의 가독성이 더 좋습니다. 예로 [Medium](https://medium.com/) 서비스는 전문지식을 다루는 퀄리티 아티클이 많은 플랫폼이기 때문에 `Serif` 계열 폰트를 Primary로 사용하는 것 같네요.

<br>

<img src="./../img/serif-vs-sans-serif.webp" />

사진 출처 : [Logo Design: Serif vs Sans-Serif](https://www.97thfloor.com/blog/serif-vs-sans-serif)

<br>

### 1-3. Web Safe Fonts

모든 주요 브라우저에서 기본으로 제공하는 폰트들을 [CSS Web Safe Fonts](https://www.w3schools.com/cssref/css_websafe_fonts.asp)라고 합니다. 브라우저에 내장되어 있기 때문에 CSS로 선언하는 것만으로 웹에서 바로 사용할 수 있는 폰트들입니다.

<br>

#### Serif 계열

- Georgia
- Lucida
- Times New Roman

<br>

#### Sans Serif 계열

- Arial
- Tahoma
- <span style="font-family: Verdana">Verdana</span>

<br>

### 1-4. Proportional vs Monospaced

폰트를 Proportional/Monospaced 계열로 나눌 수도 있습니다. 특별한 이유가 없다면 모든 글자의 넓이가 동일한 Monospaced 폰트 대신 Proportional 폰트를 사용합니다. 애초에 Monospaced 폰트는 글자 하나 하나를 면밀히 체크할 필요가 있는 코드의 가독성을 높이기 위해 고안된 폰트이기 때문입니다. 반면 Proportional 폰트는 책, 잡지 등 일반적인 인쇄물과 화면에서 컨텐츠의 가독성을 높이기 위해 고안된 폰트이죠.

<br>

다음은 [Proportional Vs. Monospace Fonts](https://www.techwalla.com/articles/proportional-vs-monospace-fonts)에서 발췌한 내용입니다.

> Benefits and Disadvantages of Monospace Fonts Setting text in a monospace font makes it easier to identify characters by themselves. Because of this, tasks that rely on the easy identification of specific characters, such as programming, benefit from the use of a monospace font. Similarly, a monospace font may be used to format code examples within a page that is otherwise set in a proportional font, in order to make them stand out more easily. Text set in a monospace font is also easier to align, leading to the creation of images constructed using characters, known as "ASCII art." <br/>
> On the other hand, because of the fixed width of all the characters, a block of text set in a monospace font will typically take up more space than the same text set in a proportional font. Additionally, long stretches of monospaced text can blend together visually and, as a result, become harder to read.

<br>

### 1-5. 폰트 패밀리(Font Family)

사실 타입페이스를 선택한다는 것은 "단 하나의 고정된 폰트"가 아닌 "폰트 패밀리(Font Family)"를 선택하는 것입니다. 아래 사진처럼 하나의 폰트는 [`100` ~ `900` 사이의 굵기](https://helpx.adobe.com/fonts/using/css-selectors.html#Usingmultipleweightsandstyles) 구성을 가지고요, `italic`체를 지원하기도 합니다. CSS에서 타입페이스를 지정하는 속성의 이름이 `font-family`인 이유이기도 합니다. 폰트 패밀리마다 굵기의 종류와 `italic`체 지원 여부가 다르기 때문에 프로젝트의 성격과 규모를 구현하는데 충분한 패밀리 구성을 갖추었는지 고려하여 타입페이스를 정해야합니다.

<br>

<img src="./../img/font-family.png" />

<br>

## 2. 여러 개의 타입페이스를 함께 사용하기

다음은 여러 개의 타입페이스를 사용할 때 참고할만한 사항들입니다.

- 최대 2 ~ 3 개의 타입페이스만 사용합니다. 그 이상은 주의력을 분산시키기 때문입니다. 폰트의 사이즈와 굵기를 다르게 하는 것으로 다양성은 충분합니다.

- 서로 명확히 대조되는 타입페이스를 사용합니다. 보통 `Serif` 계열 1 가지와 `Sans Serif` 계열 1 가지를 사용합니다.

- 타입페이스간 위계를 명확히 합니다. Primary와 Secondary로 나누어 사용합니다.

<br>

## 3. 줄높이(Line Height): 정의, 웹접근성과 가독성, 4포인트 그리드 시스템

### 3-1. 정의

텍스트의 위아래 행간(Leading)을 포함한 높이를 줄높이(Line Height)라고 합니다. 줄높이(Line Height)는 [웹접근성](https://en.m.wikipedia.org/wiki/Web_accessibility)과 [그리드 시스템](https://material.io/design/layout/responsive-layout-grid.html#columns-gutters-and-margins)을 고려하여 정합니다.

<img src="./../img/line-height.png" />

사진 출처 : [sirong.tistory.com](https://sirong.tistory.com/64)

<br>

### 3-2. 웹접근성과 가독성

[WCAG Success Criterion 1.4.8](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-visual-presentation.html)에 명시된 가이드에 따르면, 폰트 크기의 최소 `1.5` 배를 텍스트의 줄높이 공간으로 사용하는 것이 좋습니다. 만약 본문 텍스트의 폰트 사이즈가 `16px`이라면, 줄높이는 최소 `24px`이어야 웹접근성이 좋다고 보는 것입니다.

<br>

> Line spacing (leading) is at least space-and-a-half within paragraphs

<br>

하지만 일반적으로는, 폰트 사이즈의 `1.3` ~ `1.5` 배의 줄높이가 가독성에 최적이고, `2` 배까지도 허용된다는 것이 중론입니다. 주의할 점은 폰트마다 최적의 줄높이가 다르다는 것입니다. [Golden Ratio Typography Calculator](https://grtcalculator.com/)는 폰트별로 최적의 줄높이를 계산해줍니다.

<br>

폰트 사이즈가 작을 수록 줄높이 배수는 커져야합니다. [Best UX practices for line spacing](https://www.justinmind.com/blog/best-ux-practices-for-line-spacing/)에 따르면, 작은 글씨는 이미 읽기 어려우며, 눈이 쉽게 따라갈 수 있도록 더 많은 공간이 필요하기 때문입니다.

<br>

> Small fonts need more spacing. Line spacing as a % should actually increase with smaller font sizes. This is because smaller fonts are already more difficult to read, and need more space around them for the eye to easily follow.

<br>

### 3-3. 4포인트 그리드 시스템(4pt Baseline Grid)

그리드 시스템에서 모든 Vertical 여백값은 텍스트의 줄높이와 정비례 관계에서 있어야 조화롭다고 봅니다. 따라서 흔히 사용하는 [4포인트 그리드 시스템(4pt Baseline Grid)]()을 채택한다면, 텍스트의 줄높이는 `4`의 배수를 사용해야합니다. 예를 들어 폰트 사이즈가 `16px`이라면 줄높이는 `24px`로 정하는 것이 좋습니다. 폰트 사이즈의 `1.5` 배이면서 `4`의 배수이기 때문입니다.

<br>

## 4. 폰트 사이즈(Font Size): 웹접근성, Type Scales

### 4-1. 웹접근성

가장 먼저 기본 사이즈를 정합니다. 보통 텍스트가 다량 사용되는 본문 영역에 기본 사이즈를 사용하게 됩니다. 웹에서 폰트의 기본 사이즈는 일반적으로 `16px`을 사용합니다. `16px` 보다 작은 크기를 사용하더라도 웹접근성을 고려하여 너무 작은 크기의 텍스트는 사용하지 말아야하고요, 일반적으로 `10pt`(대략 `13px`)을 최소 크기로 보고 있습니다. 텍스트 웹접근성에 대한 다양한 자료는 [WebAIM](https://webaim.org/resources/designers/#text)에서 확인할 수 있습니다.

<br>

`<input>`, `<textarea>` 등의 폼 입력 요소(Form Elements)는 사용자들이 정확한 값을 입력하고 쉽게 확인할 수 있어야하므로 폰트 사이즈를 최소 `16px`로 지정하는 것이 좋습니다. iOS Safari에서는 해당 요소의 폰트 사이즈가 `16px` 보다 작을 경우 `16px` 크기로 보이도록 화면을 강제로 줌(Zoom) 시킵니다.

<br>

### 4-2. Type Scales

이제 기본 사이즈를 중심으로 특정 배수 `x`씩 증감하는 사이즈 팔레트를 구축합니다. 만약 [Major Second Ratio](https://en.wikipedia.org/wiki/Major_second#Epogdoon)에 기반한 디자인을 구축한다면, 아래와 같이 `1rem`을 기점으로 `1.125` 배씩 스케일링하는 사이즈 팔레트를 구성할 수 있겠습니다. `1rem`은 `<html>` 태그의 폰트 사이즈이고요, `<html>` 태그의 폰트 사이즈를 `px` 단위로 고정시킨 후 다른 모든 폰트 사이즈는 `rem` 단위를 사용합니다. 확장성과 웹접근성 때문입니다.

<br>

[Type Scale](https://type-scale.com/)을 사용했습니다.

<br>

<img src="./../img/type-scales.png" />

<br>

기본 사이즈와 배수 `x`를 모듈화하면 CSS는 아래와 같이 작성할 수 있습니다. (Modular Scale)

```css
:root {
	/* base modules */
	--font-size-base: 16px; /* fallback for old browsers */
	--font-size-base: 1rem;
	--type-scale-ratio: 1.125;

	/* type scales */
	--font-size-2xl: calc(
		var(--font-size-base) * var(--type-scale-ratio) * var(--type-scale-ratio) *
			var(--type-scale-ratio)
	);
	--font-size-xl: calc(
		var(--font-size-base) * var(--type-scale-ratio) * var(--type-scale-ratio)
	);
	--font-size-l: calc(var(--font-size-base) * var(--type-scale-ratio));
	--font-size-m: var(--font-size-base);
	--font-size-s: calc(var(--font-size-base) / var(--type-scale-ratio));
	--font-size-xs: calc(
		var(--font-size-base) / (var(--type-scale-ratio) * var(--type-scale-ratio))
	);
}
```

<br>

배수 `x`의 값은 정하기 나름이지만, [Type Scale](https://type-scale.com/)은 아래와 같이 가이드를 제공합니다. 일반적으로 `1.15` ~ `1.333` 배수를 사용하면 충분하고요, 모바일 버전은 `1.2`보다 작은 배수를 사용하는 것이 좋겠네요.

<br>

> Small scales (less than 1.2) are subtle and good for both mobile and desktop apps, or the mobile version of a responsive site. <br/>
> Medium scales (1.15–1.333) have a clear hierarchy, and help to organize sections with subheadings. A medium scale is versatile and works well for a wide variety of desktop sites, including blogs and marketing sites.<br/>
> Large scales (1.333 or greater) may be challenging to implement effectively, but could work well for portfolios, agencies, some marketing sites, or avant-garde works.

<br>

다음은 사이즈 팔레트 구축을 빠르게 도와주는 도구들입니다.

- [Modular Scale](https://www.modularscale.com/) : 자동으로 Modular Scale 팔레트를 구성해주는 사이트
- [Type Scale](https://type-scale.com/) : 사용할 디자인 레이아웃 방법론을 선택하면 자동으로 사이즈 팔레트를 구성해주는 사이트
- [archetype](https://archetypeapp.com/#) : 2 개 이상의 타입페이스를 사용한다면 유용한 사이트

<br>

## 5. 폰트 사이즈와 줄높이를 그리드에 맞추기

4포인트 그리드 시스템을 사용한다면 `x` 배씩 증감하는 사이즈 팔레트의 값들을 인위적으로 조정하여 `4`의 배수 위주로 구성할 수도 있습니다.

```css
:root {
	/* fallback for old browsers */
	--font-size-2xl: 24px;
	--font-size-xl: 20px;
	--font-size-l: 18px;
	--font-size-m: 16px;
	--font-size-s: 14px;
	--font-size-xs: 12px;
	/* rem */
	--font-size-2xl: 1.5rem;
	--font-size-xl: 1.25rem;
	--font-size-l: 1.125rem;
	--font-size-m: 1rem;
	--font-size-s: 0.875rem;
	--font-size-xs: 0.75rem;
}
```

<br>

[CSS 전처리기](https://developer.mozilla.org/ko/docs/Glossary/CSS_preprocessor) 중 하나인 [SCSS](https://sass-lang.com/)의 `math.floor` 함수를 사용하면, 아래와 같이 `4`의 배수로 구성된 Modular Scale을 구축할 수 있습니다.

```scss
@use "sass:list";

/* base modules */
$font-size-base: 1rem;
$type-scale-ratio: 1.125;
$grid-base-pt: 4;

/* type scales */
$font-size-2xl: math.floor(
		$font-size-base * $type-scale-ratio * $type-scale-ratio *
			$type-scale-ratio / $grid-base-pt
	) * $grid-base-pt;
$font-size-xl: math.floor(
		$font-size-base * $type-scale-ratio * $type-scale-ratio / $grid-base-pt
	) * $grid-base-pt;
$font-size-l: math.floor($font-size-base * $type-scale-ratio / $grid-base-pt) *
	$grid-base-pt;
$font-size-m: $font-size-base;
$font-size-s: math.floor(
		($font-size-base / $type-scale-ratio) / $grid-base-pt
	) * $grid-base-pt;
$font-size-xs: math.floor(
		($font-size-base / ($type-scale-ratio * $type-scale-ratio)) / $grid-base-pt
	) * $grid-base-pt;
```

<br>

줄높이 역시 `line-height: 1.5`와 같이 일괄로 적용하지 않고, Type Scales에 따라 폰트 사이즈별로 다른 줄높이 값을 지정하는 것이 좋습니다. 4포인트 그리드 시스템을 사용한다면 줄높이가 `4`의 배수여야 안정감을 주기 때문입니다.

```css
:root {
	/* fallback for old browsers */
	--line-height-2xl: 36px;
	--line-height-xl: 32px;
	--line-height-l: 28px;
	--line-height-m: 24px;
	--line-height-s: 20px;
	--line-height-xs: 16px;
	/* rem */
	--line-height-2xl: 2.25rem;
	--line-height-xl: 2rem;
	--line-height-l: 1.75rem;
	--line-height-m: 1.5rem;
	--line-height-s: 1.25rem;
	--line-height-xs: 1rem;
}
```

<br>

Modular Scale을 구축하기 위해 CSS 전처리기를 사용합니다. 저는 SCSS를 사용했습니다.

```scss
@use "sass:list";
/* modules */
$line-height-ratio: 1.5;

/* line heights */
$line-height-2xl: math.floor(
		$font-size-2xl * $line-height-ratio / $grid-base-pt
	) * $grid-base-pt;
$line-height-xl: math.floor(
		$font-size-xl * $line-height-ratio / $grid-base-pt
	) * $grid-base-pt;
$line-height-l: math.floor($font-size-l * $line-height-ratio / $grid-base-pt) *
	$grid-base-pt;
$line-height-m: math.floor($font-size-m * $line-height-ratio / $grid-base-pt) *
	$grid-base-pt;
$line-height-s: math.floor($font-size-s * $line-height-ratio / $grid-base-pt) *
	$grid-base-pt;
$line-height-xs: math.floor(
		$font-size-xs * $line-height-ratio / $grid-base-pt
	) * $grid-base-pt;
```

<br>

## 6. 폰트 굵기(Font Weight)

웹에서 폰트의 굵기는 [`100` ~ `900` 사이의 9 가지 값](https://helpx.adobe.com/fonts/using/css-selectors.html#Usingmultipleweightsandstyles)을 사용합니다. 폰트에 따라 모든 굵기를 지원하거나 일부 굵기만 지원하기도 합니다. 같은 굵기 값이라도 폰트에 따라 실제 굵기는 다르고요.

<br>

폰트 굵기(Font Weight)는 텍스트간 위계를 나타내는 용도로 사용합니다. 텍스트간 위계를 부여할 때 굵기 대신 폰트 사이즈를 사용할 수도 있으므로 어떤 것이 효과적일지 비교하여 결정해야합니다. 폰트 사이즈가 작은 텍스트에는 얇은 굵기를 적용하는 것을 지양해야합니다. 접근성 문제가 있기 때문입니다.

<br>

## 7. 자간(Letter Spacing)

폰트의 크기가 커지면 자간을 적게 사용하고, 폰트의 크기가 작아지면 자간을 늘려 사용하는 것이 좋습니다. [Guidelines for Letterspacing Type](https://johndjameson.com/blog/guidelines-for-letterspacing-type/)에서 참고할 만한 사항들을 정리해보면 다음과 같습니다.

<br>

- 대문자 : 대문자의 자간은 늘리는 것이 좋음
- 큰 텍스트는 자간을 줄이는 것이 좋습니다. (타이틀 등)
- 본문 텍스트는 기본 자간을 사용하거나 기본 자간과 매우 비슷해야 합니다.
- 작은 텍스트는 자간을 늘려 사용합니다.
- 어두운 배경의 밝은 색 텍스트는 자간을 약간 늘리는 것이 좋습니다.
- 폰트 굵기와 자간은 반비례 관계입니다. 폰트가 굵을 수록 자간은 좁아지고, 폰트 굵기가 얇을 수록 자간은 늘어납니다.

<br>

## 8. 줄 길이(Line length)

줄 길이는 텍스트 박스의 왼쪽과 오른쪽 사이의 간격입니다.
권장되는 줄의 길이는 레퍼런스마다 조금씩 다르지만 데스크탑의 경우 한 줄에 50 ~ 75자, 모바일은 35 ~ 40자 사이가 일반적입니다. [Atlassian Design System](https://atlassian.design/foundations/typography#line-length)은 여백 포함 60 ~ 100자 사이를 이상적인 줄 길이로 제안합니다. 어찌됐든 정확한 줄 길이를 측정하기란 불가능하고, 디바이스마다 뷰포트의 너비 또한 다르기 때문에 폰트 크기와 자간, 디바이스들의 뷰포트 크기를 종합적으로 고려하여 결정하면 되겠습니다. 가장 간단한 방법 중 하나는 가장 보편적일 수 있는 값을 정한 다음 `max-width`로 지정하는 것입니다.

<br>

<img src="./../img/line-length.png" />

사진 출처 : [Atlassian Design System](https://atlassian.design/foundations/typography#line-length)

<br>

줄 길이를 고려해야하는 이유는 줄 길이가 너무 길거나 짧으면 읽는 속도와 이해도가 떨어지기 때문입니다. 다음은 [Wikipedia](https://en.wikipedia.org/wiki/Line_length#Electronic_text)에서 발췌한 가독성 연구 결과에 대한 설명입니다.

<br>

> Legibility research specific to digital text has shown that, like with printed text, line length can affect reading speed. If lines are too long it is difficult for the reader to quickly return to the start of the next line (saccade), whereas if lines are too short more scrolling or paging will be required.

> In order for on-screen text to have both the best speed and comprehension possible about 55 cpl should be used.

<br>

[StackExchange - What is the best number of paragraph width for readability?](https://ux.stackexchange.com/questions/108801/what-is-the-best-number-of-paragraph-width-for-readability) 페이지가 도움이 되었습니다.

<br>

## 9. 폰트 스타일(Font Style)

폰트 스타일에는 `italic`, `bold`, `underline` 등이 있습니다. HTML에서 `<strong>`, `<em>` 등의 태그에 기반하는 스타일들입니다. 다음은 [Building a design system — where to start?](https://uxdesign.cc/building-a-design-system-where-to-start-part-4-typography-5065b8d360c)에서 가져온 몇가지 참고할 만한 규칙들입니다.

<br>

- `italic`과 `bold`는 상호배타적이므로 함께 사용하지 않습니다.
- 폰트 스타일은 최대한 적게 사용합니다.
- `Serif` 계열 폰트에서 부드러운 강조는 `italic`, 강한 강조는 `bold`를 사용합니다.
- `Sans Serif` 계열 폰트에서는 강조시 `italic` 스타일을 사용하지 않습니다. 눈에 띄지 않기 때문입니다.

<br>

---

### References

- [Building a design system — where to start?](https://uxdesign.cc/building-a-design-system-where-to-start-part-4-typography-5065b8d360c)
- [Serif | Oxford Learner's Dictionaries](https://www.oxfordlearnersdictionaries.com/definition/english/serif?q=serif)
- [Typography | Design Systems](https://www.designsystems.com/typography-guides/)
- [A framework to create an accessible & harmonious typography system for faster design-dev handoff](https://blog.prototypr.io/10-practical-steps-to-create-a-predictable-accessible-and-harmonious-typography-system-a-case-6c85d901bedd)
- [A framework for creating a predictable & harmonious spacing system for faster design-dev handoff](https://blog.prototypr.io/a-framework-for-creating-a-predictable-and-harmonious-spacing-system-8eee8aaf773c)
- [Typography | Shopify Design System](https://polaris.shopify.com/design/typography#navigation)
- [Typography | Atlassian Design System](https://atlassian.design/foundations/typography)
- [7 Things To Remember When Selecting Fonts For Your Design](https://uxplanet.org/7-things-to-remember-when-selecting-fonts-for-your-design-ec1e592266c5)
- [The Ultimate Guide to Creating a Design System — Part Two, Typography](https://blog.prototypr.io/the-ultimate-guide-to-creating-a-design-system-part-two-typography-4513dca4476f)
- [Typography in Design Systems - A typographic system that optimizes for guessability](https://superfriendlydesign.systems/articles/typography-in-design-systems/)
- [Create your design system, part 1: Typography](https://medium.com/codyhouse/create-your-design-system-part-1-typography-7c630d9092bd)
- [Mathematical Web Typography](https://jxnblk.com/blog/mathematical-web-typography/)
- [More Meaningful Typography](https://alistapart.com/article/more-meaningful-typography/)
- [Defining a Modular Type Scale for Web UI](https://blog.prototypr.io/defining-a-modular-type-scale-for-web-ui-51acd5df31aa)
- [Guidelines for Letterspacing Type](https://johndjameson.com/blog/guidelines-for-letterspacing-type/)
- [디자인 시스템 구축하기 4부 : 타이포그래피](https://brunch.co.kr/@thinkaboutlove/291)
- [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051)
- [Why we’re using a 4-point grid in Webflow](https://webflow.com/blog/why-were-using-a-4-point-grid-in-webflow)
