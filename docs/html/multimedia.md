# 이미지와 멀티미디어 태그(Image and multimedia tags)

이미지, 오디오, 비디오 등 멀티미디어 리소스를 지원하는 태그들입니다.

- `<img>`

- `<audio>`

- `<video>`

- `<map>`

- `<area>`

- `<track>`

> 멀티미디어 리소스가 보여질 영역을 나타내기 위해 `<figure>`/`<figcaption>` 태그를 사용합니다.

<br>

## `<img>`

문서에 이미지를 삽입하는 태그입니다.

<br>

### 속성 (기본)

- `src` : 이미지 URL (필수)

- `alt` : 대체 텍스트

- `crossorigin` : CORS를 사용해 이미지 파일을 가져와야 하는지

  > `anonymous`/`use-credentials`

- `loading` : 로딩 방식

  > `eager` / `lazy`

<br>

### 반응형 이미지를 위한 속성

> IE 지원 X

<br>

반응형 이미지를 구현하려면 아래의 2 개 속성을 사용합니다.

- 1\) `srcset` : 사용할 수 있는 이미지 후보들 (`path size`)

  > `@media` 쿼리 대체 !

- 2\) `sizes` : 특정 뷰포트 크기 조건에 최적화된 이미지의 사이즈 명시

<br>

아래와 같이 사용합니다.

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

### 1) `srcset`

브라우저가 `srcset`에 명시한 이미지 후보들 중 하나를 자동으로 선택합니다.

- 작은 사이즈의 이미지부터 순서대로 입력합니다.

- 이미지의 사이즈를 지정하는 단위는 `w`/`x` Descriptor 입니다. (`px` 말구요..)

- `src` 속성을 함께 지정하면, `srcset` 속성에 지정한 이미지를 사용할 수 없을 때 유효합니다.

<br>

#### `w` Descriptor (Width)

이미지 원본의 사이즈(가로)를 의미합니다. 브라우저는 지정된 `w` Descriptor를 통해 각 이미지의 최적화된 픽셀 밀도를 계산합니다.

<br>

#### `x` Descriptor (Device pixel ratio)

디바이스의 픽셀 비율(Device pixel ratio)과 일치하는 값으로 최적화 선택됩니다.

<br>

> `w` Descriptor를 사용하면 `x`를 사용하지 않아도 됩니다. 보통 `w` 사용을 추천합니다.

<br>

### 2) `sizes`

미디어 조건(뷰포트의 사이즈)와 그 조건에 최적화된 이미지의 사이즈를 지정합니다.

예를 들어 `sizes="(min-width: 1000px) 700px"`라고 지정하면, 뷰포트의 너비가 `1000px` 이상일 때 최적화된 이미지의 출력 너비는 `700px`이라는 뜻입니다. 따라서 브라우저는 이미지의 너비를 `700px`에 맞추어 출력하는데요, 출력할 이미지는 `srcset`의 이미지 중 너비가 `700px` 이거나 더 큰 이미지를 우선적으로 사용합니다. 이는 크기가 더 작은 이미지를 사용하면 비트맵 이미지의 경우 깨짐이 발생할 수 있기 때문입니다.

`sizes`와 `width` 속성을 같이 작성하면 `width`가 우선합니다.

<br>

## `<audio>`

문서에 소리 콘텐츠를 포함할 때 사용합니다. 아래와 같이 사용합니다.

```html
<audio controls src="/media/examples/t-rex-roar.mp3">
	Your browser does not support the
	<code>audio</code> element.
</audio>
```

<br>

### 속성

- `src` : 오디오 URL

- `autoplay` : 전체 오디오 파일의 다운로드를 기다리지 않고 가능한 빠른 시점에 재생을 시작합니다.

  > 오디오 및 오디오를 가진 비디오를 자동으로 재생하는 사이트는 사용자 경험에 악영향을 끼칠 수 있으므로 피해야 합니다. 반드시 자동 재생을 제공해야 한다면 사용자의 명시적인 동의를 얻어야 하도록 해야 합니다.[미디어 및 Web Audio API 자동 재생 가이드](https://developer.mozilla.org/ko/docs/Web/Media/Autoplay_guide)를 참고하세요.

- `controls` : 오디오 재생, 볼륨, 탐색, 일시 정지 컨트롤을 브라우저에서 제공합니다.

  > 이 속성을 명시하지 않으면, 브라우저에서 오디오를 시각적으로 확인하거나 조작할 수 없습니다.

- `crossorigin` : CORS 사용 여부입니다.

- `loop` : 재생이 끝나면 자동으로 처음부터 다시 재생합니다.

- `muted` : 오디오의 초기 음소거 여부입니다. (지정하지 않으면 기본값은 `false` 입니다.)

- `preload` : 페이지가 로드될 때 오디오 파일도 로드할지 여부입니다.
  - `none` : 로드하지 않습니다.
  - `metadata` : 오디오의 메타데이터(e.g. length)만 가져옵니다.
  - `auto`/`""` : 사용자가 오디오를 사용하지 않더라도 무조건 로드합니다.

<br>

`autoplay`와 `preload` 속성을 함께 지정하면, `autoplay`가 우선합니다.

<br>

## `<video>`

비디오 플레이백을 지원하는 미디어 플레이어를 문서에 삽입합니다. 아래와 같이 사용합니다.

```html
<video controls width="250">
	<source src="/media/examples/flower.webm" type="video/webm" />
	<source src="/media/examples/flower.mp4" type="video/mp4" />

	Sorry, your browser doesn't support embedded videos.
</video>
```

<br>

### 속성

- `src`

- `autoplay`

- `controls`

- `crossorigin`

- `loop`

- `muted`

- `preload`

- `poster` : 동영상 썸네일 이미지 URL. 이 속성이 명시되지 않으면, 첫 번째 프레임이 사용 가능하게 될때까지 아무것도 출력되지 않다가, 가능하게 되면 첫 번째 프레임을 포스터 프레임으로 출력합니다.

- `width` : 동영상의 너비 (`px`)

- `played` : 재생된 동영상 영역을 나타내는 [TimeRanges](https://developer.mozilla.org/ko/docs/Web/API/TimeRanges) 객체 입니다.

<br>

## `<figure>`/`<figcaption>`

- `<figure>` : 이미지나 삽화, 도표 등의 영역을 지정합니다.

- `<figcaption>` : `<figure>` 요소 내에서 사용하며, 이미지 등의 멀티미디어에 대한 설명을 작성합니다.

<br>

아래와 같이 사용합니다.

```html
<figure>
	<figcaption>Listen to the T-Rex:</figcaption>
	<audio controls src="/media/examples/t-rex-roar.mp3">
		Your browser does not support the
		<code>audio</code> element.
	</audio>
</figure>
```

```html
<figure>
	<img src="./tree.png" />
	<figcaption>Tree image</figcaption>
</figure>
```

<br>

---

### References

- [\<img\>: The Image Embed element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)
- [Responsive images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)
- [\<audio\>: The Embed Audio element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio)
- [\<video\>: The Video Embed element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)
