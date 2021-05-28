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

## 3. Apple 서버에 접근하기 위한 키 등록하기

이제 외부에서 Apple의 서버에 접근할 수 있도록 키를 등록해야합니다. 이 문서에서는 자체 서버를 구축하지 않고 FCM을 사용하므로, 여기에서 생성하는 키 파일은 Firebase 콘솔에서 프로젝트 셋업시 사용됩니다. Apple Developer [Certificates, Identifiers & Profiles > Keys](https://developer.apple.com/account/resources/authkeys/list) 페이지에서 `Create a key` 또는 `+` 버튼을 클릭하여 시작합니다.

<br>

<img src="./../img/firebase54.png" alt="firebase" />

<br>
<br>

`Key Name`을 입력하고요, `Apple Push Notifications service (APNs)` 항목을 `ENABLED`로 체크한 다음 `Continue`, `Register` 버튼을 차례로 클릭합니다. 화면에 표시된 안내문구대로, 이 단계에서 생성하는 키는 개발팀의 푸시 알림 서버와 Apple의 APNs를 연결하는 역할을 합니다.

<br>

<img src="./../img/firebase55.png" alt="firebase" />

<br>
<br>

키 등록이 완료되면 아래와 같이 키 정보와 경고문구가 나타납니다. 키는 단 한 번만 다운로드 가능하다는 내용입니다. 다른 Apple 서비스를 함께 사용한다면 해당 서비스들을 포함하도록 키를 편집한 후 다운로드하는 것이 좋습니다.

<br>

`Key ID` 항목에 표시된 키 ID는 추후 서버 설정시 사용됩니다. 사용할 서비스가 모두 선택되었는지 확인한 후 `Download` 버튼을 클릭하여 `p8` 포맷의 키 파일을 다운로드하고 보관합니다.

<br>

<img src="./../img/firebase56.png" alt="firebase" />

<br>
<br>

## 4. APN 인증서 생성 및 프로비저닝 프로파일(Provisioining profile) 구성하기

APN 서비스를 사용하는 경우 Xcode에서 프로비저닝 프로파일을 자동으로 생성할 수 없기 때문에, 개발자 인증 정보가 포함된 APN 서비스 인증서를 발급한 후 이를 포함한 프로비저닝 프로파일을 수동으로 가져와야 합니다. [APNs 인증서 생성 및 프로비저닝 프로파일 구성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-automatically-manage-signing)를 참고하여 Xcode에서 프로비저닝 프로파일을 서명에 포함하는 것까지 진행합니다. 또는 FlutterFire 공식문서의 [Generating a provisioning profile](https://firebase.flutter.dev/docs/messaging/apple-integration#3-generating-a-provisioning-profile)를 참고하여 진행합니다.

<br>

## 5. `ImageNotification` 추가하기

FCM을 통해 원하는 이미지를 푸시 알림에 노출시킬 수 있습니다. iOS 앱에서는 `ImageNotification` 라이브러리를 사용해야합니다. FlutterFire 공식문서의 [(Advanced, Optional) Allowing Notification Images](https://firebase.flutter.dev/docs/messaging/apple-integration#advanced-optional-allowing-notification-images)를 참고하여 진행합니다. 이 단계는 선택입니다.

<br>

## 6. Firebase 프로젝트 생성하기

이제 FCM 사용을 위해 [Firebase 프로젝트 생성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-2-firebase-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0)를 참고하여 Google Firbase 콘솔에서 프로젝트를 생성합니다.

<br>

## 7. Firebase에 iOS 앱 등록하기

[Firebase에 iOS 앱 등록하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-3-firebase%EC%97%90-ios-%EC%95%B1-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0), [Flutter 프로젝트에 Firebase 구성 파일 추가하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-4-flutter-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-firebase-%EA%B5%AC%EC%84%B1-%ED%8C%8C%EC%9D%BC-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0)를 참고하여 위 단계에서 생성한 Firebase 프로젝트에 앱을 연결해주세요.

<br>

## 8. Firebase 프로젝트에 APN 인증 키 등록하기

Firebase 콘솔의 [프로젝트 설정](https://console.firebase.google.com/project/_/settings/cloudmessaging) 페이지에서 `클라우드 메시징` 탭으로 이동합니다. `iOS 앱 구성` 섹션에서 위에서 등록한 iOS 앱을 선택한 후, `APN 인증 키` 섹션의 `업로드` 버튼을 클릭하면 아래와 같이 `APN 인증 키 업로드` 창이 나타납니다.

- `APN 인증 키` : [Apple 서버에 접근하기 위한 키 등록하기](#user-content-3-apple-서버에-접근하기-위한-키-등록하기)에서 다운로드했던 키 파일을 업로드합니다.

- `키 ID` : Apple Developer [Certificates, Identifiers & Profiles > Keys](https://developer.apple.com/account/resources/authkeys/list) 페이지에서 해당하는 키 항목을 클릭하여 확인할 수 있습니다.

- `팀 ID` : Apple Developer [Membership](https://developer.apple.com/account/#/membership) 페이지에서 확인할 수 있습니다.

<br>

<img src="./../img/firebase60.png" alt="firebase" />

<br>
<br>

## 9. 푸시 알림 구현하기

이제 모든 셋업이 완료되었습니다. Flutter 프로젝트에서 푸시 알림을 구현하는 코드는 FlutterFire 중 [`firebase_messaging`](https://pub.dev/packages/firebase_messaging) 라이브러리를 사용합니다. 라이브러리를 설치하고 [FlutterFire 초기화](https://firebase.flutter.dev/docs/overview/#initializing-flutterfire)를 완료한 후 진행해주세요.

<br>

### 1) 클래스 만들기

저는 `push_notification_manager.dart` 파일을 생성한 후 라이브러리를 임포트하고 아래와 같이 클래스를 생성했습니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class PushNotificationManager {
  FirebaseMessaging firebaseMessaging = FirebaseMessaging.instance;

  // ..
}
```

<br>

### 2) 사용자 허용 요청하기

사용자의 디바이스에 푸시 알림을 보내기 위해서는 사용자가 이를 허용해야합니다. 사용자가 거절한 이후에는 허용 요청을 다시 보내도 작동하지 않습니다. 허용 요청을 보내기 전 사용자가 푸시 알림 허용 요청을 받은 적이 있는지 확인하기 위해 아래와 같이 사용자의 설정값을 가져옵니다. 사용자가 푸시 알림을 허용할지 결정한 적이 없는 경우 `authorizationStatus` 값은 `AuthorizationStatus.notDetermined`입니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class PushNotificationManager {
  FirebaseMessaging firebaseMessaging = FirebaseMessaging.instance;

  Future<NotificationSettings> getFCMPermissionStatus() async {
    NotificationSettings previousSettings =
        await firebaseMessaging.getNotificationSettings();

    if (previousSettings.authorizationStatus ==
        AuthorizationStatus.notDetermined) {

        // ..
    }

    return previousSettings;
  }
}

```

<br>

이제 `requestPermission()` 메소드를 호출하여 사용자에게 허용 요청을 보냅니다. `requestPermission()` 메소드의 인자에는 사용자가 푸시 알림을 허용할 경우 기본값으로 지정될 설정값들을 넘깁니다. 각 인자에 대한 설명은 FlutterFire 공식문서의 [Permission settings](https://firebase.flutter.dev/docs/messaging/permissions#permission-settings)를 참고합니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class PushNotificationManager {
  FirebaseMessaging firebaseMessaging = FirebaseMessaging.instance;

  Future<NotificationSettings> getFCMPermissionStatus() async {
    NotificationSettings previousSettings =
        await firebaseMessaging.getNotificationSettings();

    if (previousSettings.authorizationStatus ==
        AuthorizationStatus.notDetermined) {
      NotificationSettings notificationSettings =
          await firebaseMessaging.requestPermission(
              alert: true,
              announcement: true,
              badge: true,
              carPlay: false,
              criticalAlert: false,
              provisional: false,
              sound: true);

      if (notificationSettings.authorizationStatus ==
          AuthorizationStatus.authorized) {
        print('User granted permission');
      } else if (notificationSettings.authorizationStatus ==
          AuthorizationStatus.provisional) {
        print('User granted provisional permission');
      } else {
        print('User declined or has not accepted permission');
      }

      return notificationSettings;
    }

    return previousSettings;
  }
}
```

<br>

[`provisional`](https://firebase.flutter.dev/docs/messaging/permissions#provisional-authorization) 상태는 iOS 12 이상에서 지원하는 설정값입니다.

<br>

### 3) 푸시 알림 보내기

기본적으로 푸시 알림은 앱이 백그라운드에 있거나 종료 상태일 때 작동합니다. 이 설정을 변경하려면 [Foreground Notifications](https://firebase.flutter.dev/docs/messaging/notifications/#foreground-notifications)를 참고하여 iOS/Android 각각 진행해야합니다. 참고로 FCM을 사용하면 스타일링, Foreground Notifications 등의 고급 기능은 지원하지 않습니다.

<br>

### 4) 사용자 동작 핸들링하기

이제 푸시 알림 전송 후 사용자의 행동에 따라 원하는 동작을 핸들링합니다. `firebase-messaging` 라이브러리는 다음 2가지 방법을 지원하고요, 자세한 내용은 공식문서의 [Handling Interaction](https://firebase.flutter.dev/docs/messaging/notifications#handling-interaction)를 참고합니다.

- `getInitialMessage()` : 종료 상태인 앱이 열리도록 야기한 메세지를 가져옵니다.

- `onMessageOpenedApp.listen()` : 백그라운드에 있던 앱이 열리는 경우 콜백을 사용하여 핸들링합니다.

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
- [Flutter push notifications with Firebase Cloud Messaging](https://blog.logrocket.com/flutter-push-notifications-with-firebase-cloud-messaging/#addingfunctionality)
