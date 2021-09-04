# 개발자를 위한 디자인 시스템 Foundation 2 - 타이포그래피

<br>

## 1. 타이포그래피(typography) 시작하기

타입페이스(Typeface)를 정하는 것으로 타이포그래피(Typography) 구축을 시작합니다. 타입페이스는 흔히 말하는 폰트(서체)를 의미합니다. 보통 1 ~ 2 개의 타입페이스를 선정한 후 크기, 굵기 등을 확장하여 타이포그래피 시스템을 구축하고요, 타입페이스는 브랜드의 톤앤매너와 가독성을 고려하여 선정해야합니다. [Wikipedia List of typefaces](https://en.wikipedia.org/wiki/List_of_typefaces)에서 존재하는 거의 모든 타입페이스 목록을 확인할 수 있습니다.

<br>

다음은 타이포그래피 구축을 빠르게 해주는 도구들입니다.

- [FontBase](https://fontba.se/)
- [Font Playground](https://play.typedetail.com/)
- [Pair & Compare](https://www.pairandcompare.net/)
- [FontPair](https://www.fontpair.co/)
- [FontSpark](https://fontspark.app/)
- [Better Font Finder](https://jmattthew.github.io/better-font-finder/better-font-finder.html)

<br>

## 2. 타입페이스(Typeface) 정하기

### 2-1. `Serif` vs `Sans Serif`

많고 많은 타입페이스 중 어떤 것을 선택해야할까요? 보통 `Serif`와 `Sans Serif` 중에서 사용할 Primary 폰트 계열을 정하는 것으로 시작합니다. `Serif`(세리프)는 인쇄된 H나 I 같은 활자에서 아래·위에 가로로 나 있는 가는 선을 의미하고요, 따라서 `Serif` 계열 폰트의 특징은 활자 끝에 확장된 선이 있다는 것입니다. `Sans Serif`는 그 반대입니다. 한글에서 `Serif`는 명조체, `Sans Serif`는 고딕체로 부릅니다.

<br>

호흡이 긴 문장을 읽을 때는 `Serif` 계열 폰트가 가독성이 좋습니다. 반면, 활자에 익숙하지 않은 어린이들이나 장애가 있는 사람들에게는 `Sans Serif` 계열 폰트의 가독성이 더 좋습니다. 예로 [Medium](https://medium.com/) 서비스는 전문지식을 다루는 퀄리티 아티클이 많은 플랫폼이기 때문에 `Serif` 계열 폰트를 Primary로 사용하는 것 같네요.

<br>

<img src="./../img/serif-vs-sans-serif.webp" />

사진 출처 : [Logo Design: Serif vs Sans-Serif](https://www.97thfloor.com/blog/serif-vs-sans-serif)

<br>

### 2-2. Web Safe Fonts

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

### 2-3. Proportional vs Monospaced

폰트를 Proportional/Monospaced 계열로 나눌 수도 있습니다. 특별한 이유가 없다면 모든 글자의 넓이가 동일한 Monospaced 폰트 대신 Proportional 폰트를 사용합니다. 애초에 Monospaced 폰트는 글자 하나 하나를 면밀히 체크할 필요가 있는 코드의 가독성을 높이기 위해 고안된 폰트이기 때문입니다. 반면 Proportional 폰트는 책, 잡지 등 일반적인 인쇄물과 화면에서 컨텐츠의 가독성을 높이기 위해 고안된 폰트이죠.

<br>

다음은 [Proportional Vs. Monospace Fonts](https://www.techwalla.com/articles/proportional-vs-monospace-fonts)에서 발췌한 내용입니다.

> Benefits and Disadvantages of Monospace Fonts Setting text in a monospace font makes it easier to identify characters by themselves. Because of this, tasks that rely on the easy identification of specific characters, such as programming, benefit from the use of a monospace font. Similarly, a monospace font may be used to format code examples within a page that is otherwise set in a proportional font, in order to make them stand out more easily. Text set in a monospace font is also easier to align, leading to the creation of images constructed using characters, known as "ASCII art." <br/>
> On the other hand, because of the fixed width of all the characters, a block of text set in a monospace font will typically take up more space than the same text set in a proportional font. Additionally, long stretches of monospaced text can blend together visually and, as a result, become harder to read.

<br>

### 2-4. 폰트 패밀리(Font Family)

사실 타입페이스를 선택한다는 것은 "단 하나의 고정된 폰트"가 아닌 "폰트 패밀리(Font Family)"를 선택하는 것입니다. 아래 사진처럼 하나의 폰트는 `100` ~ `900` 사이의 다양한 굵기 구성을 가지고요, `italic`체를 지원하기도 합니다. CSS에서 타입페이스를 지정하는 속성의 이름이 `font-family`인 이유이기도 합니다. 폰트 패밀리마다 굵기의 종류와 `italic`체 지원 여부가 다르기 때문에 프로젝트의 성격과 규모를 구현하는데 충분한 패밀리 구성을 갖추었는지 고려하여 타입페이스를 정해야합니다.

<br>

<img src="./../img/font-family.png" />

<br>

### 2-5. 여러 개의 타입페이스를 함께 사용하기

다음은 여러 개의 타입페이스를 사용할 때 참고할만한 사항들입니다.

- 최대 2 ~ 3 개의 타입페이스만 사용합니다. 그 이상은 주의력을 분산시키기 때문입니다. 폰트의 사이즈와 굵기를 다르게 하는 것으로 다양성은 충분합니다.

- 서로 명확히 대조되는 타입페이스를 사용합니다. 보통 `Serif` 계열 1 가지와 `Sans Serif` 계열 1 가지를 사용합니다.

- 타입페이스간 위계를 명확히 합니다. Primary와 Secondary로 나누어 사용합니다.

<br>

## 3. 줄높이(Line Height): 웹접근성, 4pt/8pt Baseline Grid

### 3-1. 웹접근성: 폰트 크기의 1.5 배

줄높이(Line Height)는 가독성과 웹접근성, 그리드와의 조화를 고려하여 정합니다. 다음은 [WCAG Success Criterion 1.4.8](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-visual-presentation.html)에 명시된 텍스트 줄높이에 대한 가이드입니다. 폰트 크기의 최소 1.5 배를 줄높이 공간으로 사용하라는 내용입니다.

<br>

> Line spacing (leading) is at least space-and-a-half within paragraphs

<br>

만약 본문 텍스트의 폰트 사이즈로 `16px`을 사용한다면, 줄높이는 최소 `24px`이어야 웹접근성이 좋다고 보는 것입니다.

<br>

### 3-2. 4pt/8pt Baseline Grid

이제 고려해야할 것은 [그리드(Grid)](https://developer.mozilla.org/ko/docs/Learn/CSS/CSS_layout/Grids)와의 조화입니다. 그리드 시스템에서 모든 Vertical 여백값은 텍스트의 줄높이에 비례해야 조화롭다고 보기 때문입니다. 따라서 텍스트의 줄높이 값은 기본적으로 홀수보다는 잘 나누어 떨어지는 짝수를 사용하는 것이 용이합니다. 본문 텍스트의 폰트 사이즈가 `16px`이라고 가정하고, 웹접근성을 고려하여 `24px`을 줄높이로 채택한다면 [4pt Baseline Grid]()와 [8pt Baseline Grid]()를 모두 사용할 수 있겠죠. `24`가 `4`의 배수이면서 `8`의 배수이기 때문입니다.

<br>

## 4. 폰트 사이즈(Font Size)

### 4-1. 웹접근성: `13px`, `16px`

가장 먼저 기본 사이즈를 정합니다. 웹에서 폰트의 기본 사이즈는 일반적으로 `16px`입니다. 보통 텍스트가 다량 사용되는 본문 영역에 기본 사이즈를 사용합니다. `16px` 보다 작은 크기를 사용하더라도 웹접근성을 고려하여 너무 작은 크기의 텍스트는 사용하지 말아야합니다. 일반적으로 `10pt`(`13px`)을 최소 크기로 보고 있습니다. 텍스트의 접근성에 대한 다양한 리소스는 [WebAIM](https://webaim.org/resources/designers/#text)에서 확인할 수 있습니다.

<br>

한편 `<input>`, `<textarea>` 등의 입력 요소는 사용자들이 정확한 값을 입력하고 쉽게 확인할 수 있어야하기 때문에 폰트 사이즈를 최소 `16px`로 지정하는 것이 좋습니다. iOS Safari에서는 해당 요소의 폰트 사이즈가 `16px` 보다 작을 경우 `16px` 크기로 보이도록 화면을 강제로 줌(Zoom) 시킵니다.

<br>

### 4-2. Type Scales

이제 폰트 크기를 확장하여 팔레트 형태의 사이즈 시스템을 구축합니다. 예를 들어, `16px`을 기점으로 심플하게 `2px`씩 확장하는 사이즈 시스템을 구축한다면 `12px`, `14px`, `16px`, `18px` .. 식으로 구성해볼 수 있겠죠. 다음은 사이즈 팔레트 구축을 빠르게 도와주는 도구들입니다.

- [Type Scale](https://type-scale.com/) : 사용할 디자인 방법론을 선택하면 자동으로 사이즈 팔레트를 구성해주는 사이트
- [archetype](https://archetypeapp.com/#) : 2 개 이상의 타입페이스를 사용한다면 유용한 사이트

<br>

만약 아래와 같이 [Major Second Ratio](https://en.wikipedia.org/wiki/Major_second#Epogdoon)에 기반한 타이포그래피를 구축한다면, `16px`을 기점으로 `1.125` 배씩 스케일링하는 사이즈 팔레트를 구성할 수 있겠습니다. 그 다음 4pt Baseline Grid와 조화를 이루도록 폰트 크기들이 4의 배수 위주로 구성될 수 있도록 조정합니다: `11px`, `12px`, `14px`, `16px`, `18px`, `20px`, `24px`, `28px`

<br>

<img src="./../img/type-scales.png" />

<br>

### 4-3. 네이밍

티셔츠의 사이즈 시스템을 흔히 사용합니다. 다음과 같은 식이죠.

```css
:root {
	--font-size-xs: 12px;
	--font-size-s: 14px;
	--font-size-m: 16px;
	--font-size-l: 18px;
	--font-size-xl: 20px;
	--font-size-xxl: 22px;
}
```

<br>

네이밍의 핵심은 직관성과 확장성인데, 이 관점에서 위의 티셔츠 시스템은 적합하지 않다는 의견이 있습니다.

<br>

## 5. 폰트 굵기(Font Weight)

폰트 굵기(Font Weight)는 텍스트간 위계를 정확하게 전달하는 용도로 사용합니다. 간단한 서비스라면 보통 굵은 폰트, 중간 폰트, 얅은 폰트 3 가지로 충분합니다. 텍스트간 위계를 부여할 때 굵기 대신 폰트 사이즈를 사용할 수도 있으므로 어떤 것이 효과적일지 고려해봐야합니다.

<br>

---

### References

- [Serif | Oxford Learner's Dictionaries](https://www.oxfordlearnersdictionaries.com/definition/english/serif?q=serif)
- [Typography | Design Systems](https://www.designsystems.com/typography-guides/)
- [A framework to create an accessible & harmonious typography system for faster design-dev handoff](https://blog.prototypr.io/10-practical-steps-to-create-a-predictable-accessible-and-harmonious-typography-system-a-case-6c85d901bedd)
- [Typography | Shopify Design System](https://polaris.shopify.com/design/typography#navigation)
- [Typography | Atlassian Design System](https://atlassian.design/foundations/typography)
- [7 Things To Remember When Selecting Fonts For Your Design](https://uxplanet.org/7-things-to-remember-when-selecting-fonts-for-your-design-ec1e592266c5)
- [The Ultimate Guide to Creating a Design System — Part Two, Typography](https://blog.prototypr.io/the-ultimate-guide-to-creating-a-design-system-part-two-typography-4513dca4476f)
- [Typography in Design Systems - A typographic system that optimizes for guessability](https://superfriendlydesign.systems/articles/typography-in-design-systems/)
- [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051)
- [Why we’re using a 4-point grid in Webflow](https://webflow.com/blog/why-were-using-a-4-point-grid-in-webflow)
