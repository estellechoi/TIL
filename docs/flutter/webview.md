# Flutter에서 웹뷰(Web View) 사용하기

<br>

1. 웹뷰 사용하기: `webview_flutter` 라이브러리
2. 웹뷰에서 HTTP 프로토콜 허용하기

<br>

## 1. 웹뷰 사용하기: `webview_flutter` 라이브러리

### 1-1. 웹뷰

웹뷰(Web View)란 Android나 iOS 앱에서 웹페이지를 임베드하는 방식으로 가져와 뷰(View)로 사용하는 기능입니다. 각 OS에 내장되어있는 웹브라우저를 통해 웹페이지를 렌더링할 수 있기 때문에 가능한 기능이죠. 네이티브 앱은 높은 성능과 부드러운 사용성이 강점이지만, 콘텐츠가 수정되어 배포될 때마다 사용자가 앱 업데이트를 해야한다는 점은 단점입니다. 개발 관점에서 OS 호환성도 단점이 될 수 있고요. 웹뷰를 사용하면 이러한 단점들을 해결할 수 있습니다. 다만, 네이티브 앱처럼 느껴질 정도로 웹뷰에서의 동작을 최적화할 필요가 있습니다.

<br>

### 1-2. `webview_flutter`

iOS 앱 프레임워크에서는 [`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview), Android 프레임워크에서는 [`WebView`](https://developer.android.com/reference/android/webkit/WebView) 클래스를 통해 웹뷰를 지원하는데요, Flutter에서 웹뷰를 사용하려면 [`webview_flutter`](https://pub.dev/packages/webview_flutter)와 같은 라이브러리를 사용하면 됩니다. 라이브러리 문서에 따르면, `webview_flutter`에서 제공하는 `WebView` 위젯을 사용하여 웹페이지를 임베드하는 코드를 작성한 후 앱을 빌드하면 iOS의 `WKWebView`, Android의 `WebView` 클래스를 사용하는 것과 같게 빌드됩니다.

<br>

> On iOS the WebView widget is backed by a WKWebView; On Android the WebView widget is backed by a WebView.

<br>

### 1-3. iOS 사전준비: `o.flutter.embedded_views_preview`

`ios/Runner/Info.plist` 파일에 다음과 같이 설정 내용을 추가합니다. 그렇지 않으면 흰 화면이 나옵니다.

```xml
<key>io.flutter.embedded_views_preview</key>
<string>YES</string>
```

<br>

### 1-4. `WebView` 위젯 사용법

이 라이브러리 사용법은 매우 간단합니다. `WebView` 위젯을 사용하고, 보여줄 웹페이지의 URL을 지정해줍니다. 그리고 `javascriptMode` 속성을 사용하여 JavaScript 사용을 허용해주어야하는데요, Android와 iOS의 웹뷰에서 기본적으로 JavaScript는 사용 중지되기 때문입니다. 하나 더 주의할 점은 웹뷰는 HTTPS 프로토콜만 지원한다는 것이고요, 테스트 등을 위해 특별히 HTTP 프로토콜을 지원해야한다면 별도의 설정이 필요합니다.

```dart
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

// ..

WebView(
    initialUrl: 'https://flutter.dev',
    javascriptMode: JavascriptMode.unrestricted,
)
```

<br>

- [`webview_flutter` 사용 예제](https://github.com/flutter/plugins/blob/master/packages/webview_flutter/webview_flutter/example/lib/main.dart)

<br>

## 2. 웹뷰에서 HTTP 프로토콜 허용하기

### 2-1. Android에서 HTTP 프로토콜 허용하기

`/android/app/src/main/AndroidManifest.xml` 파일에 다음 설정을 추가합니다.

```xml
<application
        android:name="io.flutter.app.FlutterApplication"
        android:label="flutterdemo"
        android:icon="@mipmap/ic_launcher"
        android:usesCleartextTraffic="true">
```

<br>

### 2-2. iOS에서 HTTP 프로토콜 허용하기

`/ios/Runner/Info.plist` 파일에 다음 설정을 추가합니다.

```plist
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
  <key>NSAllowsArbitraryLoadsInWebContent</key>
  <true/>
</dict>
```

<br>

---

### References

- [webview_flutter library](https://pub.dev/documentation/webview_flutter/latest/webview_flutter/webview_flutter-library.html)
- [WebView에서 웹 앱 빌드 | Android Developers](https://developer.android.com/guide/webapps/webview?hl=ko)
