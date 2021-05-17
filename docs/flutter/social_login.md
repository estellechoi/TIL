# Flutter 앱에서 Apple 로그인 구현하기

<br>

> 자세한 내용은 [iOS에서 Apple을 통해 인증](https://firebase.google.com/docs/auth/ios/apple?authuser=0) 문서와 Apple의 [로그인을 손쉽게](https://developer.apple.com/kr/sign-in-with-apple/get-started/) 문서를 참고하세요.

<br>

## 1. Firebase 프로젝트 생성하기

Google [Firebase](https://console.firebase.google.com/u/0/)에서 `프로젝트 만들기` 버튼을 클릭하여 프로젝트를 생성합니다. 보통 Firebase 콘솔에 표시될 프로젝트 이름으로 정합니다. 프로젝트 이름을 기반으로 고유한 프로젝트 ID가 생성되고 하단에 보여집니다. 프로젝트 생성이 완료되면 해당 프로젝트의 메인보드 화면으로 자동 이동됩니다. 이 화면은 [Firebase](https://console.firebase.google.com/u/0/)에서 생성한 프로젝트를 클릭하여 접근할 수 있습니다.

<br>

## 2. Firebase에 앱 등록하기

Firebase에 iOS 앱을 등록하기 위해 위에서 생성한 프로젝트 메인보드 페이지에서 iOS 아이콘을 클릭합니다. [Flutter 앱에 Firebase 추가](https://firebase.google.com/docs/flutter/setup?hl=ko) 문서를 참고하는 것이 정확합니다.

<br>

<img src="./../img/firebase9.png" alt="firebase" />

<br>

아래와 같이 나타난 폼 양식을 작성한 후 `앱 등록` 버튼을 클릭합니다.

- iOS 번들 ID : [Apple Developer](https://developer.apple.com/account/resources/identifiers/list/bundleId)에서 생성한 앱의 고유 번들 ID를 입력합니다.

- 앱 닉네임 : Firebase Console에서 본인만 볼 수 있는 편의용 닉네임입니다.

- App Store ID : 앱에 아직 App Store ID가 없으면 나중에 프로젝트 설정에서 ID를 추가할 수 있습니다.

<br>

<img src="./../img/firebase10.png" alt="firebase" />

<br>

프로젝트 메인보드 페이지로 이동하면 아래 스크린샷과 같이 방금 등록한 앱을 확인할 수 있습니다. 앱 이름을 클릭한 후 톱니바퀴 아이콘을 클릭하면 건너뛰었던 모든 내용을 편집할 수 있습니다.

<br>

<img src="./../img/firebase11.png" alt="firebase" />

<br>

## 3. Firebase 구성 파일 추가

`GoogleService-Info.plist 다운로드`를 클릭하여 Firebase iOS 구성 파일(`GoogleService-Info.plist`)을 가져옵니다.

<br>

<img src="./../img/firebase12.png" alt="firebase" />

<br>

파일 이름이 변경되지 않도록 주의하시고요, 그 다음 Flutter 프로젝트의 루트 경로를 기준으로 `ios/Runner.xcworkspace` 파일을 실행하여 Xcode를 열고 다운로드한 파일을 `Runner/Runner` 경로에 추가합니다. `Runner/Runner` 경로에서 마우스 우클릭 후 `Add Files to "Runner"`를 선택하여 추가하면 됩니다. 이때 Finder나 다른 에디터를 사용하지않고 반드시 Xcode를 사용하여 프로젝트에 해당 파일을 추가하세요. 그래야 파일이 Xcode 프로젝트에 연결됩니다.

<br>

<img src="./../img/firebase13.png" alt="firebase" />

<br>

## 4. FlutterFire 라이브러리 추가하기

Flutter 프로젝트에서는 [FlutterFire](https://firebaseopensource.com/projects/firebaseextended/flutterfire/)를 사용하여 Firebase API 등 다양한 플랫폼별 서비스에 접근할 수 있습니다. 각 Firebase 제품에 필요한 라이브러리를 추가하는 방식인데, 이러한 라이브러리들을 총칭하여 FlutterFire라고 부릅니다. FlutterFire 라이브러리들을 프로젝트에 추가하면 Firebase 앱의 iOS, Android 버전 모두에서 사용됩니다.

<br>

일반적으로 아래의 라이브러리들이 필요합니다. 프로젝트의 `pubspec.yaml` 파일에 추가합니다.

- [`firebase_core`](https://pub.dev/packages/firebase_core) : 모든 Flutter-Firebase 앱(iOS 및 Android 버전)에 Flutter용 `firebase_core` 플러그인이 필요합니다.

- [`firebase_auth`](https://pub.dev/packages/firebase_auth)

- [`cloud_firestore`](https://pub.dev/packages/cloud_firestore)

- [`firebase_analytics`](https://pub.dev/packages/firebase_analytics) : Google Analytics 사용 설정한 경우 필요합니다. 이 라이브러리를 추가한 경우 앱을 실행하여 Firebase를 성공적으로 통합했다는 확인을 Firebase에 보냅니다. 그렇지 않으면 확인 단계를 건너뛰어도 됩니다.

<br>

...

<br>
<br>

## 5. Firebase에서 Apple 로그인 사용 설정하기

이제 왼쪽 메뉴바에서 `Authentication` 메뉴를 클릭하여 이동한 후, `시작하기` 버튼을 클릭합니다.

<br>

<img src="./../img/firebase1.png" alt="firebase" />

<br>

그 다음, `Sign-in method` 탭의 `로그인 제공업체` 항목에서 `Apple`을 클릭하세요. 아래 스크린샷과 같은 폼 양식이 나타나는데요, Apple 로그인을 사용하기 위해 `사용 설정`을 활성화하고, 필요한 경우 입력 양식을 작성하세요. iOS 앱에서만 Apple 로그인을 사용한다면 일반적으로 아무것도 입력하지 않는 것이 좋습니다.

<br>

<img src="./../img/firebase2.png" alt="firebase" />

<br>

- `서비스 ID` : Android 앱에서 Apple 로그인을 사용할 때 필요한 ID 입니다. [Apple Store Connect](https://appstoreconnect.apple.com/)에서 생성한 앱 ID를 입력하면 됩니다.

  > 아직 앱 ID가 없다면, [Flutter 프로젝트를 iOS 앱으로 배포하기 : 앱 ID, 프로비저닝 프로파일, APNs, 미국 수출 규정](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md) 문서를 참고하여 앱 ID를 먼저 생성해주세요.

<br>

`OAuth 코드 흐름 구성(선택사항)` 항목을 펼치면 아래와 같은 양식이 추가로 나타납니다.

- `Apple 팀 ID` : 앱을 소유한 Apple 개발자 계정의 팀 ID를 입력합니다. 영대문자와 숫자로 이루어져있습니다.

- `키 ID` : ...

- `비공개 키` : ...

<br>

<img src="./../img/firebase3.png" alt="firebase" />

<br>

폼 양식의 하단에 있는 승인 콜백 URL은 [Apple Developer](https://developer.apple.com)에 등록하기위해 복사하여 보관합니다. 저장한 후 `로그인 제공업체` 항목에서 `Apple`이 `사용 설정됨` 상태로 바뀐 것을 확인할 수 있습니다. 아래의 `승인된 도메인` 항목에 Firebase 도메인이 생성된 것도 확인해주세요.

<br>

<img src="./../img/firebase4.png" alt="firebase" />

<br>

## 6. Apple Developer에서 서비스 ID 생성하기

이제 위에서 보관해둔 승인 콜백 URL과 Firebase 도메인을 사용하여 서비스 ID를 만들어야합니다. [Apple Developer](https://developer.apple.com) 웹사이트에서 [Account > Certificates, IDs & Profiles > Identifiers](https://developer.apple.com/account/resources/identifiers/list)로 이동합니다. `+` 버튼을 클릭하여 ID 생성을 시작합니다.

<br>

Sign in with Apple 서비스에 대한 ID를 생성할 것이므로 `Service IDs`를 선택하고 `Continue` 버튼을 클릭합니다.

<br>

<img src="./../img/firebase5.png" alt="firebase" />

<br>

아래의 두 항목을 입력하고 `Continue`, `Register` 버튼을 차례로 클릭하여 서비스 ID를 생성합니다.

- `Description` : 사용자가 Apple 로그인 과정에서 보게 될 앱의 이름을 작성합니다.

- `Identifier` : OAuth의 `client_id`로 사용될 서비스 ID를 작성합니다. 하단 안내문구에 따라 `com.domainname.appname` 형태로 작성합니다.
  > 앱 ID와 중복되지 않도록 작성합니다.

<br>

이제 [Identifiers](https://developer.apple.com/account/resources/identifiers/list/serviceId) 페이지에서 방금 생성한 서비스 ID 항목을 클릭하여 상세정보 페이지로 이동합니다.

<br>

<img src="./../img/firebase6.png" alt="firebase" />

<br>

Sign in with Apple에 체크합니다.

<br>

<img src="./../img/firebase6.png" alt="firebase" />

<br>

`Configure` 버튼을 클릭하면 아래 스크린샷과 같은 창이 나타납니다. Website URLs 섹션의 각 항목을 작성합니다. 두 항목에 필요한 값은 Firebase 프로젝트의 Authentication 메뉴에서 확인할 수 있습니다.

- Domains and Subdomains : Firebase 프로젝트의 승인된 도메인을 입력합니다.

- Return URLs : Firebase 프로젝트의 Apple 로그인 사용 설정시 확인했던 승인 콜백 URL을 입력합니다.

<br>

입력을 완료했으면 `Next`, `Done`을 클릭하여 완료하고, 다시 우측 상단의 `Continue`, `Save` 버튼을 클릭하여 마무리합니다.

<br>

---

### References

- [Flutter 앱에 Firebase 추가](https://firebase.google.com/docs/flutter/setup?hl=ko)