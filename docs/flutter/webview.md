# Flutter에서 웹뷰(Web View) 사용하기

<br>

1. `webview_flutter` 라이브러리 사용하기
2. 웹뷰에서 HTTP 프로토콜 허용하기

<br>

## 1. `webview_flutter` 라이브러리 사용하기

Flutter에서 웹페이지를 보여주려면 [`webview_flutter`](https://pub.dev/packages/webview_flutter)와 같은 라이브러리를 사용합니다. `webview_flutter`에서 제공하는 `WebView` 위젯을 사용하면 되는데요, 이 위젯은 Flutter 앱을 빌드했을 때 iOS의 [`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview), Android의 [`WebView`](https://developer.android.com/reference/android/webkit/WebView) 위젯으로 각각 빌드됩니다.

<br>

### 1-1. iOS 준비사항

`ios/Runner/Info.plist` 파일에 다음과 같이 설정 내용을 추가합니다. 그렇지 않으면 흰 화면이 나옵니다.

```plist
<key>io.flutter.embedded_views_preview</key>
<string>YES</string>
```

<br>

### 1-2. `WebView` 위젯으로 웹페이지 보여주기

이 라이브러리 사용법은 매우 간단합니다. `WebView` 위젯을 사용하고, 보여줄 웹페이지의 URL을 지정해주는 것이 전부입니다. 한 가지 주의할 점은 HTTPS 프로토콜만 지원한다는 것이고요, 테스트 등을 위해 특별히 HTTP 프로토콜을 지원해야한다면 별도로 설정이 필요합니다.

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
