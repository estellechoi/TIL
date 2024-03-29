# PWA(Progressive Web App)

<br>

1. PWA: PWA란, 브라우저 호환성, 예제 사이트, 완성도 측정하기, PWA를 네이티브 앱으로 포장하기
2. 설치 가능하게 하기: 최소조건, 설치하기
3. 설치 유도하기: `beforeinstallprompt` 이벤트, `prompt()` 메소드로 설치 Prompt 띄우기, 설치여부 감지, UX 가이드라인
4. Manifest: 자동생성 툴, Manifest 파일, Manifest 항목
5. 아이콘 규격: `png` 포맷, OS별 사이즈
6. Service Worker API: Service Worker란, Service Worker 등록하기
7. 오프라인 Fallback 페이지 제공하기: Service Worker, Cache Storage
8. 브라우저에서 알림 전송하기: Notification API, 권한 핸들링, 알림 전송, 알림 닫기, 이벤트 핸들링
9. 서버에서 알림 전송하기: Push API, 구독상태 확인, 애플리케이션 서버 키, `push` 이벤트
10. 접속모드(브라우저/PWA)에 따라 다르게 스타일링하기
11. PWA와 네이티브 앱
12. iOS에서의 PWA

<br>

## 1. PWA: PWA란, 브라우저 호환성, 예제 사이트, 완성도 측정하기, PWA를 네이티브 앱으로 포장하기

### 1-1. PWA란

PWA(Progressive Web App)란 말그대로 점진적인 웹앱(Web app)입니다. 웹사이트와 네이티브 앱의 장점을 모두 갖도록 개발된 웹의 확장판 정도로 이해해볼 수 있는데요, 설치 가능, 쉬운 동기화, 푸시 알림, 오프라인에서 동작 등 네이티브 앱에서만 가능하던 기능들을 갖춘 웹앱을 말합니다. 중요한 것은 PWA로 식별되는 기능들을 오래된 브라우저가 지원하지 않더라도 사용자들이 웹을 이용할 수 있도록 "점진적으로" 적용하는 것입니다. 모든 사용자가 웹앱을 정상적으로 사용할 수 있어야하며, 최신 브라우저 사용자는 PWA 기능으로부터 더 많은 이점을 얻을 수 있도록 한다는 개념입니다.

<br>

다음은 웹앱을 기존의 일반적인 웹앱과 달리 PWA로 식별하기 위한 몇 가지 핵심 원칙입니다. 이전에는 네이티브 앱에서만 가능하던 기능들입니다.

- 설치 가능 : 디바이스의 홈 화면에 추가하여 사용할 수 있음

- 네트워크 독립적 : 오프라인이나 불안정한 네트워크 환경에서 동작함

- 재참여(Re-engageable) : 새로운 컨텐츠가 사용 가능할 때마다 알림 전송이 가능함

<br>

다음 나열된 특징들 역시 PWA의 원칙들이지만, 기존 웹앱의 특징이면서 지향점이기도 하죠.

- 발견 가능 : 브라우저의 검색엔진을 통해 찾을 수 있음

- 연결 가능 : URL을 공유하여 접근할 수 있음

- 점진적 : 이전 브라우저의 기본 기능을 여전히 사용할 수 있어야함

- 반응형 : 모든 디바이스, 모든 브라우저에서 호환됨

- 안전 : 사용자의 민감한 데이터에 접근하려는 시도로부터 안전함

<br>

PWA의 장점은 네이티브 앱의 편리함과 웹의 접근성을 모두 잡을 수 있다는 것입니다. 네이티브 앱의 편리하고 부드러운 UX를 제공하면서, 웹의 검색엔진을 통하거나 링크를 공유하여 서비스에 바로 접근할 수 있도록 합니다. 앱스토어에서 앱을 검색하고 다운로드할 필요가 없고요, 자주 사용하는 웹사이트를 스마트폰이나 노트북의 홈화면에 앱처럼 추가하여 사용하는 방식입니다. FIRE(Fast, Integrated, Reliable, Engaging)는 PWA 전략을 나타내는 키워드들입니다.

<br>

### 1-2. 브라우저 호환성

