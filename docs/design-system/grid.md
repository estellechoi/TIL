# 개발자를 위한 디자인 시스템 Foundation 2 - 그리드(Grid)

<br>

1. 픽셀과 포인트
2. 그리드의 종류
3. 4포인트 베이스라인 그리드(4pt Baseline Grid)
4. CSS 픽셀, DPPX(Device Pixel Ratio), 4픽셀 베이스라인 그리드(4px Baseline Grid)
5. 베이스라인 그리드 적용 범위: 박스모델(Box Model), 타이포그래피 - Escape 베이스라인 그리드
6. 모듈식 스케일(Modular Scales)
7. 단위: `px`, `rem`, `em`

<br>

## 1. 픽셀과 포인트

### 1-1. 픽셀(`px`)

[픽셀](https://en.wikipedia.org/wiki/Pixel)(`px`)은 디지털 화면에서 가장 기본이 되는 단위입니다. 화면을 쪼갤 수 있는 가장 작은 단위를 픽셀, 우리말로는 화소라고 합니다. 물건의 사이즈를 측정할 때 사용하는 `mm`나 `inch`와 같이 고정된 값을 나타내는 단위가 아니기 때문에 동일한 크기의 화면이라도 픽셀 수는 다를 수 있죠. 따라서 `1px`의 실제 물리적 크기는 디바이스 해상도에따라 제각각입니다.

<br>

### 1-2. 포인트(`pt`), `72ppi`

포인트(`pt`)를 이해하려면 역사를 조금 살펴봐야합니다. 포인트는 디지털 디바이스가 없던 시절, 인쇄물의 타이포그래피(Typography)에서 사용하는 가장 작은 단위였습니다. 실제 인쇄된 활자의 크기를 나타내는 단위이므로 `1pt`는 픽셀과 달리 고정된 현실 세계의 값을 나타냅니다. `1pt`는 대략 `1/72inch`(`0.3527mm`) 정도가 됩니다.

<br>

그런데 이 포인트 단위를 픽셀 기반의 디지털 디바이스로 옮겨오는 과정에서 많은 사람들이 혼선을 겪게 되었고요, [Xerox PARC 연구소](<https://en.wikipedia.org/wiki/PARC_(company)>)는 이를 해소하기 위해 디지털 화면의 표준 해상도를 `72 ppi(pixels per inch)`로 채택합니다. 이로써 표준 해상도를 가진 디바이스에서 `1pt = 1px`가 성립하게 되었습니다. 당시 `72 ppi` 표준 해상도를 채택한 Apple PC의 성공으로 `72 ppi`가 보편화되었고요, [Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/#app-icon-sizes) 문서를 보면 현재까지도 Apple 디바이스에서 이 표준해상도를 사용하고있음을 알 수 있습니다. 가령 해상도가 2 배(`@x2`)라면 `1pt = 4px`이 됩니다. 가로, 세로 모두 인치당 픽셀 수가 2 배씩 커졌음을 의미하기 때문이죠.

<br>

위 내용을 이해하는데 [spoqa 기술블로그 픽셀과 포인트](https://spoqa.github.io/2012/07/06/pixel-and-point.html)가 도움이 되었습니다.

<br>

## 2. 그리드의 종류

그리드 레이아웃에는 여러 방법론이 있지만, 일반적인 웹/앱 서비스에 적용할만한 그리드에는 다음의 두 가지가 있을 것 같습니다.

- 컬럼 그리드(Column Grid) : 가독성을 위해 컨텐츠를 컬럼별로 나열하는 그리드, 최초에 잡지에서 사용되었음

- 베이스라인 그리드(Baseline Grid) : 타이포그래피 등에 주로 사용하는 기술적 그리드 방식, 다량의 텍스트가 포함된 모든 디자인에 적합하고, 컨텐츠를 읽을 때 리듬을 만들어줌

<br>

## 3. 4포인트 베이스라인 그리드(4pt Baseline Grid)

### 3-1. 개념

4포인트 베이스라인 그리드(4pt Baseline Grid)는 `4pt`를 기준으로 증감하는 값들만을 사용하는 그리드 레이아웃 방식입니다. 레이아웃을 구성하는 모든 요소의 크기를 `4`의 배수로 하겠다는 개념입니다. `72ppi`의 표준 해상도를 가정하면, `4px`, `8px`, `12px`과 같은 값들만 사용하겠다는 겁니다. 흔히 `8`의 배수만 사용하는 8포인트 베이스라인 그리드가 많이 사용되지만, 최근에는 공간을 더 세밀하게 나눌 수 있는 4포인트 베이스라인 그리드가 인기를 얻고 있습니다. 저는 [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051) 등의 아티클을 참고하여 포트폴리오 프로젝트를 위해 4포인트 그리드 레이아웃을 채택했습니다. 참고로 디지털 화면을 타겟으로 하는 UI 디자인에서는 4픽셀 베이스라인 그리드(4px Baseline Grid)를 사용하는 것이 적합하지만, 이는 뒤에 설명할 CSS 픽셀과 DPPX에 대한 이해가 필요하므로 일단 포인트 단위를 사용하여 설명합니다.

<br>

### 3-2. 왜 4, 8인가요?

그런데 왜 `4`, 또는 `8`인가요? `2`와 `4`로만 나눌 수 있기 때문에, 고해상도 디바이스에서 화면이 깨지는 계단 현상이나 흐릿해지는 현상을 방지할 수 있기 때문입니다. 예를 들어 1.5 배 해상도 디바이스에서 `5pt`를 사용하면 픽셀 값은 `11.25px`(`5 * 1.5 * 1.5`)이 되겠죠. 애초에 픽셀은 화면을 구성하는 가장 작은 단위이기 때문에 정수(Integer) 값보다 `0.25`만큼 초과된 값으로 인해 [계단 현상(Aliasing)](https://en.wikipedia.org/wiki/Aliasing)이 발생할 수 있고요, 디바이스에서 계단현상이 일어난 부분을 자동으로 부드럽게 처리하는 [Anti-aliasing](https://en.wikipedia.org/wiki/Anti-aliasing) 과정에서 화면이 흐릿해질 수도 있습니다. 또는 자동으로 픽셀 값을 반올림 처리하면서 디자인과 다르게 렌더링될 수 있는 문제가 있습니다. 4포인트, 8포인트 베이스라인 그리드에서는 이런 현상이 발생하지 않습니다.

<br>

또한 `4`와 `8`은 웹 브라우저의 기본 폰트 크기인 `16px`과도 맞아떨어집니다.

<br>

## 4. CSS 픽셀, DPPX(Device Pixel Ratio), 4픽셀 베이스라인 그리드(4px Baseline Grid)

### 4-1. CSS 픽셀

[CSS 픽셀](https://developer.mozilla.org/en-US/docs/Glossary/CSS_pixel)은 `72ppi`가 아닌 `96ppi` 해상도를 기준으로 합니다. 이에 따라 Apple의 Safari를 비롯한 대부분의 웹 브라우저도 `96ppi` 해상도를 기준으로 합니다. 따라서 CSS에서는 1 배의 표준 해상도라고해도 엄밀히 말해 `1pt = 1px`이 성립되지 않습니다. CSS `1px`은 `1/96inch`입니다. 가령, CSS에서 `16px`로 지정하면 물리적 크기는 `16pt`가 아닌 `12pt`가 됩니다.

```css
div {
	font-size: 16px; /* = 12pt */
}
```

<br>

`96ppi`는 Microsoft가 개발하고 채택했던 해상도 규격입니다.

<br>

### 4-2. DPPX(Device Pixel Ratio)

어쨋든 우리는 위의 사실을 참고만 하고 DPPX(Device Pixel Ratio)만 이해하면 됩니다. DPPX는 디바이스 픽셀과 CSS 픽셀간 비율입니다. 모든 디바이스는 CSS에서의 `1px`이 실제 화면에서 몇 픽셀을 차지하는지 정해진 비율을 갖습니다. 가령, DPPX가 2 배(`@ 2x`)라면 CSS `1px`이 실제 화면에 렌더링될 때 `2px`을 차지합니다. 덕분에 웹 프론트엔드 개발자는 디바이스별 해상도를 고려할 필요없이 동일한 `px` 값을 사용하여 개발할 수 있는거죠. 다음은 [CSS Length Explained](https://hacks.mozilla.org/2013/09/css-length-explained/#dppx)에서 발췌한 설명입니다.

<br>

> In order to make sure that CSS pixels are sized consistently across every device that accesses the web (i.e. everything with a screen and network connection), device manufacturers had to map multiple device pixels to one CSS pixel to make up for it’s relative bigger physical size. The ratio of the dimension of CSS pixel relative to device pixels is the device pixel ratio (DPPX).

<br>

참고로 JavaScript에서는 `window.devicePixelRatio` 속성을 통해 DPPX 값을 얻을 수 있습니다.

```javascript
console.log(window.devicePixelRatio); // 2
```

<br>

### 4-3. 4픽셀 베이스라인 그리드(4px Baseline Grid)

거의 모든 디바이스가 `@ 1x`, `@ 1.5x`, `@ 2x`, `@ 3x`의 DPPX를 채택하기 때문에 여전히 `4`의 배수 베이스라인 그리드가 유효합니다. 다만, CSS에서 `px`을 `pt`에 맞추기 어렵고, 디지털 매체가 보편화된 지금은 `pt` 보다 `px` 기반 커뮤니케이션이 익숙하기 때문에 4픽셀 베이스라인 그리드(4px Baseline Grid)를 사용하면 되겠습니다. 모든 것이 포인트 베이스라인 그리드와 동일하게 적용됩니다. 가령 `@ 1.5x`에서 `5px`을 사용하면 디바이스에서 인지하는 픽셀 값은 `7.5px`로 계단 현상이 발생할 수 있기 때문에, 4픽셀/8픽셀 베이스라인 그리드를 사용합니다.

<br>

## 5. 베이스라인 그리드 적용 범위: 박스모델(Box Model), 타이포그래피 - Escape 베이스라인 그리드

### 5-1. 박스모델(Box Model)

그럼 이러한 픽셀 베이스라인 그리드는 정확히 어디에 사용될까요? 웹이나 앱에서 화면을 그릴 때 사용하는 [박스모델(Box Model)](https://www.w3schools.com/css/css_boxmodel.asp)의 모든 구성요소와 타이포그래피에 사용됩니다.

<br>

<img src="./../img/box-model.png" />

사진출처 : [W3S 박스모델(Box Model)](https://www.w3schools.com/css/css_boxmodel.asp)

<br>

4픽셀 그리드 레이아웃을 사용한다면, 하나의 박스를 구성하는 4가지 구성요소의 크기를 `4`의 배수로 지정합니다.

- 컨텐츠(Content) : 박스의 컨텐츠 영역
- 패딩(Padding) : 박스의 컨텐츠를 둘러싼 여백
- 선(Border) : 박스의 선
- 마진(Margin) : 박스 외부의 여백, 주변 요소와의 레이아웃에 사용

<br>

### 5-2. 타이포그래피 - Escape 베이스라인 그리드

레이아웃을 구성하다보면 아무리해도 `4`로 나누어떨어지지 않는 경우가 있습니다. 버튼의 여백이 너무 넓거나 좁아지는 경우, 텍스트가 너무 크거나 작은 경우이죠. 이런 경우, 박스의 크기와 줄높이(Line Height)는 `4`의 배수를 유지하고 폰트의 크기(Font Size)는 `4`의 배수에서 벗어나도 괜찮다는 것이 [Material Design](https://material.io/design/layout/spacing-methods.html#baseline-grid)의 설명입니다. 다음은 발췌 내용입니다.

<br>

> Type can be placed outside of the 4dp grid when it’s centered within a component, such as a button or list item. When placed outside of the grid but centered within a component, text can still appear vertically center-aligned.

<br>

## 6. 모듈식 스케일(Modular Scales)

일반적으로 그리드 시스템에서는 특정 비율로 증감하는 값들로 공간을 구성합니다. 그래야 사용자가 화면을 읽어내려갈 때 컨텐츠가 리듬을 갖는다고 보기 때문입니다. [Modular Scale](https://www.modularscale.com/?1&em&1.618) 등의 도구를 사용하면 증감 값들을 빠르게 추출할 수 있습니다. 증감 비율은 [피보나치 수열에 기반한 황금비율(1:1.618)](https://brunch.co.kr/@shaun/14)을 흔히 사용합니다. 또한 4픽셀 베이스라인 그리드 레이아웃을 채택하였으므로, 추출된 각 값들은 가장 가까운 `4`의 배수로 반올림하여 구성해볼 수 있겠습니다. 가령 `16px`을 기준으로 황금비로 증감하는 값들을 추출하면 `3.78`, `6.11`, `9.89`, `16`, `25.89`, `41.89`, `67.77`, `109.66`, `177.42`이고요, 이를 기반으로 4픽셀 베이스라인 그리드 모듈들을 구성하면 아래와 같이 할 수 있습니다.

<br>

```css
:root {
	--grid-1: 1px;
	--grid-2: 4px;
	--grid-3: 8px;
	--grid-4: 16px;
	--grid-5: 24px;
	--grid-6: 40px;
	--grid-7: 68px;
	--grid-8: 108px;
	--grid-9: 176px;
}
```

<br>

## 7. 단위: `px`, `rem`, `em`

지금까지 픽셀을 가지고 얘기했지만, 사실 웹 개발에서는 공간과 타이포그래피에 `rem`을 흔히 사용합니다. `<html>` 태그의 폰트사이즈(기본적으로 `16px`)을 `1rem`으로 환산하는 상대적 단위이기 때문에, 단 하나의 픽셀 값을 조정함으로써 모든 폰트와 주변 여백 값이 자동으로 늘어나고 줄어든다는 사실 때문이죠. 이는 레이아웃의 유연성과 [접근성](https://www.w3.org/WAI/fundamentals/accessibility-intro/ko) 측면에서 장점으로 작용합니다. [Tailwind CSS](https://tailwindcss.com/)에서도 `padding`, `margin` 값으로 `rem`을 사용합니다.

<br>

하지만 항상 `rem`을 사용해야 하는 것은 아닙니다. 어떤 컴포넌트가 상황에 따라 다양한 크기를 갖게하려면 본인 요소의 폰트사이즈를 `1em`으로 환산하는 `em`을 사용하는 것이 좋고, 고정된 값을 반드시 유지해야할 때는 `px`을 사용하는 것이 적합합니다. 그리드 시스템에서도 `px`, `rem`, `em` 각 단위는 용도에 맞게 사용하면 되지만, 그 기준을 찾기가 참 어렵습니다.

<br>

[Create your design system, part 1: Typography](https://medium.com/codyhouse/create-your-design-system-part-1-typography-7c630d9092bd)에서 케이스 스터디를 해볼 수 있습니다. [Comprehensive Guide: When to Use Em vs. Rem](https://webdesign.tutsplus.com/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984)도 도움이 됩니다.

<br>

---

### References

- [Understanding layout | Material Design](https://material.io/design/layout/understanding-layout.html#principles)
- [Adaptivity and Layout | Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/)
- [A Quick Look at Types of Grids for Creating Professional Designs](https://visme.co/blog/layout-design/)
- [Pixel | Wikipedia](https://en.wikipedia.org/wiki/Pixel)
- [CSS pixel | MDN](https://developer.mozilla.org/en-US/docs/Glossary/CSS_pixel)
- [Absolute Lengths: the cm, mm, Q, in, pt, pc, px units | W3C Editor's Draft](https://drafts.csswg.org/css-values/#absolute-lengths)
- [CSS Length Explained](https://hacks.mozilla.org/2013/09/css-length-explained/)
- [픽셀과 포인트 | Spoqa 기술블로그](https://spoqa.github.io/2012/07/06/pixel-and-point.html)
- [Point vs Pixel: What is the difference? | StackExchange](https://graphicdesign.stackexchange.com/questions/199/point-vs-pixel-what-is-the-difference)
- [CSS Box Model | W3C Schools](https://www.w3schools.com/css/css_boxmodel.asp)
- [Window.devicePixelRatio | MDN](https://developer.mozilla.org/ko/docs/Web/API/Window/devicePixelRatio)
- [Baseline grids | Figma](https://www.figma.com/best-practices/everything-you-need-to-know-about-layout-grids/baseline-grids/)
- [Baseline grids & design systems](https://uxdesign.cc/baseline-grids-design-systems-ae23b5af8cec)
- [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051)
- [Why we’re using a 4-point grid in Webflow](https://webflow.com/blog/why-were-using-a-4-point-grid-in-webflow)
- [The 8-Point Grid | Spec.fm](https://spec.fm/specifics/8-pt-grid)
- [A framework for creating a predictable & harmonious spacing system for faster design-dev handoff](https://blog.prototypr.io/a-framework-for-creating-a-predictable-and-harmonious-spacing-system-8eee8aaf773c)
- [Comprehensive Guide: When to Use Em vs. Rem](https://webdesign.tutsplus.com/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984)
- [디자인 시스템 구축하기 3부 : 크기와 간격](https://brunch.co.kr/@thinkaboutlove/290)
- [1px 보다 얇은 디자인 — 라인 편](https://brunch.co.kr/@euid/6)
