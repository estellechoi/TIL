# 개발자를 위한 디자인 시스템 Foundation 3 - 그리드(Grid)

<br>

## 1. 4포인트 베이스라인 그리드(4pt Baseline Grid)

### 1-1. 포인트 베이스라인(pt Baseline)

4포인트 베이스라인 그리드(4pt Baseline Grid)는 UI 디자인에서 모든 것을 `4`의 배수로 하겠다는 개념입니다. `4px`, `8px`, `12px`과 같은 값들만 사용하겠다는 겁니다. 흔히 `8`의 배수만 사용하는 8포인트 그리드 레이아웃이 많이 사용되지만, 최근에는 그리드를 더 세밀하게 나누어 사용할 수 있는 4포인트 그리드가 인기를 얻고 있습니다. 저는 [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051) 등의 아티클을 참고하여 포트폴리오 프로젝트를 위해 4포인트 그리드 레이아웃을 채택했습니다.

<br>

### 1-2. 왜 4, 8인가요?

그런데 왜 `4`, 또는 `8`인가요? `2`와 `4`로만 나눌 수 있기 때문입니다. 이는 고해상도 디바이스에서 [Anti-aliasing](https://en.wikipedia.org/wiki/Anti-aliasing)으로 인해 특정 부분이 흐릿해지는 현상을 방지할 수 있다는 말입니다. 예를 들어 `@ 1.5x` 해상도 디바이스에서 `5px`을 사용하면 실제 픽셀(px) 값은 `7.5px`이 되겠죠. `0.5`만큼 픽셀 절반 값으로 인해 계단현상(Aliasing)이 발생하고요, 화면에서 이를 부드럽게 처리하는 과정에서 가장자리가 흐릿해질 수 있습니다.

<br>

`4`와 `8`은 웹 브라우저의 기본 폰트 크기인 `16px`과도 맞아떨어집니다. 또한 대부분의 웹 브라우저에서 `1px`보다 작은 값은 화면을 그릴 때 반영되지 않습니다. Firefox와 같은 브라우저가 `1px`보다 얇은 값을 지원하기는 하지만, 모든 사용자에게 일관된 경험을 제공하려면 정수 값만 사용하는 것이 좋습니다.

<br>

## 2. 박스모델(Box Model)

그럼 이러한 포인트 베이스라인 그리드는 정확히 어디에 사용될까요? 웹이나 앱에서 화면을 그릴 때 사용하는 [박스모델(Box Model)](https://www.w3schools.com/css/css_boxmodel.asp)의 모든 구성요소에 사용됩니다.

<br>

<img src="./../img/box-model.png" />

<br>

4포인트 그리드 레이아웃을 사용한다면, 하나의 박스를 구성하는 다음 4가지 구성요소의 값들을 `4`의 배수로 지정합니다.

- 컨텐츠(Content) : 박스의 컨텐츠 영역
- 패딩(Padding) : 박스의 컨텐츠를 둘러싼 여백
- 선(Border) : 박스의 선
- 마진(Margin) : 박스 외부의 여백, 주변 요소와의 레이아웃에 사용

<br>

## 3. 모듈식 확장(Modular Scales)

그리드 시스템에서는 사용할 공간 값들을 모듈로 구성합니다. 이 모듈 값들은 일반적으로 특정 비율로 증감하는 값들로 구성하고요, [Modular Scale](https://www.modularscale.com/?1&em&1.618) 등의 도구를 사용하여 증감 값들을 빠르게 추출하고 각 값들을 가장 가까운 `4`의 배수로 반올림하여 구성해볼 수 있습니다.

<br>

## 4. 단위: `px`, `rem`, `em`

<br>

---

### References

- [Window.devicePixelRatio | MDN](https://developer.mozilla.org/ko/docs/Web/API/Window/devicePixelRatio)
- [Baseline grids](https://www.figma.com/best-practices/everything-you-need-to-know-about-layout-grids/baseline-grids/)
- [Goodbye 8-point grid, hello 4-point grid?](https://uxdesign.cc/goodbye-8-point-grid-hello-4-point-grid-1aa7f2159051)
- [Why we’re using a 4-point grid in Webflow](https://webflow.com/blog/why-were-using-a-4-point-grid-in-webflow)
- [The 8-Point Grid | Spec.fm](https://spec.fm/specifics/8-pt-grid)
- [A framework for creating a predictable & harmonious spacing system for faster design-dev handoff](https://blog.prototypr.io/a-framework-for-creating-a-predictable-and-harmonious-spacing-system-8eee8aaf773c)
- [디자인 시스템 구축하기 3부 : 크기와 간격](https://brunch.co.kr/@thinkaboutlove/290)
