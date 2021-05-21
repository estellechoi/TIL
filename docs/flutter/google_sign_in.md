# Flutter 앱에서 Firebase를 사용하여 Sign in with Google 구현하기

<br>

1. 선행 작업하기 : Android/iOS 앱 ID 가져오기
2. Firebase 프로젝트 생성하기
3. Firebase에 Android/iOS 앱 등록하기

<br>

## 1. 선행 작업하기 : Android/iOS 앱 ID 가져오기

> Sign in with Apple 기능과는 달리, Google 로그인은 iOS 디바이스 사용자들도 종종 사용하므로 Android 앱과 iOS 앱에서의 구현을 모두 다루겠습니다.

<br>

Flutter 앱에서 Google 로그인을 구현하기에 앞서 Firebase 프로젝트와 Android/iOS 앱을 각각 연결하는 작업이 필요합니다. 이를 위해서는 고유한 Android 앱 ID와 iOS 앱 ID가 필요하고요. iOS 앱을 연결하기 위한 선행 작업은 [Flutter 앱에서 Firebase를 사용하여 Sign in with Apple 구현하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-1-%EC%84%A0%ED%96%89-%EC%9E%91%EC%97%85%ED%95%98%EA%B8%B0) 문서를 참고하여 진행해주시고요, Android 앱 ID는 Flutter 프로젝트의 `/android/app/build.gradle` 파일에서 지정합니다. `defaultConfig` 섹션의 `applicationId` 값이 Android 앱의 ID가 됩니다.

```gradle
android {

    // ...

    defaultConfig {
        applicationId "com.hinoki.seoul"

        // ...
    }
}
```

<br>

Android 앱 ID 설정에 대한 자세한 내용은 [Flutter 프로젝트를 Android 앱으로 배포하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy_android.md) 문서의 [빌드 구성 검토하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy_android.md#user-content-5-%EB%B9%8C%EB%93%9C-%EA%B5%AC%EC%84%B1-%EA%B2%80%ED%86%A0%ED%95%98%EA%B8%B0) 섹션을 참고하거나, [애플리케이션 ID 설정](https://developer.android.com/studio/build/application-id) 공식문서를 확인하세요.

<br>

## 2. Firebase 프로젝트 생성하기

Google [Firebase 콘솔](https://console.firebase.google.com/u/0/)에서 `프로젝트 만들기` 버튼을 클릭하여 프로젝트를 생성합니다. [Flutter 앱에서 Firebase를 사용하여 Sign in with Apple 구현하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md) 문서의 [2. Firebase 프로젝트 생성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-2-firebase-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0) 섹션을 참고하여 동일하게 진행합니다.

<br>

## 3. Firebase에 Android/iOS 앱 등록하기

### Android 앱 등록

Firebase 프로젝트 메인페이지에서 아래 스크린샷에 표시한 `앱 추가` 버튼을 클릭해서 Android 앱 등록을 시작해주세요.

<br>

<img src="./../img/firebase40.png" alt="firebase" />

<br>
<br>

그 다음 나타나는 화면의 폼 양식을 작성하고 `앱 등록` 버튼을 클릭합니다. 각 항목은 아래를 참고하여 작성하세요.

<br>

<img src="./../img/firebase41.png" alt="firebase" />

<br>
<br>

- `Android 패키지 이름` : Flutter 프로젝트의 `build.gradle` 파일에 지정한 Android 앱의 ID를 입력합니다.

- `앱 닉네임` : Firebase 콘솔에서 사용하는 편의용 앱 이름입니다.

- `디버그 서명 인증서 SHA-1` : Google 로그인을 구현하는데 필요합니다. Firebase OAuth2 클라이언트와 API 키를 생성하는데 앱 서명 인증서의 [SHA-1(Secure Hash Algorithm 1)](https://en.wikipedia.org/wiki/SHA-1) 지문값(Fingerprint)가 사용되기 때문입니다. 앱 서명 인증서는 [Flutter 프로젝트를 Android 앱으로 배포하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy_android.md#user-content-2-%EC%95%B1-%EC%84%9C%EB%AA%85%ED%95%98%EA%B8%B0) 문서의 앱 서명하기 섹션을 참고하여 생성해주세요. 바로 아래 섹션을 참고하거나, [Authenticating Your Client](https://developers.google.com/android/guides/client-auth) 문서를 참고하여 인증서의 SHA-1 지문값을 가져올 수 있습니다. Android 앱에서 Firebase 서비스를 사용하기 위해서는 디버그용/배포용 SHA-1 지문값이 모두 필요하고요, 그 중 디버그용 지문값을 이 항목에 입력하면 됩니다.

<br>

#### Keytool을 사용하여 서명 인증서의 SHA-1 지문값 가져오기

Flutter 프로젝트의 루트 경로에서 아래의 `keytool` 명령어를 사용하여 보유한 인증서의 SHA-1 지문값을 가져올 수 있습니다. 디버그용/배포용 2개 지문값이 모두 필요합니다. [Authenticating Your Client](https://developers.google.com/android/guides/client-auth) 문서와 StackOverflow의 [Generate SHA-1 for Flutter/React-Native/Android-Native app](https://stackoverflow.com/questions/51845559/generate-sha-1-for-flutter-react-native-android-native-app) 페이지가 도움이 되었습니다.

<br>

- 디버그용 인증서 지문값 가져오기

```
keytool -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore
```

<br>

- 배포용 인증서 지문값 가져오기

```
keytool -list -v -alias <your-key-name> -keystore <path-to-production-keystore>
```

<br>

#### `signingReport`를 사용하여 서명 인증서의 SHA-1 지문값 가져오기

또는 `/android/gradlew` 파일이 있는 경로로 이동한 후 `signingReport` 명령어를 사용하여 디버그용/배포용 SHA-1 지문값을 가져올 수도 있습니다.

<br>

`android/` 경로로 이동하신 후,

```
cd android
```

<br>

아래 명령어를 실행합니다.

```
./gradlew signingReport
```

<br>

아래 스크린샷에 표시된 두 섹션의 `SHA1` 값이 우리가 필요한 값입니다.

<img src="./../img/firebase42.png" alt="firebase" />

<br>
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
