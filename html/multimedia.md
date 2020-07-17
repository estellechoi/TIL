# Image and multimedia tags

<br>

## Image and multimedia tags

이미지, 오디오, 비디오 등 멀티미디어 리소스를 지원하는 태그들입니다.

- `<img>`

<br>

## `<img>`

> 이미지 요소

<br>

- `alt` 속성에 대체 텍스트를 제공해야 합니다.

<br>

### 속성 (기본)

- `src` : 이미지 URL

- `alt` : 대체 텍스트

- `crossorigin` : CORS를 사용해 이미지 파일을 가져와야 하는지

  > `anonymous`/`use-credentials`

- `loading` : 로딩 방식

  > `eager` / `lazy`

<br>

### 반응형 이미지

반응형 이미지를 구현하려면 아래의 2 개 속성을 사용합니다.

- `srcset` : 사용할 수 있는 이미지 후보들(경로, 원본 사이즈)

  > 브라우저가 가능한 후보들 중 하나를 자동으로 선택합니다. (사용자의 개인 설정 또는 대역폭 상황에 따라)
  > `@media` 쿼리 대체 !

- `sizes` : 특정 뷰포트 크기 조건에 최적화된 이미지의 사이즈 명시

<br>

아래는 예시입니다.

```html
<img
	srcset="
		images/logo_small.png   400w,
		images/logo_medium.png  700w,
		images/logo_large.png  1000w
	"
	sizes="(max-width: 500px) 444px,
         (max-width: 800px) 777px,
         1222px"
	src="images/logo.png"
	alt="LOGO"
/>
```

<br>

#### `srcset`

- `srcset` 속성은 작은 사이즈의 이미지부터 순서대로 입력합니다.

- 이미지의 크기로 `px` 단위가 아닌 `w`/`x` 디스크립터(Descriptor)를 사용합니다.

- `src` 속성은 `srcset`을 사용할 수 없을 때 유효합니다.

<br>

##### W 디스크립터(Width descriptor)

W 디스크립터(Width descriptor)는 이미지의 원본 크기(가로)를 의미합니다. 브라우저는 지정된 `w` 디스크립터를 통해 각 이미지의 최적화된 픽셀 밀도를 계산합니다.

##### X 디스크립터(Width descriptor)

이미지의 비율을 의미합니다. `x` 디스크립터는 디바이스의 픽셀 비율(Pixel ratio)과 일치하는 값으로 최적화 선택됩니다.

<br>
