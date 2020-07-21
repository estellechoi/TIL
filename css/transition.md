# Transition

<br>

## 목차

1. `transition`

2. `transition-timing-function`

3. `transform`

4. 변형 함수(Transform Function)

<br>

## 1.`transition`

CSS 속성의 시작 값, 종료 값을 지정하여 전환되는 과정을 애니메이션으로 구현하는 속성입니다. `transition`은 아래 속성들의 단축 속성입니다.

- `transition-property` : 대상 속성 / 기본값 `all`

- `transition-duration` : 전환 지속 시간 / 기본값 `0s`

- `transition-timing-function`: 타이밍 함수

- `transition-delay` : 전환 대기 시간 / 기본값 `0s`

<br>

시간을 지정하는 `transition-duration`, `transition-delay` 속성의 값은 단위로 `s`/`ms`를 사용할 수 있습니다.

<br>

### 문법

```
transition: property duration timing-function delay;
```

<br>

### 예시

아래는 간단한 예시입니다.

```css
.box {
	width: 100px;
	height: 100px;
	background-color: FloralWhite;
	border: 3px solid FireBrick;
	transition: width 1s, background-color 1s;
}

.box:hover {
	width: 300px;
	background-color: Gainsboro;
}
```

<br>

## 2. `transition-timing-function`

아래는 대표적으로 가능한 값의 목록입니다.

- `linear` : 일정하게 / 기본값

- `ease` : 빠르게 → 느리게

- `ease-in` : 느리게 → 빠르게

- `ease-in-out` : 느리게 → 빠르게 → 느리게

- `cubic-bezier(n, n, n, n)` : 커스텀 값 / `n`: `0`-`1`

  > 다양한 애니메이션 형태를 이 함수를 이용해 구현할 수 있습니다. [Easing functions Cheat Sheet](https://easings.net/)에서 확인하세요.

- `steps(n)` : 전환 과정을 `n`번 분할하여 단편적으로 보여줌

<br>

> [transition-timing-function | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function)

<br>

## 3. `transform`

요소를 변형시키는 속성이고요, CSS 시각적 서식 모델(Visual Formatting Model)의 좌표 공간을 변경합니다. [변형 가능한 요소에만 `transform` 속성을 지정할 수 있다](https://developer.mozilla.org/ko/docs/Web/CSS/transform#%EB%B3%80%ED%98%95%20%EA%B0%80%EB%8A%A5%ED%95%9C%20%EC%9A%94%EC%86%8C%EB%A7%8C)는 점에 주의하세요.

<br>

## 4. 변형 함수(Transform Function)

값으로는 [변형 함수](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function)를 지정합니다. 변형 함수는 2D 또는 3D 공간 내에서 요소를 회전하고, 크기를 바꾸고, 왜곡하고, 이동할 수 있습니다.

<br>

### 1) 2D 변형 함수

크게 4 가지 변형(이동, 크기, 회전, 기울임)을 지정합니다.

<br>

#### Translate

- `translateX(x)`: X축 이동

- `translateY(y)`: Y축 이동

- `translate(x, y)`: `translateX(x)` + `translateY(y)` 단축 속성

#### Scale

- `scaleX(x)`: X축 크기 변경 / 단위 : (배수)

- `scaleY(y)`: Y축 크기 변경

- `scale(x, y)`: 단축 속성

#### Rotate

- `rotate(r)`: 회전 / 단위 : `deg`

#### Skew

- `skewX(x)`: X축 기울임 / 단위 : `deg`

- `skewY(y)`: Y축 기울임

- `skew(x-deg, y-deg)`: X축 이동

#### 단축

- `matrix(n, n, n, n. n, n)` : 위의 4 가지 변형(이동, 크기, 회전, 기울임) 단축 속성

<br>

### 2) 3D 변형 함수

<br>

---

### Reference

- [transition | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)
- [transition-timing-function | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function)
- [transform | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)
- [\<transform-function\> | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function)
