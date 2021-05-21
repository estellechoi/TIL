# Flutter 앱에서 Firebase를 사용하여 Sign in with Google 구현하기

<br>

1. 선행 작업하기
2. Firebase 프로젝트 생성하기
3. Firebase에 Android/iOS 앱 등록하기

<br>

## 1. 선행 작업하기

Flutter 앱에서 Google 로그인을 구현하기에 앞서 Firebase 프로젝트와 Android/iOS 앱을 각각 연결하는 작업이 필요합니다. 이를 위해서는 고유한 Android 앱 ID와 iOS 앱 ID가 필요하고요. iOS 앱을 연결하기 위한 선행 작업은 [Flutter 앱에서 Firebase를 사용하여 Sign in with Apple 구현하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-1-%EC%84%A0%ED%96%89-%EC%9E%91%EC%97%85%ED%95%98%EA%B8%B0) 문서를 참고하여 진행해주시고요, Android 앱 ID는 Flutter 프로젝트의 `/android/app/build.gradle` 파일에서 지정합니다. Android 앱 ID 설정에 대한 자세한 내용은 [Flutter 프로젝트를 Android 앱으로 배포하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy_android.md) 문서의 [빌드 구성 검토하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy_android.md#user-content-5-%EB%B9%8C%EB%93%9C-%EA%B5%AC%EC%84%B1-%EA%B2%80%ED%86%A0%ED%95%98%EA%B8%B0) 섹션을 참고하거나, [애플리케이션 ID 설정](https://developer.android.com/studio/build/application-id) 공식문서를 확인하세요.

<br>

## 2. Firebase 프로젝트 생성하기

Google [Firebase 콘솔](https://console.firebase.google.com/u/0/)에서 `프로젝트 만들기` 버튼을 클릭하여 프로젝트를 생성합니다. [Flutter 앱에서 Firebase를 사용하여 Sign in with Apple 구현하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md) 문서의 [2. Firebase 프로젝트 생성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-2-firebase-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0) 섹션을 참고하여 동일하게 진행합니다.

<br>

## 3. Firebase에 Android/iOS 앱 등록하기

Sign in with Apple 기능과는 달리, Google 로그인은 iOS 디바이스 사용자들도 종종 사용하므로 Android 앱과 iOS 앱에서의 구현을 모두 다루겠습니다.

<br>

### Android 앱 등록

<br>

### iOS 앱 등록

iOS 앱 등록은 [Firebase에 iOS 앱 등록하기](user-content-3-firebase에-ios-앱-등록하기)를 참고하여 진행해주세요.

<br>
<br>
<br>
<br>
<br>
<br>
<br>

---

### References

- [Social Authentication | FlutterFire](https://firebase.flutter.dev/docs/auth/social)
