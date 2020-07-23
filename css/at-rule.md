# `@` Rule과 미디어쿼리(Media Query)

<br>

## `@` Rule

아래와 같이 여러 `@` 규칙이 있습니다.

- `@charset` : 스타일 시트에 의해 사용되는 문자 집합 정의

- `@import` : CSS 엔진에게 외부 스타일 시트를 임포트하도록 선언

- `@media` : 미디어쿼리(Media Query)를 사용하여 정의된 조건을 만족하면 해당 콘텐츠를 적용하는 [조건부 그룹 규칙](https://developer.mozilla.org/ko/docs/Web/CSS/At-rule#%EC%A1%B0%EA%B1%B4%EB%B6%80_%EA%B7%B8%EB%A3%B9_%EA%B7%9C%EC%B9%99)

- `@supports` : 브라우저가 주어진 조건을 만족하면 해당 콘텐츠를 적용하는 조건부 그룹 규칙

- `@font-face` : 다운로드되는 외부 폰트의 스타일 지정

- `@keyframes` : 애니메이션 순서대로 중간 단계 스타일 지정

<br>

## `@media`

`@media` 규칙은 특정한 스타일을 미디어 쿼리(Media Query) 결과에 따라 적용/미적용할 때 사용할 수 있습니다. `@media` 규칙 문에는 스타일을 적용할 조건을 2 가지로 나누어 명시하는데요. 아래 규칙 문에서 볼 수 있듯이 그 2 가지 조건은 미디어 유형(`screen`), 미디어 특성(`(min-width: 900px)`)입니다.

```css
@media screen and (min-width: 900px) {
	section {
		padding: 1rem 3rem;
	}
}
```

위의 코드는 미디어가 스크린(`screen`)이고 뷰포트의 너비가 `900px` 이상이면, `section` 요소에 `padding: 1rem 3rem` 스타일을 적용한다는 의미입니다.

<br>

### 미디어 유형

미디어 유형에는 아래 4 가지 중 하나를 명시할 수 있습니다. 명시하지 않으면 기본값이 적용됩니다.

- `all` : 모든 미디어 유형 / 기본값

- `screen` : PC, 스마트폰 등 일반적인 디바이스 스크린

- `print` : 인쇄 결과물 및 출력 미리보기 화면

- `speech` : 음성 합성장치

<br>

### 미디어 특성

아래의 특성들이 많이 사용됩니다.

- `width`

- `height`

- `aspect-ratio` : 스크린의 가로/세로 비율

- `orientation` : 스크린 방향 / 세로가 더 긴 상태(`portrait`), 가로가 더 긴 상태(`landscape`)

- `resolution` : 해상도

- `max-width` : 최대 너비 (해당 너비 이하에서)

- `min-width` : 최소 너비 (해당 너비 이상에서)

<br>

---

### Reference

- [At-rules | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)
- [\@media | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media)