PWA는 하나의 독립된 기술을 명명하는 말이 아닙니다. 위에 나열된 기능들을 다수 갖추고 있는 웹앱을 PWA, 점진적인 웹앱이라고 부를 뿐입니다. PWA로 식별될 수 있는 기능들은 각각 해당하는 웹기술을 사용하여 구현하면 되고요, 그 중 핵심적인 기술이 [Service Worker](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)라는 웹 API이기 때문에, 보통 Service Worker API 사용이 가능한 브라우저라면 PWA를 지원한다고 말합니다. [Is Service Worker Ready?](https://jakearchibald.github.io/isserviceworkerready/#moar)에서 Service Worker API의 브라우저 지원 현황을 확인할 수 있습니다. 또한 [Edge](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps-chromium/#requirements), [Firefox](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Add_to_home_screen#How_do_you_make_an_app_A2HS-ready), [Opera](https://dev.opera.com/articles/installable-web-apps/), [Samsung Internet](https://hub.samsunginter.net/docs/ambient-badging/), [UC Browser](https://plus.ucweb.com/docs/pwa/docs-en/zvrh56) 각 브라우저의 공식사이트에서 PWA를 구성하는 기술별로 브라우저 지원 범위를 확인할 수 있습니다. 지원 현황을 아주 대략적으로 요약해보면 아래와 같습니다.

- Safari 11.1부터 대부분 지원 (iOS용 Safari 푸시알림 불가)
- Chrome 40부터 대부분 지원 (iOS용 Chrome 미지원)
- MS Edge 17부터 대부분 지원

<br>

현시점 기준 PWA 지원 범위가 가장 좁은 브라우저는 iOS용 Safari입니다. iOS 상에서는 다른 브라우저들도 몇가지 기능이 제한되는데요, iOS에서는 결국 Safari가 웹페이지의 최종적인 해석과 렌더링을 담당하기 때문입니다. 대표적으로 iOS에서는 어떤 브라우저에서도 푸시알림을 보낼 수가 없습니다. iOS용 Safari에서 PWA를 구현하려면 별도의 섹션으로 분리한 [iOS에서의 PWA](./#ios에서의-pwa)를 참고하세요.

<br>

### 1-3. 예제 사이트

- [Hacker News readers as Progressive Wep Apps](https://hnpwa.com/) : React, Vue 등 프론트엔드 툴을 사용하여 개발한 PWA 예제
- [Service Worker Cookbook](https://serviceworke.rs/) : Service Worker 사용 및 푸시 알림 예제
- [PWA Stats](https://www.pwastats.com/) : PWA 적용사례

<br>

### 1-4. 완성도 측정하기

아래의 툴, 체크리스트를 사용하여 웹앱이 PWA로서 얼마나 "잘" 작동하고 있는지, 얼마나 많은 사용자들이 PWA를 통해 웹앱에 접속하는지 검사할 수 있습니다. PWA 완성도를 정량적으로 측정할 수 있는 [Lighthouse](https://developers.google.com/web/tools/lighthouse#cli)가 유명합니다.

- [Tools for PWA Developers | Google Developers](https://developers.google.com/web/ilt/pwa/tools-for-pwa-developers)
- [Chrome Flags](chrome://flags/) > Bypass user engagement checks
- [Lighthouse](https://developers.google.com/web/tools/lighthouse#cli)
- [Measuring Impact](https://pwa-book.awwwards.com/chapter-8)
- [What makes a good Progressive Web App?](https://web.dev/pwa-checklist/)

<br>

### 1-5. PWA를 네이티브 앱으로 포장하기

[PWABuilder](https://www.pwabuilder.com/), [Trusted Web Activity](https://developers.google.com/web/android/trusted-web-activity) 등의 툴을 사용하여 PWA를 네이티브 앱으로 포장하여 Google Playstore에 등록할 수도 있습니다. 자세한 내용은 아래 문서를 참고하세요.

- [Using a PWA in your Android app](https://web.dev/using-a-pwa-in-your-android-app/)
- [Trusted Web Activities Quick Start Guide](https://developers.google.com/web/android/trusted-web-activity/quick-start)

<br>

## 2. 설치 가능하게 하기: 최소조건, 설치하기

위에서 소개한 PWA로 식별되기 위한 핵심원칙들을 모두 충족해야하는 것은 아닙니다. 브라우저마다 지원 범위 차이도 있고요. 가능한 기능들부터 점진적으로 제공해나가면 된다고 봅니다. 이번 섹션에서는 "설치 가능"에 대해 설명합니다. 

<br>

### 2-1. 최소조건

아래의 최소 조건을 충족하면 "설치 가능한" 웹앱이 됩니다. 최소 조건을 만족하면서, 동시에 앱이 사용자의 OS에 설치되어있지 않다면, 대부분의 브라우저는 사용자가 웹앱을 디바이스에 설치하도록 자동으로 Prompt를 띄워 유도합니다. 참고로 최소조건은 브라우저마다 약간의 차이가 있습니다.

- HTTPS를 통해 제공
- [Service Worker](./#5-service-worker) 등록 완료 (Android용 Chrome에서 필수)
- [Manifest](https://web.dev/add-manifest/)파일을 포함하고, 이 `json` 포맷의 파일은 [최소 아래의 항목들을 포함](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Add_to_home_screen#manifest)

```json
{
	"name": "name",
	"icons": [],
	"start_url": "/",
	"display": "fullscreen",
	"prefer_related_applications": false
}
```

<br>

참고로 Manifest 파일을 작성할 때,

- `name` 대신 `short_name`만 포함해도 됩니다.
- `prefer_related_applications`의 기본값은 `false`이므로 명시하지 않아도 됩니다.
- `related_applications` 항목에 관련된 네이티브 앱을 지정하고, `prefer_related_applications` 값을 `true`로 지정하면, Android에서 앱 설치를 유도하기 위해 사용자를 자동으로 Google Playstore로 이동시킵니다.

<br>

Service Worker, Manifest 파일에 대한 자세한 내용은 섹션 `4` ~ `7`에서 다룹니다.

<br>

### 2-2. 설치하기

웹앱을 설치하는 방법은 간단합니다. iOS에서는 `홈 화면에 추가` 개념으로 지원하고요, Android에서는 `앱 설치`를 지원합니다. Android의 경우 `앱 설치`와 단순히 홈 화면에 바로가기 아이콘을 추가하는 것이 서로 다른 개념이므로 참고해주세요.

<br>

#### ★ 예시 : iOS용 Safari

iOS용 Safari에서는 브라우저의 `공유하기` > `홈 화면에 추가` 버튼을 클릭하여 설치할 수 있습니다.

<br>

<img src="./../img/how-to-install-pwa.png" width="375" />

<br>

## 3. 설치 유도하기: `beforeinstallprompt` 이벤트, `prompt()` 메소드로 설치 Prompt 띄우기, 설치여부 감지, UX 가이드라인

Safari에서 지원하지 않습니다.

<br>

### 3-1. `beforeinstallprompt` 이벤트

대부분의 브라우저는 웹사이트가 PWA로 식별되는 경우 앱 설치를 유도하는 알림을 자동으로 띄워줍니다. 앱 설치를 유도하는 Prompt UI를 커스텀하고 싶다면 `beforeinstallprompt` 이벤트가 발생했을 때 `preventDefault()` 메소드를 호출하여 브라우저가 자체적으로 띄우는 알림을 중단시킵니다.

<br>

```javascript
let deferredPrompt;

window.addEventListener("beforeinstallprompt", (e) => {
	e.preventDefault();

	// 이벤트를 나중에 핸들링하기 위해 저장
	deferredPrompt = e;

	// 커스텀 Prompt 제공
	showInstallPromotion();
});
```

<br>

### 3-2. `prompt()` 메소드로 설치 Prompt 띄우기

`prompt()` 메소드를 호출하면 사용쟈에게 PWA 앱을 설치할지 물어보는 브라우저의 Prompt 모달이 나타납니다. `prompt()` 메소드는 한 번만 호출할 수 있기 때문에, 사용자가 앱 설치를 거부한다면 다시 Prompt 모달을 띄울 수 없습니다. `prompt()` 메소드를 다시 호출하려면 `beforeinstallprompt` 이벤트가 다시 발생하기까지 기다려야합니다. 보통 그 시점은 `userChoice` 속성이 사용자의 응답을 `resolve`하는 `Promise` 객체를 반환할 때이죠.

```javascript
installBtn.addEventListener("click", (e) => {
	// 커스텀 Prompt 숨기기
	hideMyInstallPromotion();

	// 브라우저의 설치 Prompt 보여주기 (위에서 저장해두었던 beforeinstallprompt 이벤트 객체를 사용)
	deferredPrompt.prompt();

	// 설치 Prompt에 대한 사용자 응답에 따라 다음 작업을 수행
	deferredPrompt.userChoice.then((result) => {
		if (result.outcome === "accepted") {
			// 설치
		} else {
			// 미설치
		}
	});
});
```

<br>

### 3-3. 설치여부 감지

`beforeinstallprompt` 이벤트 객체의 `userChoice` 속성을 사용하여 사용자가 Prompt 모달에서 "설치하기"를 선택했는지 감지할 수 있지만, 완벽하지 않을 수 있습니다. 주소창과 같은 브라우저 내장 알림을 통해 PWA를 설치한다면 `userChoice` 속성을 사용할 수 없을 수 있기 떄문입니다. 

<br>

참고로 앱이 설치되었는지를 완벽하게 판단하려면 `appinstalled` 이벤트를 사용하면 되었지만, 이 이벤트는 현재 Deprecated 상태이므로 브라우저들의 동향을 살펴봐야 합니다.

<br>

### 3-4. UX 가이드라인

[Patterns for promoting PWA installation](https://web.dev/promote-install/)을 참고하는 것도 좋습니다.

- 웹사이트의 UX 흐름에 방해가 돼서는 안됩니다. 가령, 로그인 페이지라면 PWA 설치를 유도하는 UI는 반드시 아이디/비밀번호 입력 필드와 제출 버튼 아래에 위치해야합니다.

- PWA 설치를 유도하는 UI는 사용자가 원할 때 제거할 수 있어야합니다.

- PWA 설치를 유도하는 UI는 `beforeinstallprompt` 이벤트가 발생한 후에 보여주세요.

<br>

## 4. Manifest: 자동생성 툴, Manifest 파일, Manifest 항목

### 4-1. 자동생성 툴

Manifest Generator를 사용하거나, Manifest 레퍼런스 프로젝트를 참고하여 Manifest를 구성하면 편리합니다.

- [Firebase Web App Manifest Generator](https://app-manifest.firebaseapp.com/)
- [Awesome Meta Tags & Manifest Properties](https://github.com/gokulkrishh/awesome-meta-and-manifest)
- [pwacompat](https://github.com/GoogleChromeLabs/pwacompat)

<br>

### 4-2. Manifest 파일

Manifest 파일은 사용자의 브라우저에 PWA에 대한 정보를 알려주는 역할을 합니다. PWA 설정 파일이라고 보면 됩니다. 예를 들어, 아래와 같이 `<head>` 태그 내에 `manifest.webmanifest` 파일을 포함시키면 브라우저는 `manifest.webmanifest` 파일을 PWA 설정 파일로 인식하고 정보를 전달받습니다. 파일명은 `filename.webmanifest` 포맷으로 자유롭게 정하거나, `manifest.json`로 정합니다. [`credentials`](https://developer.mozilla.org/ko/docs/Web/API/Request/credentials)가 필요하다면 아래 태그에 [`crossorigin="use-credentials"` 속성을 추가](https://developer.mozilla.org/ko/docs/Web/HTML/Attributes/crossorigin)하세요.

<br>

```html
<head>
	<!-- .. -->
	<link rel="manifest" href="/manifest.webmanifest" />
</head>
```

<br>

Manifest 파일은 Chrome, Edge, Firefox, UC Browser, Opera, Samsung Internet 등의 브라우저에서 지원하고, iOS용 Safari에서는 상당부분 제한됩니다. 자세한 호환범위는 [Web app manifests - Browser compatibility](https://developer.mozilla.org/en-US/docs/Web/Manifest#browser_compatibility)에서 확인하시고요, iOS용 Safari의 경우 제한된 부분 중 일부는 Apple만의 방식으로 별도로 지원하기 때문에 [iOS에서의 PWA](./#user-content-12-ios에서의-pwa) 섹션을 확인하세요.

<br>

다음은 제가 구성하여 사용했던 Manifest 파일입니다. 예시로 봐주세요.

```json
{
  "lang": "ko",
  "name": "Estelle's",
  "short_name": "Estelle's",
  "description": "Estelle's",
  "icons": [
    {
      "src": "img/android/mipmap-mdpi/app-icon.png",
      "sizes": "48x48",
      "type": "image/png",
      "density": 1
    },
    {
      "src": "img/android/mipmap-hdpi/app-icon.png",
      "sizes": "72x72",
      "type": "image/png",
      "density": 1.5
    },
    {
      "src": "img/android/mipmap-xhdpi/app-icon.png",
      "sizes": "96x96",
      "type": "image/png",
      "density": 2
    },
    {
      "src": "img/android/mipmap-xxhdpi/app-icon.png",
      "sizes": "144x144",
      "type": "image/png",
      "density": 3
    },
    {
      "src": "img/android/mipmap-xxxhdpi/app-icon.png",
      "sizes": "192x192",
      "type": "image/png",
      "density": 4
    },
    {
      "src": "img/android/app-icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "scope": "/",
  "start_url": "/",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#ffffff",
  "theme_color": "#ffffff",
  "shortcuts": [
    {
      "name": "Works",
      "url": "/projects",
      "description": "Estelle이 수행한 모든 프로젝트를 한 곳에서 둘러보세요"
    },
    {
      "name": "About",
      "url": "/about",
      "description": "Estelle에 대해 알아보세요."
    }
  ]
}
```

<br>

### 4-3. Manifest 항목

#### `name` / `short_name`

- `name` : 웹앱의 이름, 설치될 때 사용됩니다.
- `short_name` : `name`보다 우선적으로 사용됩니다. 앱이 홈 화면에 추가되었을 때 아이콘 이미지와 함께 노출되는 이름이죠.

<br>

#### `icons`

아이콘 정보를 담는 배열입니다. 아래와 같은 하위 속성들을 갖고요, `src`, `sizes`, `type` 속성은 반드시 포함해야 합니다. OS별 아이콘 사이즈 규격은 [아이콘 규격](./#user-content-4-아이콘-규격) 섹션을 확인하시고요, iOS용 Safari에서 지원하지 않습니다.

- `src` : 이미지 경로
- `sizes` : 이미지가 적용될 디바이스 사이즈 (`px`)
- `type` : 이미지 타입

<br>

#### \* Android Maskable Icons

Android에서 [Maskable Icon](https://web.dev/maskable-icon/)을 사용하려면 해당 아이콘 정보에 `purpose` 속성을 추가하고, 값은 `maskable` 또는 `any maskable`로 지정하세요.

```json
{
	"icons": [
		{
			"src": "/images/icons-192.png",
			"type": "image/png",
			"sizes": "192x192",
			"purpose": "any maskable"
		}
	]
}
```

<br>

다음은 [Adaptive icon support in PWAs with maskable icons | web.dev](https://web.dev/maskable-icon/)에서 `any maskable` 대신 `maskable` 지정을 권고하는 내용을 발췌한 부분입니다. 참고해보면 좋을 것 같습니다.

> While you can specify multiple space-separated purposes like `any maskable`, in practice you shouldn't. Using `maskable` icons as `any` icons is suboptimal as the icon is going to be used as-is, resulting in excess padding and making the core icon content smaller. Ideally, icons for the `any` purpose should have transparent regions and no extra padding, like your site's favicons, since the browser isn't going to add that for them.

<br>

#### `start_url`

앱이 실행될 때 앱을 시작할 페이지 URL입니다.

<br>

#### `scope`

PWA에 속하는 페이지들의 URL 범위를 지정합니다. 지장한 URL 범위 밖의 페이지들은 PWA에 속하지 않는 것으로 간주됩니다.

- `scope`을 지정하지 않으면, `manifest.json` 파일이 위치한 디렉토리가 `scope`의 값으로 사용됩니다.
- `scope`의 값은 상대경로/절대경로 모두 가능합니다.
- `start_url`은 당연히 `scope`에서 지정한 URL 범위 내에 있어야합니다.
- `start_url`은 `scope`에 지정된 경로를 기준으로 하는 상대경로입니다.
- `start_url`이 `/`으로 시작되면 루트경로로 인식됩니다.

<br>

★ 주의 : PWA에서 `<a>` 태그를 사용하면 무조건 PWA 내에서 이동하고 실행됩니다.
이동하는 URL이 `scope`에서 지정한 범위를 벗어나도 말이죠.
PWA를 벗어나서 외부 사이트로 이동하도록 하려면 `<a>` 태그에 `target="_blank"` 속성값을 지정해야합니다.
브라우저의 새 탭을 열어 해당 URL로 이동시킬 때 주로 사용하는 속성이죠.

<br>

#### `background_color`

모바일에서 처음 앱을 실행시켰을 때 보여줄 스플래시 화면(Splash screen)의 배경색을 지정합니다. iOS용 Safari에서 지원하지 않습니다.

<br>

#### `display`

앱이 실행되었을 때 보여질 브라우저의 UI 형태를 지정합니다. 아래의 값 중에서 지정할 수 있고요, 지정된 값을 지원하지 않는 디바이스에서는 한 단계씩 Fallback 합니다. 여기서 지정된 값은 CSS에서 [`@media (display-mode: fullscreen)`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/display-mode) [Media Feature](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#media_features)를 사용하여 감지할 수 있습니다. 디스플레이 모드에 따라 스타일링을 다르게 할 수 있습니다.

- `fullscreen` : 브라우저의 주소창을 포함한 모든 UI를 완전히 제거하고 디바이스의 뷰포트(Viewport) 전체를 사용
- `standalone` : 브라우저의 주소창을 제거하여 독립적인 앱처럼 보이도록 함, 데스크탑과 노트북에서는 앱이 실행되는 독립된 OS 창을 사용
- `minimal-ui` : `standalone` 디스플레이와 비슷하지만, 브라우저의 뒤로가기/새로고침 등 일부 UI를 제공
- `browser` : 일반적인 브라우저 UI를 모두 사용

<br>

iOS용 Safari에서는 다음 2 개의 옵션만 지원합니다.

- `standalone`
- `browser`

<br>

#### `orientation`

앱의 가로세로 모드의 디폴트 상태를 지정합니다. 자세한 내용은 [MDN `orientation`](https://developer.mozilla.org/en-US/docs/Web/Manifest/orientation) 문서를 참고하시고요, 이 속성으로 지정하는 디폴트 값은 말그대로 디폴트 값이기 때문에 [Screen Orientation API](https://developer.mozilla.org/en-US/docs/Web/API/Screen/orientation)에 의해 조작될 수 있습니다. 

<br>

iOS용 Safari에서 지원하지 않습니다. 따라서 앱의 런치 화면 이미지는 사이즈당 가로, 세로 두 가지 모드를 모두 제공해야합니다. iOS용 Safari 런치 화면 관련 내용은 이 문서의 [앱 런치 화면(Launch Screen)](./user-content-12-3-앱-런치-화면launch-screen) 섹션을 참고하세요.

<br>

#### `theme_color`

앱의 테마 색을 지정합니다. [`<head>`의 `<meta>` 태그로 지정한 색](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name/theme-color)과 일치해야 하고요, iOS용 Safari에서 지원하지 않습니다.

<br>

#### `shortcuts`

PWA의 [쇼트컷(Shortcut)](https://web.dev/app-shortcuts/) 페이지들을 지정합니다. Android에서는 앱 아이콘을 길게 눌러서 아래와 같이 특정 페이지들에 빠르고 편하게 접근할 수 있습니다. 대표적으로 Android용 Chrome과 Samsung Internet에서 지원하고요, Safari에서는 지원하지 않습니다.

![App Shortcut](./../img/app-shortcut.png)

<br>

아래와 같이 지정하면 되고요, 자세한 내용은 [MDN `shortcuts`](https://developer.mozilla.org/en-US/docs/Web/Manifest/shortcuts) 문서를 참고하세요.

```json
 "shortcuts": [
    {
      "name": "Open Play Later",
      "short_name": "Play Later",
      "description": "View the list of podcasts you saved for later",
      "url": "/play-later?utm_source=homescreen",
      "icons": [{ "src": "/icons/play-later.png", "sizes": "192x192" }]
    },
    {
      "name": "View Subscriptions",
      "short_name": "Subscriptions",
      "description": "View the list of podcasts you listen to",
      "url": "/subscriptions?utm_source=homescreen",
      "icons": [{ "src": "/icons/subscriptions.png", "sizes": "192x192" }]
    }
  ]
```

<br>

#### 기타 항목

더 많은 항목들을 지정하려면 [MDN의 Web app manifests](https://developer.mozilla.org/en-US/docs/Web/Manifest) 문서를 참고하세요.

<br>

## 5. 아이콘 규격: `png` 포맷, OS별 사이즈

### 5-1. `png` 포맷, OS별 사이즈

현시점에서 PWA의 앱 아이콘은 모두 `png` 포맷으로 제공하면 됩니다. 홈화면, 스플래시 화면 등에서 보여질 아이콘의 사이즈 규격은 OS마다 다르기 때문에 각 OS의 앱 아이콘 가이드 문서를 확인해야합니다. 또한, 대부분 OS에서 아이콘은 테두리가 둥글게 잘리기 때문에 잘려질 부분과 여백을 고려하여 아이콘을 제작해야합니다. [App Icon Generator](https://appicon.co/)와 같은 아이콘 생성 툴을 사용하면 편리합니다.

- iOS : [iOS App Icon | Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/#app-icon-sizes)
- MacOS : [MacOS App Icon | Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/macos/icons-and-images/app-icon/#app-icon-sizes)
- Android : [Google Play icon design specifications](https://developer.android.com/google-play/resources/icon-design-specifications)
- Windows : [App icons and logos | Windows Developer](https://docs.microsoft.com/en-us/windows/apps/design/style/app-icons-and-logos)

<br>

다음은 빠르게 Cheat 할 수 있도록 iOS, MacOS, Android를 커버하는 모든 규격을 목록화했습니다. 현시점 2021.09.23 기준입니다. OS별 사이즈 규격에 대한 설명은 이어지는 `4-2` ~ `4-5` 섹션에서 다룹니다. 역시나 현시점 기준입니다.

- `16*16 px`
- `32*32 px`
- `64*64 px`
- `120*120 px`
- `128*128 px`
- `152*152 px`
- `167*167 px`
- `180*180 px`
- `192*192 px`
- `256*256 px`
- `512*512 px`
- `1024*1024 px`

<br>

### 5-2. iOS

현시점 기준 iOS 앱 아이콘의 사이즈 규격입니다. `@2x`는 표준해상도 대비 해상도가 2 배인 디바이스라는 의미입니다. 가령, iPhone의 경우 물리적으로는 `60*60 pt` 크기의 아이콘을 제공하면 되는데요, iOS의 표준해상도에서 `1pt = 1px` 이므로 `60*60 px` 크기의 아이콘을 제공하면 됩니다. `@2x` 디바이스에서는 가로, 세로 각각 픽셀 밀도가 2 배씩 높기 때문에 `120*120 px` 크기의 이미지 파일을 제공하면 되고요.

- `120*120 px @2x` : iPhone
- `152*152 px @2x` : iPad, iPad Mini
- `167*167 px @2x` : iPad Pro
- `180*180 px @3x` : iPhone
- `1024*1024 px @1x` : App Store

<br>

### 5-3. MacOS

현시점 기준 MacOS의 Finder, Dock, Launchpad 등에서 사용되는 앱 아이콘의 사이즈 규격입니다.

- `16*16 px @1x`
- `32*32 px @1x @2x`
- `64*64 px @2x`
- `128*128 px @1x`
- `256*256 px @1x @2x`
- `512*512 px @1x @2x`
- `1024*1024 px @2x`

<br>

### 5-4. Android

[Google Play icon design specifications](https://developer.android.com/google-play/resources/icon-design-specifications)에 따르면 `512*512 px` 사이즈의 아이콘만 제공하면 되지만, Android용 Chrome에서 모든 디바이스 뷰포트에 아이콘을 자동으로 핏되게 하려면 다음 2개 사이즈를 반드시 제공해야합니다.

- `192*192 px`
- `512*512 px`

<br>

만약 Android에서 최적화된 픽셀 경험을 제공하려면 Android에서 사용할 가능성이 있는 모든 크기의 아이콘 파일을 제공하면 됩니다. Android는 표준해상도에서 앱 아이콘의 크기를 `48px`로 봅니다. 따라서 `@1.5x`, `@2x`, `@3x`, `@4x` 해상도에서 필요한 각 사이즈의 아이콘 파일을 제공합니다. 이는 Android가 앱 아이콘의 사이즈로 `48dp`를 사용하기 때문입니다. 자세한 내용은 [다양한 픽셀 밀도 지원](https://developer.android.com/training/multiscreen/screendensities) 문서를 확인하세요.

- `48px`
- `72px`
- `96px`
- `144px`
- `192px`

<br>

#### ★ Android Maskable Icon

Maskable 아이콘은 Android 앱 규격에 딱 맞추어 둥근 형태로 잘린 아이콘을 말합니다. Maskable 아이콘을 제공하면 Android 앱에 기본으로 적용되는 흰 색 배경 대신 아이콘 영역을 원하는 이미지로 모두 채울 수 있습니다. [Maskable.app Editor](https://maskable.app/editor)를 사용하여 간편하게 Maskable 아이콘 이미지를 생성할 수 있고요, `webmanifest`에서 Maskable 아이콘의 경우 `"purpose": "maskable"` 항목을 명시합니다.

<br>

Chrome 개발자도구의 Application > Manifest 탭에서 아이콘이 어떻게 보이는지 미리 확인할 수 있고요, [Maskable.app](https://maskable.app/)을 사용해서 검사해볼 수도 있습니다. 자세한 내용은 [Maskable Icon](https://web.dev/maskable-icon/)과 [Maskable Icons: Android Adaptive Icons for Your PWA](https://css-tricks.com/maskable-icons-android-adaptive-icons-for-your-pwa/)를 참고하세요.

<br>

### 5-5. Windows

가짓수가 많아 생략합니다. 공식문서의 [Target-size app icon assets](https://docs.microsoft.com/en-us/windows/apps/design/style/app-icons-and-logos#target-size-app-icon-assets) 섹션을 참고하세요.

<br>

## 6. Service Worker API: Service Worker란, Service Worker 등록하기

### 6-1. Service Worker란

[Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)의 역할 중 하나는 웹앱에서 외부로 요청(Request)을 보낼 때 인터셉터(Interceptor)로서 작동하는 것입니다. 요청이 보내지는 시점에 끼어들어 특정한 일을 처리할 수 있습니다. Service Worker API는 대부분의 브라우저에 내장되어 있고요, 이 Service Worker를 사용하여 PWA 기능들을 구현할 수 있습니다.

<br>

### 6-2. Service Worker 등록하기

Service Worker API는 `window.navigator` 객체 내의 `serviceWorker`라는 이름의 객체로 제공됩니다. Service Worker API를 제공하는 브라우저라면, 처음 앱이 로드될 때 아래와 같이 `service-worker.js` 파일을 앱의 Service Worker로 등록시킵니다. 이제 `service-worker.js` 파일에 작성된 로직이 앱의 인터셉터로서 작동하게 됩니다.

```javascript
window.addEventListener("load", () => {
	if ("serviceWorker" in window.navigator) {
		window.navigator.serviceWorker.register("/service-worker.js");
	}
});
```

<br>

## 7. 오프라인 Fallback 페이지 제공하기: Service Worker, Cache Storage

[App Shell](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure#app_shell)이라는 개념이 있습니다. SSR(Server-side rendering)과 CSR(Client-side rendering)을 믹스한 개념으로, 사용자가 앱에 재방문했을 때 캐시에 미리 저장해놓은 페이지를 즉시 로드하여 보여주기 때문에 인터넷이 없는 환경에서도 앱을 사용할 수 있습니다. 사용자의 네트워크 환경이 불안정할 때, 아무것도 보여주지 않는 대신 사용자가 어떻게 대처하면 될지 안내문구가 포함된 페이지를 보여주는 UX를 제공할 수 있죠. 네트워크 연결이 없을 때 보여줄 `offline.html` 파일을 [`CacheStorage`](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage) API를 사용하여 미리 캐싱해놓고, 필요할 때 보여주는 방식입니다.

<br>

새로 업데이트된 부분만 서버에 요청하여 받아오기 때문에 전체 페이지를 로딩하는 것보다 빠르고 부드러운 UX를 제공할 수 있는 것은 덤입니다. 무엇을 캐시에서 받아오고, 무엇을 서버에 새로 요청할지는 [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)를 사용하여 설정할 수 있습니다. 

<br>

아래는 구글에서 제공하는 `service-worker.js`의 예시 코드입니다. 오프라인 Fallback 페이지를 제공하는 코드이고요, 자세한 설명은 [Making PWAs work offline with Service workers](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Offline_Service_workers) 문서를 참고하세요.

```javascript
/*
Copyright 2015, 2019, 2020 Google LLC. All Rights Reserved.
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 http://www.apache.org/licenses/LICENSE-2.0
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
*/

// Incrementing OFFLINE_VERSION will kick off the install event and force
// previously cached resources to be updated from the network.
const OFFLINE_VERSION = 1;
const CACHE_NAME = "offline";
// Customize this with a different URL if needed.
const OFFLINE_URL = "offline.html";

self.addEventListener("install", (event) => {
	event.waitUntil(
		(async () => {
			const cache = await caches.open(CACHE_NAME);
			// Setting {cache: 'reload'} in the new request will ensure that the
			// response isn't fulfilled from the HTTP cache; i.e., it will be from
			// the network.
			await cache.add(new Request(OFFLINE_URL, { cache: "reload" }));
		})()
	);
	// Force the waiting service worker to become the active service worker.
	self.skipWaiting();
});

self.addEventListener("activate", (event) => {
	event.waitUntil(
		(async () => {
			// Enable navigation preload if it's supported.
			// See https://developers.google.com/web/updates/2017/02/navigation-preload
			if ("navigationPreload" in self.registration) {
				await self.registration.navigationPreload.enable();
			}
		})()
	);

	// Tell the active service worker to take control of the page immediately.
	self.clients.claim();
});

self.addEventListener("fetch", (event) => {
	// We only want to call event.respondWith() if this is a navigation request
	// for an HTML page.
	if (event.request.mode === "navigate") {
		event.respondWith(
			(async () => {
				try {
					// First, try to use the navigation preload response if it's supported.
					const preloadResponse = await event.preloadResponse;
					if (preloadResponse) {
						return preloadResponse;
					}

					// Always try the network first.
					const networkResponse = await fetch(event.request);
					return networkResponse;
				} catch (error) {
					// catch is only triggered if an exception is thrown, which is likely
					// due to a network error.
					// If fetch() returns a valid HTTP response with a response code in
					// the 4xx or 5xx range, the catch() will NOT be called.
					console.log("Fetch failed; returning offline page instead.", error);

					const cache = await caches.open(CACHE_NAME);
					const cachedResponse = await cache.match(OFFLINE_URL);
					return cachedResponse;
				}
			})()
		);
	}

	// If our if() condition is false, then this fetch handler won't intercept the
	// request. If there are any other fetch handlers registered, they will get a
	// chance to call event.respondWith(). If no fetch handlers call
	// event.respondWith(), the request will be handled by the browser as if there
	// were no service worker involvement.
});
```

<br>

오프라인 환경에서 `offline.html`이 제대로 작동하려면 필요한 모든 리소스들도 미리 캐싱되어야 합니다. 가장 간단한 방법은 오프라인 페이지에 필요한 CSS와 JavaScript 코드를 `offline.html` 파일에 직접 포함시키는 것입니다. 따로 불러올 필요가 없도록 말이죠.

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Offline</title>

		<!-- 인라인 스타일 -->
		<style>
			body {
				font-family: helvetica, arial, sans-serif;
				margin: 2em;
			}

			h1 {
				font-style: italic;
				color: #373fff;
			}

			p {
				margin-block: 1rem;
			}

			button {
				display: block;
			}
		</style>
	</head>
	<body>
		<h1>네트워크 연결이 불안정합니다.</h1>

		<p>페이지를 새로고침하려면 아래 버튼을 클릭하세요.</p>
		<button type="button">⤾</button>

		<!-- 인라인 JavaScript -->
		<script>
			document.querySelector("button").addEventListener("click", () => {
				window.location.reload();
			});
		</script>
	</body>
</html>
```

<br>

## 8. 브라우저에서 알림 전송하기: Notification API, 권한 핸들링, 알림 전송, 알림 닫기, 이벤트 핸들링

웹앱에서 알림을 전송하려면 [Notification API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API) 또는 [Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)를 사용합니다. [Codelab: Build a push notification client](https://web.dev/push-notifications-client-codelab/)에서 예제를 확인할 수 있고요, 두 API의 차이점이 헷갈린다면 StackOverflow [Difference between Notifications API and Push API from Web perspective](https://stackoverflow.com/questions/34844561/difference-between-notifications-api-and-push-api-from-web-perspective) 페이지를 확인해보세요. 이 섹션에서는 Notification API를 다룹니다.

<br>

★ 주의 : 알림 관련 API들은 브라우저 호환성이 떨어집니다. 대표적으로 iOS에서는 모든 브라우저에서 지원하지 않습니다.

<br>

### 8-1. Notification API

[Notification API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API)를 사용하여 사용자에게 알림을 전송할 수 있습니다. 브라우저에서 전송하는 알림이 OS의 시스템 알림 UI를 통해 표시됩니다. HTTPS에서만 작동하고, Service Worker를 통해 사용할 수 있습니다. 브라우저가 아닌 서버에서 전송하는 알림을 수신하게 하려면 Push API를 사용해야 합니다.

<br>

### 8-2. 권한 핸들링

읽기전용 속성인 `Notification.permission`을 사용하여 현재 권한 상태를 확인합니다.

```javascript
if (!("Notification" in window)) return;

console.log(Notification.permission); // 'granted'
```

<br>

`Notification.permission`의 값은 다음 세 가지 중 하나입니다.

- `default` : 사용자에게 아직 권한을 요구하지 않았으며 따라서 알림을 표시하지 않습니다.
- `granted` : 사용자에게 알림 표시 권한을 요구했으며 사용자는 권한을 허용했습니다.
- `denied` : 사용자가 명시적으로 알림 표시 권한을 거부했습니다.

<br>

만약 권한 상태가 `granted`가 아니라면, 사용자가 권한을 직접 허용해줘야 합니다. `Notification.requestPermission()` 메소드를 사용하여 사용자에게 권한 허용을 요청합니다.

<br>

```javascript
if (Notification.permission === "default") {
	Notification.requestPermission().then((result) => {
		if (result === "granted") {
			// ..
		}
	});
}
```

<br>

ES5 이하에서는 `Promise`를 사용할 수 없으므로 콜백을 사용합니다.

```javascript
// Promise 지원여부 확인
function supportNotificationPromise() {
	try {
		Notification.requestPermission().then();
	} catch (e) {
		return false;
	}
	return true;
}

if (!supportNotificationPromise) {
	Notification.requestPermission(callback);
}
```

<br>

### 8-3. 알림 전송

알림은 `Notification()` 생성자를 호출할 때 전송됩니다. 생성자를 호출할 때 인자를 통해 옵션을 지정할 수 있습니다. [MDN `Notification()`](https://developer.mozilla.org/en-US/docs/Web/API/Notification/Notification) 문서에서 모든 가능한 옵션을 확인하세요.

```javascript
const notification = new Notification("제목", {
	lang: "ko",
	body: "내용",
	icon: "/icon.png",
	vibrate: 200
});
```

<br>

### 8-4. 알림 닫기

대부분의 브라우저에서는 알림을 약 4초 후에 자동으로 닫습니다. 하지만 오래된 브라우저에서는 `Notification.close()` 메소드를 사용하여 코드를 통해 닫아야합니다. 여기서 주의할 점은 `close()` 메소드를 사용하면 알림 내역이 알림 트레이(Notification Tray)에서도 완전히 제거될 수 있다는 것입니다. 따라서 `setTimeout`을 사용하여 일괄적으로 제거하는 것은 적절하지 않습니다. 대신, 아래와 같이 [`document.visibilityState`](https://developer.mozilla.org/ko/docs/Web/API/Document/visibilityState) 값을 사용하여 사용자가 앱을 열었는지 판단하는 것이 좋습니다.

```javascript
document.addEventListener("visibilitychange", function() {
  if (document.visibilityState === "visible") {
    notification.close();
  }
});
```

<br>

참고로, 웹팩 사용이 불가능하다면 [`bind()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)를 사용하여 열려있는 `Notificaiton` 객체를 연동시켜야합니다.

<br>

### 8-5. 이벤트 핸들링

`Notification` 인스턴스에는 다음 네 가지 이벤트가 발생할 수 있습니다.

- `click` : 사용자가 알림을 클릭
- `close` : 알림이 닫힘
- `error` : 알림에 오류 발생
- `show` : 알림이 사용자에게 표출됨

<br>

## 9. 서버에서 알림 전송하기: Push API, 구독상태 확인, 애플리케이션 서버 키, `push` 이벤트

### 9-1. Push API

[Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)는 웹앱이 현재 로딩되어있지 않더라도 서버로부터 메시지를 받을 수 있도록 하는 API입니다. [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)가 등록되어야 사용할 수 있고요, [Adding Push Notifications to a Web App](https://developers.google.com/web/fundamentals/codelabs/push-notifications/) 문서에서 예제를 확인할 수 있습니다.

<br>

### 9-2. 구독상태 확인

먼저 등록된 Service Worker 객체를 저장하시고요.

```javascript
// register-service-worker.js

const swRegistration = register()

async function register() {
	try {
		if ("serviceWorker" in navigator) {
			return await navigator.serviceWorker.register("service-worker.js")
		} else {
			throw new "Service Worker is not supported";
		}
	} catch(e) {
		// ..
		return null
	}
}
```

<br>

`PushManager`를 지원하는 브라우저라면 등록된 Service Worker 객체 내의 `pushManager.getSubscription()` 메소드를 사용하여 사용자가 푸시알림을 구독중인지 확인할 수 있습니다. 

```javascript
// register-service-worker.js

getPushSubscription();

async function getPushSubscription() {
	try {
		if (swRegistration === null) {
			throw new "Service Worker is not registered";
		}

		if ("PushManager" in window)  {
			const subscription = await swRegistration.pushManager.getSubscription()
			const isSubscribed = !(subscription === null)
			// ..
		} else {
			throw new "PushManager is not supported";
		}
	} catch(e) {
		// ..
	}
}
```

<br>

### 9-3. 애플리케이션 서버 키

사용자가 현재 구독하지 않은 상태라면 `pushManager.subscribe()` 메소드를 호출하여 구독 프로세스를 시작합니다. 구독 프로세스라는 것은 다음 두 단계를 말합니다. 이 두 단계가 성공적으로 완료되면 `Promise`를 반환합니다.

- 사용자에게 Notification 권한 허용 Prompt를 띄우고, 사용자가 허용함
- 브라우저가 세부정보를 얻기 위해 푸시 알림을 보낼 서버로 네트워크 요청을 보냄

<br>

`subscribe()` 메소드를 호출할 때 구독할 웹서버의 [애플리케이션 서버 키](https://developers.google.com/web/fundamentals/push-notifications/web-push-protocol)를 제공해야하는데, 이 키는 [`UInt8Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) 타입으로 변환하여 제공합니다. `userVisibleOnly` 속성의 값은 `true`로 지정해야 알림이 표시됩니다.

```javascript
// register-service-worker.js

subscribePush();

async function subscribePush(appServerPublicKey) {
  try {
	// ..

    const applicationServerKey = Uint8Array.from(appServerPublicKey);

    const subscription = await swRegistration.pushManager.subscribe({
      userVisibleOnly: true, // 푸시가 전송될 때마다 알림을 표시하도록 허용
      applicationServerKey
    });
	// ..
  } catch(e) {
    // ..
  }
}
```

<br>

사용자가 권한을 거부하면 구독 프로세스는 실패하고, 알림을 전송할 수도 없습니다. 사용자의 권한 허용 상태는 `Notification.permisison` 값으로 알 수 있습니다. 값이 `denied`라면 사용자가 권한을 거부한 것이므로, 더이상 할 수 있는 것이 없습니다.

```javascript
if (Notification.permisison === 'denied') {
	// ..
}
```

<br>

### 9-4. `push` 이벤트

서버에서 푸시알림을 수신할 때 발생하는 `push` 이벤트를 사용하여 최종적으로 알림을 표시합니다. `push` 이벤트는 Service Worker 내에서 핸들링하면 되는데요, Service Worker 파일에서 `self`는 Service Worker 자체를 참조합니다.

```javascript
// service-worker.js

self.addEventListener("push", function(evt) {
	evt.waitUntil(self.registration.showNotification("제목", {
		body: "내용",
		icon: "img/icon.png",
	}));
})
```

<br>

## 10. 접속모드(브라우저/PWA)에 따라 다르게 스타일링하기

아래와 같이 CSS 미디어쿼리를 이용하여 웹사이트의 접속모드에 따라 다르게 스타일링할 수 있습니다. (브라우저를 통해 접속했는지, 홈화면을 통해 접속했는지에 따라서 말이죠) `display-mode` 값에 따라 아래와 같이 CSS를 작성하는거죠.

```css
@media all and (display-mode: standalone) {
	body {
		background-color: yellow;
	}
}
```

<br>

JavaScript 코드에서도 웹사이트의 접속모드를 검사할 수 있습니다.

```javascript
window.addEventListener("DOMContentLoaded", () => {
	// 기본
	let displayMode = "browser tab";

	// iOS
	if (navigator.standalone) displayMode = "standalone-ios";

	// Chrome
	if (window.matchMedia("(display-mode: standalone)").matches)
		displayMode = "standalone";
});
```

<br>

아래와 같이 접속모드가 변경되었는지도 확인할 수 있고요.

```javascript
window.addEventListener("DOMContentLoaded", () => {
	window.matchMedia("(display-mode: standalone)").addListener((evt) => {
		let displayMode = "browser tab";

		if (evt.matches) displayMode = "standalone";
	});
});
```

<br>

## 11. PWA와 네이티브 앱

만약 별도로 네이티브 앱을 제공한다면, 네이티브 앱이 사용자의 디바이스에 설치되었는지 확인할 수 있습니다. 네이티브 앱 설치여부에 따라 PWA 설치를 유도하거나, 유도하지 않을 수 있죠. 아래 문서를 참고하세요.

- [Is your app installed? getInstalledRelatedApps() will tell you!](https://web.dev/get-installed-related-apps/)

<br>

PWA와 네이티브 앱을 적절하게 블렌딩 하여 사용자들에게 심리스한 경험을 제공할 수 있습니다. 아래 영상에 설명이 있습니다.

- [Blending PWA into native environments (Chrome Dev Summit 2019)](https://www.youtube.com/watch?v=V7YX4cZ_Cto&feature=youtu.be)

<br>

## 12. iOS에서의 PWA

### 12-1. `webmanifest` 제한 - `link`, `meta` 태그로 대체

iOS용 Safari에서는 `webmanifest` 파일의 속성 중 상당수가 제한되어있습니다. [Web app manifests - Browser compatibility](https://developer.mozilla.org/en-US/docs/Web/Manifest#browser_compatibility)에서 어떤 속성들이 제한되었는지 확인할 수 있고요, 다만 제한된 속성 중 일부는 아래의 태그들을 `html` 파일의 `<head>` 태그 내에 추가함으로써 PWA를 적용할 수 있습니다. 아래에서 하나씩 설명합니다.

- `<link rel="apple-touch-icon" href="touch-icon-iphone.png" />`
- `<link rel="apple-touch-startup-image" href="/launch.png" />`
- `<meta name="apple-mobile-web-app-title" content="AppTitle" />`
- `<meta name="apple-mobile-web-app-capable" content="yes" />`
- `<meta name="apple-mobile-web-app-status-bar-style" content="black" />`

<br>

### 12-2. 아이콘

아이콘은 `<link>` 태그를 사용하여 지정합니다. 모든 디바이스를 지원하려면 필요한 규격을 확인하여 모두 제공하면 됩니다. 만약 `<link>` 태그를 사용하여 아이콘을 지정하지 않으면, 웹사이트의 루트 경로에서 파일명에 `apple-touch-icon` Prefix가 포함된 이미지 파일을 찾아서 아이콘으로 사용합니다. 만약 `50 * 50 px` 사이즈의 디바이스라면 다음의 우선순위대로 파일명과 포맷을 탐색하여 아이콘으로 사용합니다. 1) `apple-touch-icon-80x80.png` 2) `apple-touch-icon.png`.

```html
<link rel="apple-touch-icon" href="touch-icon-iphone.png" />
<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-ipad.png" />
<link
	rel="apple-touch-icon"
	sizes="180x180"
	href="touch-icon-iphone-retina.png"
/>
<link
	rel="apple-touch-icon"
	sizes="167x167"
	href="touch-icon-ipad-retina.png"
/>
```


<br>

### 12-3. 앱 런치 화면(Launch Screen)

앱아이콘과 마찬가지로 디바이스별 런치 화면 사이즈를 확인한 후 규격에 맞는 파일을 모두 제공하면 됩니다. 런치 화면용 이미지 파일은 [About splash-screens](https://appsco.pe/developer/splash-screens)와 같은 툴을 사용하여 빠르게 생성할 수 있습니다. 참고로 iOS 14 이후로 런치 화면 이미지 파일 용량을 `25MB`로 제한합니다. 다음은 StackOverflow [iOS PWA splash screen?](https://stackoverflow.com/questions/55840186/ios-pwa-splash-screen)에서 가져온 구성 예시입니다.

```html
<!-- Fallbacks for old devices -->
<link rel="apple-touch-startup-image" media="screen and (device-width: 320px) and (device-height: 568px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/1.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/2.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/3.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/4.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/5.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/6.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/7.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/8.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/9.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/10.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/11.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/12.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/13.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/14.png">
<link rel="apple-touch-startup-image" media="screen and (width: 834px) and (height: 1194px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/15.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/16.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 320px) and (device-height: 568px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/17.png">
<link rel="apple-touch-startup-image" media="screen and (width: 834px) and (height: 1194px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/18.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/19.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/20.png">

<!-- 2020 Start -->
<link rel="apple-touch-startup-image" media="screen and (device-width: 810px) and (device-height: 1080px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/21.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 810px) and (device-height: 1080px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/22.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 820px) and (device-height: 1180px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)" href="/pages/splash-screen/images/23.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 820px) and (device-height: 1180px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)" href="/pages/splash-screen/images/24.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 428px) and (device-height: 926px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/25.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 428px) and (device-height: 926px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/26.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 360px) and (device-height: 780px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/27.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 360px) and (device-height: 780px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/28.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)" href="/pages/splash-screen/images/29.png">
<link rel="apple-touch-startup-image" media="screen and (device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)" href="/pages/splash-screen/images/30.png">
```

<br>

### 12-4. 앱 이름

별도로 지정하지 않으면 `<title>` 태그에 지정한 이름을 그대로 사용합니다.

```html
<meta name="apple-mobile-web-app-title" content="AppTitle" />
```

<br>

### 12-5. `standalone` 모드 (브라우저 UI 제거하기)

아래와 같이 `<meta>` 태그를 지정하면 PWA가 `standalone` 모드로 전환되고요, 주소창과 하단 컨트롤러 등 모든 브라우저 UI를 제거합니다.

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
```

<br>

### 12-6. 상태바 스타일링

PWA를 `standalone` 모드로 지정하면 상태바를 스타일링할 수 있습니다. 가령, 아래와 같이 값을 `black`으로 지정하면 상태바가 검정색으로 스타일링됩니다.

```html
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
```

<br>

가능한 값은 아래의 세 가지입니다. 별도로 지정하지 않으면 기본값은 `default`입니다.

- `default` : 회색

- `black` : 검정

- `black-translucent` : 상태바의 색이 없어지고(투명), 앱 콘텐츠 위로 쌓임

<br>

<br>

---

### References

- [Introduction to progressive web apps | MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction)
- [Progressive web app structure | MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)
- [Progressive Web Apps](https://web.dev/progressive-web-apps/)
- [What does it take to be installable?](https://web.dev/install-criteria/)
- [How to provide your own in-app install experience](https://web.dev/customize-install/)
- [Add a web app manifest](https://web.dev/add-manifest/)
- [Service workers and the Cache Storage API](https://web.dev/service-workers-cache-storage/)
- [PWA case studies](https://www.pwastats.com)
- [F.I.R.E.: An Introduction to Progressive Web Apps](https://www.punchkick.com/software/2019/01/29/what-are-progressive-web-apps)
- [PWA 코드랩 가이드라인](https://euncho.medium.com/pwa-%EC%BD%94%EB%93%9C%EB%9E%A9-%EA%B0%80%EC%9D%B4%EB%93%9C%EB%9D%BC%EC%9D%B8-597049b2df40)
- [PWA를 구성하는 기술들](https://euncho.medium.com/pwa%EB%A5%BC-%EA%B5%AC%EC%84%B1%ED%95%98%EB%8A%94-%EA%B8%B0%EC%88%A0%EB%93%A4-a5be57df5575)
- [Progressive Web Apps on iOS are here](https://medium.com/@firt/progressive-web-apps-on-ios-are-here-d00430dee3a7)
- [Don’t use iOS meta tags irresponsibly in your Progressive Web Apps](https://medium.com/@firt/dont-use-ios-web-app-meta-tag-irresponsibly-in-your-progressive-web-apps-85d70f4438cb)
- [Supported Meta Tags | Safari HTML Reference](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)
- [Configuring Web Applications | Safari HTML Reference](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
- [6 Tips to make your iOS PWA feel like a native app](https://www.netguru.com/codestories/pwa-ios)
- [Encouraging iOS users to install your Progressive Web Apps in Ember](https://dockyard.com/blog/2017/09/27/encouraging-pwa-installation-on-ios)
- [Streams API](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API)
