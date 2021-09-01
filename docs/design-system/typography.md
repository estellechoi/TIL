# 개발자를 위한 디자인 시스템 Foundation 2 - 타이포그래피

<br>

## 1. 타이포그래피(Typography) 시스템 시작하기

타이포그래피(Typography) 시스템의 기본은 타입페이스(Typeface)를 선정하는 것입니다. 보통 1 ~ 2 개의 타입페이스를 크기, 굵기 등으로 확장하여 타이포그래피 시스템을 구축하고요, 당연히 브랜드의 톤앤매너와 가독성을 고려하여 타입페이스를 선정해야합니다. 참고로 [Wikipedia List of typefaces](https://en.wikipedia.org/wiki/List_of_typefaces)에서 모든 타입페이스를 살펴볼 수 있습니다.

<br>

## 2. 타입페이스(Typeface) 선정: Serif vs Sans Serif

타입페이스를 정하는 일에 정답은 없지만, 보통 Serif vs Sans Serif 중에서 폰트의 큰 계열을 정하는 것으로 시작합니다. 한글에서 Serif는 명조체, Sans Serif는 고딕체로 부릅니다.

<br>

다음은 [옥스퍼드 영한사전](https://www.oxfordlearnersdictionaries.com/)에서 검색한 Serif의 뜻입니다.

> Serif : 셰리프(인쇄된 H나 I 같은 활자에서 아래·위에 가로로 나 있는 가는 선)

<br>

### Proportional vs Monospaced

참고로 Serif, Sans Serif는 [Proportional 폰트]()에 속하고요, 특별한 이유가 없다면 일반적으로 모든 글자의 넓이가 동일한 [Monospaced 폰트]() 대신 Proportional 폰트를 사용합니다. Monospaced 폰트를 사용하지말라는 규칙이 있는 것은 아니지만, 애초에 Monospaced 폰트는 글자 하나 하나의 확인이 중요한 코드의 가독성을 높이기 위해 고안된 폰트이고요, Serif와 Sans Serif는 책, 잡지 등에서 사용하기 위해 만들어졌기 때문입니다.

<br>

다음은 도움이 되었던 [Proportional Vs. Monospace Fonts](https://www.techwalla.com/articles/proportional-vs-monospace-fonts)에서 발췌한 내용입니다.

> Benefits and Disadvantages of Monospace Fonts Setting text in a monospace font makes it easier to identify characters by themselves. Because of this, tasks that rely on the easy identification of specific characters, such as programming, benefit from the use of a monospace font. Similarly, a monospace font may be used to format code examples within a page that is otherwise set in a proportional font, in order to make them stand out more easily. Text set in a monospace font is also easier to align, leading to the creation of images constructed using characters, known as "ASCII art." <br/>
> On the other hand, because of the fixed width of all the characters, a block of text set in a monospace font will typically take up more space than the same text set in a proportional font. Additionally, long stretches of monospaced text can blend together visually and, as a result, become harder to read.

<br>

다음은 폰트 서칭과 선택을 도와주는 도구들입니다.

- [FontBase](https://fontba.se/)
- [Font Playground](https://play.typedetail.com/)
- [Pair & Compare](https://www.pairandcompare.net/)
- [FontPair](https://www.fontpair.co/)
- [FontSpark](https://fontspark.app/)
- [Better Font Finder](https://jmattthew.github.io/better-font-finder/better-font-finder.html)

<br>

---

### References

- [Typography | Design Systems](https://www.designsystems.com/typography-guides/)
- [A framework to create an accessible & harmonious typography system for faster design-dev handoff](https://blog.prototypr.io/10-practical-steps-to-create-a-predictable-accessible-and-harmonious-typography-system-a-case-6c85d901bedd)
- [Typography | Shopify Design System](https://polaris.shopify.com/design/typography#navigation)
- [Typography | Atlassian Design System](https://atlassian.design/foundations/typography)
- [7 Things To Remember When Selecting Fonts For Your Design](https://uxplanet.org/7-things-to-remember-when-selecting-fonts-for-your-design-ec1e592266c5)
