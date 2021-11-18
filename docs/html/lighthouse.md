# Lighthouse 퍼포먼스 점수 올리기

<br>

1. Lighthouse
2. Brotli, Gzip으로 텍스트 압축하기
3. 네트워크 페이로드 사이즈 줄이기

<br>

## 1. Lighthouse

[Lighthouse](https://developers.google.com/web/tools/lighthouse/)는 웹페이지의 상태를 진단하고 퍼포먼스 개선을 위한 가이드를 제공하는 툴입니다. Lighthouse를 사용하는 가장 쉬운 방법으로, Chrome 개발자도구의 `Lighthouse` 탭에서 버튼을 클릭하여 웹페이지 진단을 바로 시작할 수 있습니다. Lighthouse는 다음 5가지 관점에서 웹페이지를 진단합니다.

- [퍼포먼스](https://web.dev/performance-scoring/)
- [PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction)
- [Best pratices](https://web.dev/lighthouse-best-practices/)
- [접근성](https://developer.mozilla.org/en-US/docs/Learn/Accessibility)
- [SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO)

<br>

<img src="./../img/lighthouse4.png" aria-hidden="true" />

<br>

### 퍼포먼스 보고서

Lighthouse의 퍼포먼스 보고서는 다음과 같습니다. 주로 로딩 속도에 관련된 지표로 구성됩니다. 사이트 퍼포먼스는 UX와 이에 따른 전환율에도 매우 중요하지만, 속도가 너무 느리면 Google 검색엔진에서 추천하지 않기 때문에 SEO에도 영향이 있습니다.

<br>

<img src="./../img/lighthouse1.png" aria-hidden="true" />

<img src="./../img/lighthouse2.png" aria-hidden="true" />

<img src="./../img/lighthouse3.png" aria-hidden="true" />

<br>

## 2. Brotli, Gzip으로 텍스트 압축하기

- [Gzip 아시나요? 그러면 Brotli는요?](https://snyung.com/content/2021-02-11--Brotli) 블로그 글이 도움이 되었습니다.

<br>

## 3. 네트워크 페이로드 사이즈 줄이기

웹페이지에서 요청하는 네트워크 전송 사이즈의 적정 수준은 `1,700` ~ `1,900` KiB 입니다. 통상적으로 `1,600` KiB를 넘지 않도록 하는 것이 좋습니다. 그래야 3G 네트워크 환경에서도 사용자가 10초 이내에 콘텐츠를 볼 수 있기 때문입니다. 네트워크 전송 사이즈를 줄이는 방법에는 다음과 같은 것들이 있습니다.

- [PRPL 패턴](https://web.dev/apply-instant-loading-with-prpl/)
- [Minification, 데이터 압축](https://web.dev/reduce-network-payloads-using-text-compression/)
- [JPEG, PNG 대신 WebP 이미지 포맷 사용하기](https://web.dev/serve-images-webp/)
- [JPEG 이미지 압축](https://web.dev/use-imagemin-to-compress-images/)
- [캐싱](https://web.dev/reliable/)

<br>

### 3-1. PRPL 패턴

PRPL 패턴은 Preload, Render, Pre-cache, Lazy load 4가지 전략을 묶어서 나타내는 말이고요, 각각은 다음을 의미합니다.

- Preload : 가장 중요한 에셋들만 미리 로드
- Render : 첫 번째 라우트 페이지를 가능한 빨리 렌더링
- Pre-cahce : 남은 에셋들은 미리 캐싱
- 나머지 라우트와 중요하지 않은 에셋들은 게으르게 로드

<br>

#### 3-1-1. Preload

[Preload 전략](https://web.dev/preload-critical-assets/)은 말그대로 중요한 에셋들을 미리 로드시키는 방법입니다. 페이지가 로드되기 전에 CSS와 같은 중요한 에셋들을 먼저 로드시키고, 화면이 렌더링될 때 미리 준비된 에셋들을 바로 사용하겠다는 전략입니다.

<br>

아래와 같이 CSS 파일을 로드하는 `<link>` 태그에 `preload` 값을 지정하면 이 전략을 쉽게 구사할 수 있습니다.

```html
<link rel="preload" as="style" href="css/style.css" />
```

<br>

위와 같이 지정하여 CSS 파일이 먼저 로드되더라도 `window.onload` 이벤트의 발생 시점이 늦춰지지는 않습니다. `rel="preload"` 속성은 단순히 브라우저에게 이 에셋이 매우 중요하기 때문에 먼저 로드되어야한다고 알려주는 역할만 하기 때문입니다. 브라우저는 이 파일의 존재를 미리 인지하기 때문에 다른 파일과 동시에 로드를 진행하는 등 가장 효율적인 타이밍에 이 파일을 미리 로드할 수 있게 됩니다. 그렇지 않으면, 브라우저는 HTML 페이지를 먼저 완전히 로드하고, 이 파일의 존재를 뒤늦게 발견하기 때문에 순차적으로 로드하면서 전체 로딩시간이 길어지게 됩니다.

<br>

---

### References

- [Lighthouse performance scoring](https://web.dev/performance-scoring/)
- [Ensure text remains visible during webfont load](https://web.dev/font-display/?utm_source=lighthouse&utm_medium=devtools)
- [Avoid enormous network payloads](https://web.dev/total-byte-weight/?utm_source=lighthouse&utm_medium=devtools)
