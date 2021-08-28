# 개발자를 위한 디자인 시스템 1: 컬러 팔레트

<br>

## 1. 개발자를 위한 디자인 시스템

디자인 시스템을 구축할 때 기본이 되는 컬러(Color), 타이포그래피(Typography), 그리드(Grid) 등과 같은 요소들을 흔히 파운데이션(Foundations)이라고 부릅니다. 이 파운데이션을 기반으로 만들어진 재활용 목적의 UI 디자인 조각들을 컴포넌트(Components)라고 하고요. 디자인 시스템을 구축하는 목적에는 브랜드 아이덴티티, 일관된 사용자 경험, 디자인 생산성(재활용성, 휴먼에러 최소화) 등이 있습니다.

<br>

디자인 시스템은 프론트 개발 관점에서도 매우 중요합니다. 웹 앱의 경우 보통 [SCSS](https://sass-lang.com/)와 같은 라이브러리를 사용하여 CSS 작업을 하게되는데요, 개인적으로 디자인 시스템이 없는 환경에서 몇 번의 프로젝트를 진행하다보니 [`$` 변수](https://sass-lang.com/documentation/variables)로 지정한 스타일 모듈들이 전혀 빛을 발하지 못했습니다. 디자인 시안을 전달받을 때마다 새로운 색, 새로운 여백 값, 새로운 폰트 스타일들이 사용되었기 때문에, 모듈의 재활용이 거의 이루어지지 않았기 때문입니다. 상황이 이렇다보니 `color.scss`, `font.scss`와 같은 스타일 파일들은 제 역할을 하지 못하고 히스토리 저장소의 역할만 하고있었죠.

<br>

디자인 시스템을 도입하면 기획 - 디자인 - 개발 플로우 전체에 걸쳐 생산성을 2 배 이상 높일 수 있겠다는 확신이 들었고, 회사에 적극 제안해서 디자인 시스템을 구축하게 되었습니다. 디자인 전문가가 부재했기 때문에 직접 스터디를 하면서 디자인 시스템을 구축하고 있고요, 미래에 디자인 시스템을 리뉴얼하거나 새로 구축할 일이 생긴다면 빠르게 Cheat하기 위한 목적, 저와 같은 상황에 있는 개발자 분들에게 도움이 되고자하는 마음으로 문서화를 하게 되었습니다.

<br>

## 2. 컬러 팔레트(Color Palettes)

디자인 시스템 파운데이션 중 가장 기본 기본이 되는 컬러 시스템은 팔레트 형태로 몇 가지 색들을 세팅합니다.

<br>

<img src="./../img/color-palettes1.png" />

<br>

## 2. Primary & Secondary Colors

<br>

---

### References

- [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG/)
- [The color system | Material Design](https://material.io/design/color/the-color-system.html#color-usage-and-palettes)
- [Color | Atlassian Design System](https://atlassian.design/foundations/color/)
- [The Ultimate Guide to Creating a Design System — Part One, Colors](https://blog.prototypr.io/the-ultimate-guide-to-creating-a-design-system-part-one-colors-20b1d3f15ee6)
