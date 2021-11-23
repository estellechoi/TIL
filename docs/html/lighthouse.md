# Lighthouse 퍼포먼스 점수 올리기

<br>

1. Lighthouse 퍼포먼스 리포트
2. Preload: CSS, 폰트, 이미지, 폰트 `crossorigin`, JavaScript 모듈, Webpack 동적 임포트, Preload 주석 사용하기
3. Brotli, Gzip으로 텍스트 압축하기

<br>

## 1. Lighthouse 퍼포먼스 리포트

### 1-1. Lighthouse란

[Lighthouse](https://developers.google.com/web/tools/lighthouse/)는 웹페이지의 상태를 진단하고 퍼포먼스 개선을 위한 가이드를 제공하는 툴입니다. Lighthouse를 사용하는 가장 쉬운 방법으로, Chrome 개발자도구의 `Lighthouse` 탭에서 버튼을 클릭하여 웹페이지 진단을 바로 시작할 수 있습니다. Lighthouse는 다음 5가지 관점에서 웹페이지를 진단합니다.

- [퍼포먼스](https://web.dev/performance-scoring/)
- [PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction)
- [Best pratices](https://web.dev/lighthouse-best-practices/)
- [접근성](https://developer.mozilla.org/en-US/docs/Learn/Accessibility)
- [SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO)

<br>

<img src="./../img/lighthouse4.png" aria-hidden="true" />

<br>

### 1-2. 퍼포먼스 리포트

Lighthouse의 퍼포먼스 리포트는 다음과 같습니다. 주로 로딩 속도에 관련된 지표로 구성됩니다. 사이트 퍼포먼스는 UX와 이에 따른 전환율에도 매우 중요하지만, 속도가 너무 느리면 Google 검색엔진에서 추천하지 않기 때문에 SEO에도 영향이 있습니다.

<br>

<img src="./../img/lighthouse1.png" aria-hidden="true" />

<img src="./../img/lighthouse2.png" aria-hidden="true" />

<img src="./../img/lighthouse3.png" aria-hidden="true" />

<br>

웹페이지에서 요청하는 네트워크 전송 사이즈의 적정 수준은 `1,700` ~ `1,900` KiB 입니다. 통상적으로 `1,600` KiB를 넘지 않도록 하는 것이 좋습니다. 그래야 3G 네트워크 환경에서도 사용자가 10초 이내에 콘텐츠를 볼 수 있기 때문입니다. 네트워크 전송 사이즈를 줄이는 방법에는 다음과 같은 것들이 있습니다.

- [PRPL 패턴: Preload, Render, Pre-cache, Lazy load](https://web.dev/apply-instant-loading-with-prpl/)
- [Minification, 데이터 압축](https://web.dev/reduce-network-payloads-using-text-compression/)
- [JPEG, PNG 대신 WebP 이미지 포맷 사용하기](https://web.dev/serve-images-webp/)
- [JPEG 이미지 압축](https://web.dev/use-imagemin-to-compress-images/)
- [캐싱](https://web.dev/reliable/)

<br>

## 2. Preload: CSS, 폰트, 이미지, 폰트 `crossorigin`, JavaScript 모듈, Webpack 동적 임포트, Preload 주석 사용하기

[Preload](https://web.dev/preload-critical-assets/)는 [PRPL 패턴](https://web.dev/apply-instant-loading-with-prpl/)에 포함된 퍼포먼스 전략 중 하나입니다. 말 그대로 곧 사용하게 될 리소스들을 미리 로드하는 개념인데요, HTML 페이지가 완전히 로드된 후에 필요한 CSS, JavaScipt 파일 등을 순차적으로 로드하는 것이 아니라 HTML 페이지가 로드되는 동안 동시에 리소스들을 미리 로드시켜 전체 로딩 시간을 절약한다는 아이디어입니다.

<br>

다음은 [Preload key requests](https://web.dev/uses-rel-preload/) 문서에서 발췌한 설명입니다.

> The potential savings are based on how much earlier the browser would be able to start the requests if you declared preload links. For example, if app.js takes 200ms to download, parse, and execute, the potential savings for each resource is 200ms since app.js is no longer a bottleneck for each of the requests.

<br>

구사는 굉장히 간단합니다. `<head>` 태그 내에서 리소스 파일을 로드하는 `<link>` 태그에 [`rel="preload"`](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload) 속성값을 지정하면 끝입니다. 단순히 브라우저에게 이 리소스를 미리 로드해야한다고 알려주는 역할을 하는 것 뿐이지만, 효과는 막강합니다. 만약 이 속성을 지정하지 않으면, HTML 페이지 로드가 완료된 후에 뒤늦게 리소스들을 순차적으로 로드합니다. 이때 로드되는 리소스가 많거나 무겁다면 화면 렌더링이 지연되면서 사용자가 체감하는 로딩 시간이 길어지고요, 실제 총 로딩 시간 역시 길어지게 되죠. 따라서 웹앱에서 주요하게 사용되는 CSS, JavaScript, 폰트, 이미지 파일들은 대부분 Preload 하는 것이 좋습니다.

<br>

```html
<head>
	<link rel="preload" as="style" href="critical-styles.css" />
	<link
		rel="preload"
		as="font"
		crossorigin
		type="font/woff2"
		href="myfont.woff2"
	/>
</head>
```

<br>

주의할 점은 리소스의 종류에 따라 로딩 우선순위가 달라지기 때문에 `as` 속성을 사용하여 리소스 종류를 명시해주어야한다는 것입니다. [earprintResource Fetch Prioritization and Scheduling in Chromium](https://docs.google.com/document/d/1bCDuq9H1ih9iNjgzyAL0gpwNFiEP4TZS-YLRp_RuMlc/edit) 문서에서는 Chrome 브라우저에서 다양한 리소스 타입들이 어떻게 분류되고, 어떤 종류의 리소스부터 로딩이 시작되는지에 대한 표를 제공합니다.

<br>

### 2-1. CSS, 폰트, 이미지

CSS 파일을 Preload 할 때 주의할 점이 있습니다. 리소스 파일 내에서 참조하는 다른 리소스가 자동으로 함께 Preload 되지 않는다는 사실입니다. 가령, 아래와 같이 `index.css` 파일 내에서 참조하는 폰트와 이미지 파일이 있다고 해봅시다.

```css
/* index.css */
@import url("https://fonts.googleapis.com/css2?family=DM+Serif+Display&display=swap");

.container {
	background-image: url("https://example.com/img/background/a.jpg");
}
```

<br>

이 폰트와 이미지 파일은 `index.css` 파일이 완전히 로드되고 파싱된 이후에나 브라우저에 의해 발견됩니다. `index.css` 파일이 파싱될 때까지 기다리지 않고 폰트와 이미지 파일을 Preload 시키려면 `<link>` 태그를 사용하여 선언해주어야합니다. [Preload web fonts to improve loading speed](https://web.dev/codelab-preload-web-fonts/) 문서가 도움이 되었습니다.

<br>

### 2-2. 폰트 `crossorigin`

폰트의 경우 아래와 같이 `crossorigin` 속성을 지정합니다. `crossorigin` 속성을 지정하지 않으면 이 파일은 불필요하게 두 번 Fetch 되기 때문입니다. 자세한 내용은 W3C Recommendation 문서 [4.9. Font fetching requirements](https://www.w3.org/TR/css-fonts-3/#font-fetching-requirements) 섹션에서 확인할 수 있습니다.

```html
<link
	rel="preload"
	href="ComicSans.woff2"
	as="font"
	type="font/woff2"
	crossorigin
/>
```

<br>

만약 웹폰트를 사용한다면 아래와 같이 `preconnect` 속성을 사용하여 폰트 서버와의 연결을 미리 시도합니다.

```html
<link rel="preconnect" href="https://fonts.googleapis.com" crossorigin />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
	rel="preload"
	as="font"
	crossorigin
	href="https://fonts.googleapis.com/css2?family=DM+Serif+Display&display=swap"
/>
```

<br>

### 2-3. JavaScript 모듈

JavaScript 모듈을 Preload 하려면, `preload` 대신 [`modulepreload`](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/modulepreload) 속성값을 사용합니다. `modulepreload` 속성을 사용하면 모듈뿐만 아니라 해당 모듈의 의존성들도 함께 로드됩니다.

<br>

### 2-4. Webpack 동적 임포트, Preload 주석 사용하기

[Webpack](https://webpack.js.org/)을 사용한다면, [동적 임포트](https://medium.com/front-end-weekly/webpack-and-dynamic-imports-doing-it-right-72549ff49234) 메소드인 `import()`를 사용할 때 [마법 주석](https://webpack.js.org/api/module-methods/#magic-comments)을 통해 Preload를 선언할 수 있습니다. 예를 들어, 아래와 같이 `App` 컴포넌트에서 `CriticalLibrary` 라이브러리를 임포트할 때 `webpackPreload: true` 주석을 사용하면, `App` 컴포넌트가 요청되는 시점에 `CriticalLibrary` 라이브러리도 함께 요청되어 동시에 로딩이 시작됩니다. 앱이 빌드되었을 때 이 `import` 구문이 `<link rel="preload">` 태그로 변환되기 때문이죠. [Webpack 공식문서](https://webpack.js.org/guides/code-splitting/)에서 자세한 내용을 확인할 수 있습니다.

```javascript
// App.js
import(/* webpackPreload: true */ "CriticalLibrary");
```

<br>

이 기능은 Webpack 4.6.0 버전부터 지원합니다. 이전 버전의 Webpack을 사용해야한다면 [preload-webpack-plugin](https://github.com/GoogleChromeLabs/preload-webpack-plugin) 플러그인을 사용할 수 있습니다.

<br>

[What is Better: Preloading or Caching JavaScript Modules?](https://javascript.plainenglish.io/what-is-better-preloading-or-caching-javascript-modules-246d3573e6ad)에 JavaScript 모듈을 Preload하여 로딩 시간을 얼마나 세이브할 수 있는지 볼 수 있습니다.

<br>

## 3. Brotli, Gzip으로 텍스트 압축하기

- [Gzip 아시나요? 그러면 Brotli는요?](https://snyung.com/content/2021-02-11--Brotli) 블로그 글이 도움이 되었습니다.

<br>

---

### References

- [Lighthouse performance scoring](https://web.dev/performance-scoring/)
- [Ensure text remains visible during webfont load](https://web.dev/font-display/?utm_source=lighthouse&utm_medium=devtools)
- [Avoid enormous network payloads](https://web.dev/total-byte-weight/?utm_source=lighthouse&utm_medium=devtools)
- [Preload critical assets to improve loading speed](https://web.dev/preload-critical-assets/)
- [SPA 초기 로딩 속도 개선하기](https://medium.com/little-big-programming/spa-%EC%B4%88%EA%B8%B0-%EB%A1%9C%EB%94%A9-%EC%86%8D%EB%8F%84-%EA%B0%9C%EC%84%A0%ED%95%98%EA%B8%B0-9db137d25566)
- [Preloading modules](https://developers.google.com/web/updates/2017/12/modulepreload)
- [Webpack and Dynamic Imports: Doing it Right](https://medium.com/front-end-weekly/webpack-and-dynamic-imports-doing-it-right-72549ff49234)
