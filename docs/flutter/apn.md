# Flutter 앱에서 FCM(Firebase Cloud Messaging)을 사용하여 APN(Apple Push Notification) 구현하기

<br>

[FCM(Firebase Cloud Messaging)](https://firebase.flutter.dev/docs/messaging/overview)은 사용자의 디바이스에 설치된 앱에서 사용자에게 푸시 알림을 보낼 수 있는 Firebase의 서비스입니다. 별도로 푸시 알림 서버를 구축해서 사용할 수도 있지만, FCM을 사용하면 iOS와 Android 앱에 동시 사용이 가능합니다.

<br>

## 1. 선행 작업하기

iOS 앱의 경우 APN(Apple Push Notification) 서비스와 연동하여 구현합니다. APN 사용을 위해서는 몇 가지 선행 작업이 필요합니다. 이 문서는 이러한 선행작업을 포함하는 테스트 배포 작업을 완료했다고 가정하기 때문에 [Flutter 프로젝트를 iOS 앱으로 배포하기 : 앱 ID, 프로비저닝 프로파일, APNs, 미국 수출 규정](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md) 문서에서 필요한 내용을 참고하여 선행 작업을 진행하거나, 아래 단계들을 따라가며 최소한의 선행 작업을 진행하세요.

<br>

### 1) Apple Developer Program 등록

APN을 포함한 Apple 서비스를 이용하려면 개발자(팀)의 Apple 계정을 개발자 계정으로 전환해야합니다. 정확하게는 앱 배포를 위한 첫단계라고 할 수 있습니다. [Apple Developer Program 등록하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-1-apple-developer-program-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0)를 참고하여 개발자 계정으로 등록하세요. 이 단계에서 비용이 발생하고, 승인까지 시간이 소요될 수 있습니다.

<br>

### 2) 고유 앱 번들 ID 등록

개발자 계정으로 전환이 완료되면 Apple에서 앱을 식별할 수 있는 고유한 앱 ID를 생성해야합니다. 이 단계에서 생성하는 앱 ID가 뒤에 나오는 Firebase 콘솔에서의 앱 등록과 서비스 ID 생성에 사용됩니다. [Apple Developer에서 고유 앱 번들 ID 등록](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-1-%EA%B3%A0%EC%9C%A0-%EB%B2%88%EB%93%A4-id-%EB%93%B1%EB%A1%9D)을 참고하여 앱 ID를 등록하세요.

<br>

새로 앱 ID를 생성하는 경우라면, 이 단계에서 Capabilites 목록 중 `Push Notifications` 항목을 `ENABLED`로 체크하고 다음 단계는 건너뜁니다. 이미 앱 ID가 있다면, 이 단계는 건너뛰고 다음 단계에 따라 Capabilites 섹션을 수정합니다.

<br>

### 3) 앱 ID Capabilites에 `Push Notifications` 항목 추가

[Apple Developer > Identifiers](https://developer.apple.com/account/resources/identifiers/list) 페이지에서 수정할 앱 ID를 클릭하여 편집 페이지로 이동하세요. Capabilities 섹션에서 `Push Notifications` 항목을 `ENABLED`로 체크하고 `Save` 버튼을 클릭하여 수정합니다.

<br>

## 2. Xcode 프로젝트 설정에 `Push Notifications`/`Background Modes` 추가

### 1) `Push Notifications`

[Signing & Capabilities](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-2-signing--capabilities)를 참고하여 Xcode에서 프로젝트 설정을 열고 `Push Notifications` 서비스를 Capability로 추가합니다.

Flutter 프로젝트의 `ios/` 경로에서 `Runner.xcworkspace`를 실행시켜서 Xcode를 엽니다. 그 다음, 아래 스크린샷과 같이 Xcode에서 프로젝트의 `Runner` 경로를 열고 `Signing & Capabilities` 탭으로 이동한 후 `+ Capability` 버튼을 클릭하여 추가하면 됩니다.

<br>

<img src="./../img/firebase51.png" alt="firebase" />

<br>
<br>

### 2) `Background Modes` 추가

같은 탭에서 다시 `+ Capability` 버튼을 클릭하여 `Background Modes`를 추가합니다.

<br>

<img src="./../img/firebase52.png" alt="firebase" width="600" />

<br>
<br>

추가된 `Background Modes` 항목 내에서 아래와 같이 2개의 세부 항목에 체크합니다.

- `Background fetch`
- `Remote Notifications`

<br>

<img src="./../img/firebase53.png" alt="firebase" />

<br>
<br>

## 3. APNs 접근을 위한 키 등록하기

이제 Firebase에서 Apple의 APN 서비스에 접근할 수 있도록 키를 등록해야합니다. Apple Developer [Certificates, Identifiers & Profiles > Keys](https://developer.apple.com/account/resources/authkeys/list) 페이지에서 `Create a key` 또는 `+` 버튼을 클릭하여 시작합니다.

<br>

<img src="./../img/firebase54.png" alt="firebase" />

<br>
<br>

`Key Name`을 입력하고요, `Apple Push Notifications service (APNs)` 항목을 `ENABLED`로 체크한 다음 `Continue`, `Register` 버튼을 차례로 클릭합니다. 화면에 표시된 안내문구대로, 이 단계에서 생성하는 키는 개발팀의 푸시 알림 서버와 Apple의 APNs를 연결하는 역할을 합니다.

<br>

<img src="./../img/firebase55.png" alt="firebase" />

<br>
<br>

키 등록이 완료되면 아래와 같이 키 정보와 경고문구가 나타납니다. 키는 단 한 번만 다운로드 가능하다는 내용입니다. 다른 Apple 서비스를 함께 사용한다면 해당 서비스들을 함께 핸들링할 수 있도록 키를 편집한 후 다운로드하는 것이 좋습니다.

<br>

<img src="./../img/firebase56.png" alt="firebase" />

<br>
<br>
<br>
<br>
<br>
<br>

## 2. Firebase 프로젝트 생성하기

선행 작업을 완료했다면 [Firebase 프로젝트 생성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-2-firebase-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0)를 참고하여 Google Firbase 콘솔에서 프로젝트를 생성합니다.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

---

### References

- [FCM via APNs Integration | FlutterFire](https://firebase.flutter.dev/docs/messaging/apple-integration/)
