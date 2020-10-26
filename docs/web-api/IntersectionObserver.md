# Intersection Observer API

<br>

## Intersection Observer 란?

Intersection Observer API는 스크롤 애니메이션을 구현할 수 있는 (아마도) 가장 쉬운 방법입니다. 단순하게 설명하자면, 특정 엘리먼트를 계속 감시하고 있다가, 사용자가 화면을 스크롤하여 해당 엘리먼트가 화면에 노출되었을 때 원하는 애니메이션이 작동하도록 지정하는 방식입니다. 만약 Intersection Observer API가 없다면 스크롤 애니메이션을 한땀 한땀 직접 구현하는 수밖에 없습니다. `scroll` 이벤트 타입에 대한 이벤트리스너의 콜백함수에서 원하는 엘리먼트가 노출되었는지를 매번 검사하는 복잡한 코드를 작성해야하겠죠..

Intersection Observer는 그 이름 그대로 특정 엘리먼트와 상위 엘리먼트가 서로 교차하는지 여부를 관찰합니다. 즉, 상위 엘리먼트, 흔히 뷰포트(화면) 영역 내에 해당 엘리먼트가 노출이 되었는지 여부를 알 수 있습니다. 이 작업을 비동기적으로 할 수 있도록 지원하고요. 우리는 `IntersectionObserver` 객체의 콜백함수에서 원하는 엘리먼트의 스크린 노출여부를 확인하고 원하는 로직만 처리하도록 하면 됩니다. 아래 예제를 보세요.

<br>

## `IntersectionObserver` 사용하기

```javascript
const options = { threshold: 1.0 };

const observer = new IntersectionObserver((entries, observer) => {
	entries.forEach((entry) => {
		if (entry.isIntersecting) observer.unobserve(entry.target);
	});
}, options);

// the targets
const target = document.getElementById("animation-target");
observer.observe(target);
```

먼저 `IntersectionObserver` 객체를 생성하면서 두 가지를 인자로 전달하는데요, 콜백함수와 옵션입니다. 그 다음, `IntersectionObserver` 객체의 `observe()` 메소드를 사용하여 관찰하고자 하는 엘리먼트를 등록하세요.

<br>

## 콜백

첫 번째 인자로 `entries` 배열, 두 번째 인자로 `IntersectionObserver` 객체를 받습니다. `entries` 배열에 관찰하고 있는 모든 엘리먼트와 그에 대한 정보가 담겨있습니다. 아래와 같은 속성들을 사용할 수 있죠.

- `target` : 타겟 엘리먼트

- `time` : 노출/비노출된 시각

- `isIntersecting` : 노출여부

- `intersectionRatio` : 노출된 비율

- `intersectionRect` : 노출된 영역

- `boundingClientRect` : 타겟 엘리먼트의 `getBoundingClientRect()` 값

- `rootBounds` : 기준 엘리먼트의 뷰포트 영역 값, 만약 옵션에서 지정하지 않았다면 브라우저 뷰포트 크기

<br>

## 옵션

`IntersectionObserver` 객체를 생성할 때 두 번째 인자로 옵션 값을 넘깁니다. 객체 형태로 넘겨야 하고요. 이 옵션에는 `root`, `rootMargin`, `threshold`가 있습니다.

<br>

### 1) `root`

타겟 엘리먼트의 노출여부를 결정할 때 기준으로 할 영역을 지정하는 속성입니다. `root` 값으로 지정한 상위 엘리먼트의 뷰포트 영역을 기준 영역으로 사용합니다. `root` 값을 지정하지 않거나 `null`로 지정하면, 기본값인 브라우저 뷰포트가 사용됩니다.

당연히 `root`는 타겟 엘리먼트의 상위 엘리먼트 중에서 지정할 수 있고요. 그렇지 않으면 타겟 엘리먼트가 화면에 노출되더라도 "노출되었다"고 판단하지 않고 무시해버립니다.

<br>

### 2) `rootMargin`

[CSS `margin`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)과 같이 `0px 0px 0px 0px` 형태로 지정합니다. `rootMargin` 값이 있으면, `threshold` 값에 따라 면적을 계산할 때도 `rootMargin` 영역을 포함하여 계산합니다.

<br>

### 3) `threshold`

`threshold` 속성에 비율 값을 지정합니다. 이 비율은 특정 엘리먼트를 관찰할 때 해당 엘리먼트가 몇 퍼센트 노출되어야 "노출되었다"고 판단할 것인지를 결정하는 값입니다. `number`/`Array<number>` 타입으로 지정합니다. 예를 들어, `{ threshold: 0.5 }` 라고 지정하면, 엘리먼트의 50%가 뷰포트 영역 내에 노출되어야 `isIntersecting` 값으로 `true`를 반환받을 수 있습니다.

`threshold` 값으로 `Array<number>` 타입의 값을 지정하면, 배열의 각 비율만큼 노출될 때마다 콜백 함수를 호출할 수 있습니다.

<br>

## 게으른 로딩(Lazy Loading)

Intersection Observer API는 스크롤 애니메이션 외에도 쓰임새가 많은데요, 게으른 로딩(Lazy Loading)도 쉽게 구현할 수 있습니다. 게으른 로딩이란, 사용자가 어떤 웹 페이지에 접근할 때 모든 콘텐츠(이미지, 비디오 등)을 대량으로 로드하는 대신 사용자가 페이지의 일부분에 접근할 때 해당 영역에서 필요한 콘텐츠만 로드하는 기법입니다.

<br>

아래는 이미지를 게으르게 로딩해보는 예제입니다. 이미지를 마크업할 때 `src` 속성에 빈 이미지(`empty.png`)를 지정하고요, 실제 로드할 이미지는 파일명을 데이터로 저장합니다.

```html
<img class="lazy-img" src="empty.png" data-src="image1.png" />
<img class="lazy-img" src="empty.png" data-src="image2.png" />
```

<br>

이제 위의 이미지 엘리먼트들을 `IntersectionObserver`의 관찰대상으로 등록해보죠.

```javascript
// the img elements
const lazyImgs = Array.from(document.getElementsByClassName("lazy-img"));

const observer = new IntersectionObserver(
	(entries, observer) => {
		entries.forEach((entry) => {
			if (entry.isIntersecting) {
				// src changes to the real image to be loaded.
				entry.target.src = entry.target.dataset.src;
				observer.unobserve(entry.target);
			}
		});
	},
	{ threshold: 0 }
);

// now you guys gonna be watched for intersection.
observer.observe(lazyImgs);
```

<br>

위의 콜백함수 부분을 보세요. 이미지 엘리먼트들이 화면에 노출되기 시작할 때 `src` 속성 값을 실제 로드할 이미지로 바꿔주는 코드가 실행되겠네요. 비로소 실제 이미지가 (게으르게) 로드됩니다.

<br>

## 브라우저 호환성

Internet Explorer에서는 `IntersectionObserver`를 지원하지 않기 때문에 IE 사용자들도 지원해야 한다면 [IntersectionObserver polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)을 사용하세요. 이 Polyfill은 IE7까지 사용할 수 있습니다.

<br>

---

### References

- [IntersectionObserver | MDN](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver)
- [Intersection Observer API | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [Intersection Observer 간단 정리하기](https://medium.com/@pks2974/intersection-observer-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-fc24789799a3)
