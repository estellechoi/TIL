# 개발자를 위한 디자인 시스템 Foundation 2 - 타이포그래피

<br>

## 1. 타이포그래피(typography) 시스템 시작하기

타입페이스(Typeface)를 정하는 것으로 타이포그래피(Typography) 시스템 구축을 시작합니다. 타입페이스는 흔히 말하는 폰트(서체)를 의미합니다. 보통 1 ~ 2 개의 타입페이스를 선정한 후 크기, 굵기 등을 확장하여 타이포그래피 시스템을 구축하고요, 타입페이스는 브랜드의 톤앤매너와 가독성을 고려하여 선정해야합니다. [Wikipedia List of typefaces](https://en.wikipedia.org/wiki/List_of_typefaces)에서 존재하는 거의 모든 타입페이스 목록을 확인할 수 있습니다.

<br>

다음은 타이포그래피 시스템 구축을 빠르게 해주는 도구들입니다.

- [FontBase](https://fontba.se/)
- [Font Playground](https://play.typedetail.com/)
- [Pair & Compare](https://www.pairandcompare.net/)
- [FontPair](https://www.fontpair.co/)
- [FontSpark](https://fontspark.app/)
- [Better Font Finder](https://jmattthew.github.io/better-font-finder/better-font-finder.html)

<br>

## 2. 타입페이스(Typeface) 정하기

### 2-1. `Serif` vs `Sans Serif`

많고 많은 타입페이스 중 어떤 것을 선택해야할까요? 보통 `Serif`와 `Sans Serif` 중에서 사용할 Primary 폰트 계열을 정하는 것으로 시작합니다. `Serif`(세리프)는 인쇄된 H나 I 같은 활자에서 아래·위에 가로로 나 있는 가는 선을 의미하고요, 따라서 `Serif` 계열 폰트의 특징은 활자 끝에서 확장된 선이 있다는 것입니다. `Sans Serif`는 반대로 활자 끝이 깔끔하게 떨어지는 폰트 계열입니다. 한글에서 `Serif`는 명조체, `Sans Serif`는 고딕체로 부릅니다.

<br>

<img src="./../img/serif-vs-sans-serif.webp" />

사진 출처 : [Logo Design: Serif vs Sans-Serif](https://www.97thfloor.com/blog/serif-vs-sans-serif)

<br>

호흡이 긴 문장을 읽을 때는 `Serif` 계열 폰트가 가독성이 좋습니다. 반면, 활자에 익숙하지 않은 어린이들이나 장애가 있는 사람들에게는 `Sans Serif` 계열 폰트의 가독성이 더 좋습니다. 예로 [Medium](https://medium.com/) 서비스는 전문지식을 다루는 퀄리티 아티클이 많은 플랫폼이기 때문에 `Serif` 계열 폰트를 사용하는 것 같습니다.

<br>

### 2-2. Web Safe Fonts

모든 주요 브라우저에서 기본으로 제공하는 폰트들을 [CSS Web Safe Fonts](https://www.w3schools.com/cssref/css_websafe_fonts.asp)라고 합니다. 브라우저에 내장되어 있기 때문에 CSS로 선언하는 것만으로 바로 사용할 수 있는 폰트들입니다.

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

폰트를 Proportional/Monospaced 계열로 나눌 수도 있습니다. 특별한 이유가 없다면 모든 글자의 넓이가 동일한 Monospaced 폰트 대신 Proportional 폰트를 사용합니다. Monospaced 폰트를 사용하지말라는 규칙이 있는 것은 아니지만, 애초에 Monospaced 폰트는 글자 하나 하나를 면밀히 체크할 필요가 있는 코드의 가독성을 높이기 위해 고안된 폰트이기 때문입니다. 반면 Proportional 폰트는 책, 잡지 등 일반적인 인쇄물과 화면에서 컨텐츠의 가독성을 높이기 위해 고안된 폰트이죠.

<br>

다음은 [Proportional Vs. Monospace Fonts](https://www.techwalla.com/articles/proportional-vs-monospace-fonts)에서 발췌한 내용입니다.

> Benefits and Disadvantages of Monospace Fonts Setting text in a monospace font makes it easier to identify characters by themselves. Because of this, tasks that rely on the easy identification of specific characters, such as programming, benefit from the use of a monospace font. Similarly, a monospace font may be used to format code examples within a page that is otherwise set in a proportional font, in order to make them stand out more easily. Text set in a monospace font is also easier to align, leading to the creation of images constructed using characters, known as "ASCII art." <br/>
> On the other hand, because of the fixed width of all the characters, a block of text set in a monospace font will typically take up more space than the same text set in a proportional font. Additionally, long stretches of monospaced text can blend together visually and, as a result, become harder to read.

<br>

### 2-4. 폰트 패밀리(Font Family)

사실 타입페이스를 선택한다는 것은 "단 하나의 고정된 폰트"가 아닌 "폰트 패밀리(Font Family)"를 선택하는 것입니다. 아래 사진처럼 하나의 폰트는 `100` ~ `900` 사이의 다양한 굵기 구성을 가지고요, `italic`체를 지원하기도 합니다. 쓰임에 맞게 얅거나 굵은 버전을 사용하고, `italic`체를 사용하기도 하죠. CSS에서 타입페이스를 지정하는 속성의 이름이 `font-family`인 이유이기도 합니다. 폰트 패밀리마다 굵기의 종류와 `italic`체 지원 여부가 다르기 때문에 프로젝트의 성격과 규모를 구현하는데 충분한 패밀리 구성을 갖추었는지 고려하여 타입페이스를 정해야합니다.

<br>

<img src="./../img/font-family.png" />

<br>

### 2-5. 2 개 이상의 타입페이스 사용하기

다음은 2 개 이상의 타입페이스를 사용할 때 참고할만한 사항들입니다.

- 2 ~ 3 개 이상의 타입페이스를 사용하지 않습니다. 주의력을 분산시키기 때문입니다. 폰트의 사이즈와 굵기를 다르게 하는 것으로 충분합니다.

- 서로 명확히 대조되는 타입페이스를 사용합니다. 보통 `Serif` 계열 1 가지와 `Sans Serif` 계열 1 가지를 사용합니다.

- 타입페이스간 위계를 명확히 합니다. Primary 타입페이스와 Secondary 타입페이스로 나누어 사용합니다.

<br>

---

### References

- [Serif | Oxford Learner's Dictionaries](https://www.oxfordlearnersdictionaries.com/definition/english/serif?q=serif)
- [Typography | Design Systems](https://www.designsystems.com/typography-guides/)
- [A framework to create an accessible & harmonious typography system for faster design-dev handoff](https://blog.prototypr.io/10-practical-steps-to-create-a-predictable-accessible-and-harmonious-typography-system-a-case-6c85d901bedd)
- [Typography | Shopify Design System](https://polaris.shopify.com/design/typography#navigation)
- [Typography | Atlassian Design System](https://atlassian.design/foundations/typography)
- [7 Things To Remember When Selecting Fonts For Your Design](https://uxplanet.org/7-things-to-remember-when-selecting-fonts-for-your-design-ec1e592266c5)
