# Flutter 앱에서 APN(Apple Push Notification) 사용하기 + FCM(Firebase Cloud Messaging)

<br>

## 1. 선행 작업하기

iOS 앱에서 사용자에게 푸시 알림을 보내기 위해서는 APN(Apple Push Notification) 서비스를 사용합니다. APN 사용을 위해서는 몇 가지 선행 작업이 필요하고요, 이 문서는 이러한 선행 작업을 포함하는 테스트 배포 작업을 완료했다고 가정하기 때문에 [Flutter 프로젝트를 iOS 앱으로 배포하기 : 앱 ID, 프로비저닝 프로파일, APNs, 미국 수출 규정](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md) 문서에서 필요한 내용을 참고하여 선행 작업을 진행하거나, 아래 단계들을 따라가며 최소한의 선행 작업을 진행하세요.

<br>

### 1) Apple Developer Program 등록

APN을 포함한 Apple 서비스를 이용하려면 개발자(팀)의 Apple 계정을 개발자 계정으로 전환해야합니다. 정확하게는 앱 배포를 위한 첫단계라고 할 수 있습니다. [Apple Developer Program 등록하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-1-apple-developer-program-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0)를 참고하여 개발자 계정으로 등록하세요. 이 단계에서 비용이 발생하고, 승인까지 시간이 소요될 수 있습니다.

<br>

### 2) 고유 앱 번들 ID 등록

개발자 계정으로 전환이 완료되면 Apple에서 앱을 식별할 수 있는 고유한 앱 ID를 생성해야합니다. 이 단계에서 생성하는 앱 ID는 Firebase 콘솔에서 프로젝트에 앱을 등록할 때 사용됩니다. [Apple Developer에서 고유 앱 번들 ID 등록](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-1-%EA%B3%A0%EC%9C%A0-%EB%B2%88%EB%93%A4-id-%EB%93%B1%EB%A1%9D)을 참고하여 앱 ID를 등록하세요.

<br>

새로 앱 ID를 생성하는 경우라면, 이 단계에서 Capabilites 목록 중 `Push Notifications` 항목을 `ENABLED`로 체크하고 다음 단계는 건너뜁니다. 이미 앱 ID가 있다면, 이 단계는 건너뛰고 다음 단계에 따라 Capabilites 섹션을 수정합니다.

<br>

### 3) 앱 ID Capabilites에 `Push Notifications` 항목 추가

[Apple Developer > Identifiers](https://developer.apple.com/account/resources/identifiers/list) 페이지에서 수정할 앱 ID를 클릭하여 편집 페이지로 이동하세요. Capabilities 섹션에서 `Push Notifications` 항목을 `ENABLED`로 체크하고 `Save` 버튼을 클릭하여 수정합니다.

<br>

## 2. Xcode에서 `Push Notifications`/`background Modes` 추가

### 1) `Push Notifications`

Flutter 프로젝트의 `ios/` 경로에서 `Runner.xcworkspace`를 실행시켜서 Xcode를 엽니다. 그 다음, 아래 스크린샷과 같이 Xcode에서 프로젝트의 `Runner` 경로를 열고 `Signing & Capabilities` 탭으로 이동한 후 `+ Capability` 버튼을 클릭, `Push Notifications`를 추가합니다. 이 탭에 대한 추가 설명은 [Signing & Capabilities](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-2-signing--capabilities)를 참고하세요.

<br>

<img src="./../img/firebase51.png" alt="firebase" />

<br>
<br>

### 2) `background Modes` 추가

같은 탭에서 다시 `+ Capability` 버튼을 클릭하여 `background Modes`를 찾아 추가합니다.

<br>

<img src="./../img/firebase52.png" alt="firebase" width="600" />

<br>
<br>

추가된 `background Modes` 항목 내에 여러 개의 세부 항목이 보일겁니다. 아래와 같이 2개 항목에 체크합니다.

- `background fetch`
- `Remote Notifications`

<br>

<img src="./../img/firebase53.png" alt="firebase" />

<br>
<br>

## 3. Apple 서버에 접근하기 위한 키 등록하기

이제 외부에서 Apple의 APN 서버에 접근할 수 있도록 인증 키를 등록해야합니다. 이 문서에서는 자체 서버를 구축하지 않고 FCM을 사용하므로, 여기에서 생성하는 키 파일은 Firebase 콘솔에서 프로젝트 셋업시 사용됩니다. Apple Developer 웹사이트에서 [Certificates, Identifiers & Profiles > Keys](https://developer.apple.com/account/resources/authkeys/list) 메뉴로 이동, `Create a key` 또는 `+` 버튼을 클릭하여 시작합니다.

<br>

<img src="./../img/firebase54.png" alt="firebase" />

<br>
<br>

`Key Name`을 입력하고요, `Apple Push Notifications service (APNs)` 항목을 `ENABLED`로 체크합니다. 화면에 표시된 안내문구대로, 이 단계에서 생성하는 키는 개발팀의 푸시 알림 서버와 Apple의 APNs를 연결하는 역할을 합니다. `Continue`, `Register` 버튼을 차례로 클릭합니다.

<br>

<img src="./../img/firebase55.png" alt="firebase" />

<br>
<br>

키 등록이 완료되면 아래와 같이 키 이름, 키 ID, 접근할 서비스 정보가 표시됩니다. `Key ID` 항목에 표시된 키 ID는 개발팀의 푸시 알림 서버에서 APN 서버에 접근할 때 사용하게 됩니다. 한편 아래에 표시된 것과 같이 경고 문구가 보일겁니다. 키는 단 한 번만 다운로드 가능하다는 내용입니다. APN 외에 다른 Apple 서비스를 함께 사용한다면 해당 서비스들을 포함하도록 키를 편집한 후 계속 진행합니다. 사용할 서비스가 모두 선택되었는지 확인한 후 `Download` 버튼을 클릭하여 `p8` 포맷의 키 파일을 다운로드하고 보관합니다.

<br>

<img src="./../img/firebase56.png" alt="firebase" />

<br>
<br>

## 4. APN 서비스 인증서 발급받기

APN 서비스를 사용하는 경우 Apple Developer에서 서비스 인증서를 발급받아야합니다. 개발자 인증 정보가 담긴 APN 서비스 인증서를 발급받고, 이 인증서 정보가 포함된 프로비저닝 프로파일(Provisioining Profile)을 생성한 후 프로젝트에 추가해야하죠. [APNs 인증서 생성 및 프로비저닝 프로파일 구성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/deploy.md#user-content-automatically-manage-signing)를 참고하여 다음 단계까지 진행합니다. 이 레퍼런스 문서는 Xcode에서 프로비저닝 프로파일을 앱 서명에 포함하는 것까지 진행합니다.

<br>

## 5. 프로비저닝 프로파일(Provisioining Profile) 구성하기

이제 위 단계에서 생성한 서비스 인증서가 앱 ID 정보에 자동으로 추가되었기 때문에, 프로비저닝 프로파일을 생성하면 됩니다. 이러한 추가적인 인증 정보를 담은 프로비저닝 프로파일은 Xcode를 사용하여 자동으로 생성할 수 없기 때문에 Apple Developer에서 진행합니다. 위 단계에서 언급한 동일한 문서를 참고하거나, FlutterFire 공식 문서의 [Generating a provisioning profile](https://firebase.flutter.dev/docs/messaging/apple-integration#3-generating-a-provisioning-profile)를 참고하여 진행합니다.

<br>

여기까지 진행하시면 APN 사용을 위한 셋업은 끝입니다.

<br>

## 6. FCM(Firebase Cloud Messaging)

[FCM(Firebase Cloud Messaging)](https://firebase.flutter.dev/docs/messaging/overview)은 앱을 통해 사용자의 디바이스로 메시지를 보낼 수 있는 Firebase의 메시징 솔루션입니다. 푸시 알림 메시지, 데이터 메시지, 채팅 등의 메시지 전송을 지원하고요, 최대 4KB 크기의 페이로드 형태로 메시지를 전송할 수 있습니다. 자세한 설명은 [Firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/?authuser=0#implementation_paths) 문서를 참고하세요.

<br>

FCM을 사용하여 보낼 수 있는 [메시지 타입](https://firebase.flutter.dev/docs/messaging/usage/#message-types)은 다음 3가지로 나눠볼 수 있습니다.

- 알림 메시지(`Notification only`) : 사용자에게 푸시 알림을 통해 메시지를 표시합니다. FCM이 앱을 대신하여 사용자의 디바이스에 자동으로 메시지를 표시합니다.

- 데이터 메시지(`Data only`) : '조용한 메시지'로 불립니다. FCM이 아닌 앱이 메시지 처리를 담당합니다. 디바이스에서 이 타입의 메시지는 우선순위가 낮다고 판단하기 때문에 무시될 수 있습니다.

- 알림 & 데이터 메시지(`Notification & Data`) : 푸시 알림과 데이터 메시지를 함께 보냅니다.

<br>

각 타입의 메시지를 전송하는 자세한 방법은 공식 문서의 [메시지 유형](https://firebase.google.com/docs/cloud-messaging/concept-options?authuser=0#notifications_and_data_messages)을 참고하세요. 다음 단계에서는 바로 메시지 전송을 다루지 않고, FCM 사용을 위한 Firebase 프로젝트 셋업을 다룹니다.

<br>

## 7. Firebase 프로젝트 생성하기

이제 FCM 사용을 위해 [Firebase 프로젝트 생성하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-2-firebase-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0)를 참고하여 Google Firbase 콘솔에서 프로젝트를 생성합니다.

<br>

## 8. Firebase에 iOS 앱 등록하기

[Firebase에 iOS 앱 등록하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-3-firebase%EC%97%90-ios-%EC%95%B1-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0), [Flutter 프로젝트에 Firebase 구성 파일 추가하기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-4-flutter-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-firebase-%EA%B5%AC%EC%84%B1-%ED%8C%8C%EC%9D%BC-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0)를 참고하여 위 단계에서 생성한 Firebase 프로젝트에 앱을 연결해주세요.

<br>

## 9. Firebase 프로젝트에 APN 인증 키 등록하기

Firebase 콘솔의 [프로젝트 설정](https://console.firebase.google.com/project/_/settings/cloudmessaging) 페이지에서 `클라우드 메시징` 탭으로 이동합니다. `iOS 앱 구성` 섹션에서 위에서 등록한 iOS 앱을 선택한 후, `APN 인증 키` 섹션의 `업로드` 버튼을 클릭하면 아래와 같이 `APN 인증 키 업로드` 창이 나타납니다.

- `APN 인증 키` : [Apple 서버에 접근하기 위한 키 등록하기](#user-content-3-apple-서버에-접근하기-위한-키-등록하기)에서 다운로드했던 키 파일을 업로드합니다.

- `키 ID` : Apple Developer [Certificates, Identifiers & Profiles > Keys](https://developer.apple.com/account/resources/authkeys/list) 페이지에서 해당하는 키 항목을 클릭하여 확인할 수 있습니다.

- `팀 ID` : Apple Developer [Membership](https://developer.apple.com/account/#/membership) 페이지에서 확인할 수 있습니다.

<br>

<img src="./../img/firebase60.png" alt="firebase" />

<br>
<br>

## 10. 푸시 알림 핸들링하기

### 1) 앱의 3가지 상태 : `foreground`/`background`/`terminated`

앱이 어떤 상태에 있는지에 따라 푸시 알림 핸들링도 달라지기 때문에 먼저 앱의 상태에 대해 알아야합니다. 사용자의 디바이스에 설치된 앱은 다음 3가지 중 하나의 상태를 갖게 된다는 것을 알아두세요.

- `foreground` : 앱이 켜져있고 사용중인 상태

- `background` : 앱이 켜져있지만, 사용중이지 않고 백그라운드에 있는 상태

- `terminated` : 앱이 켜져있지 않거나 디바이스가 잠겨있는 상태

<br>

기본적으로 대부분 플랫폼에서 푸시 알림은 앱이 `background`나 `terminated` 상태일 때만 작동합니다. 필요한 경우 공식 문서의 [Foreground Notifications](https://firebase.flutter.dev/docs/messaging/notifications/#foreground-notifications)를 참고하여 `foreground` 상태에서 푸시 알림이 어떻게 동작할지 컨트롤할 수 있습니다.

<br>

> FCM은 Foreground Notifications, 스타일링 등의 고급 기능은 지원하지 않습니다.

<br>

### 2) `firebase_messaging` 라이브러리 설치, FlutterFire 초기화하기

푸시 알림 전송시 앱이 수행할 일을 정의하고, 사용자에게 알림 허용을 묻는 등의 작업들 공식 문서에 따라 FlutterFire 중 [`firebase_messaging`](https://pub.dev/packages/firebase_messaging) 라이브러리를 사용합니다. 라이브러리를 설치하시고요, FlutterFire 라이브러리를 사용하므로 [FlutterFire 초기화](https://firebase.flutter.dev/docs/overview/#initializing-flutterfire)를 완료한 후 다음 단계를 진행해주세요.

<br>

#### \* FlutterFire 요구 버전 맞추기

FlutterFire 라이브러리를 사용하려면 Xcode 프로젝트에서 iOS 10.0 이상을 타겟팅하고, `Podfile`에도 플새폼 버전을 `10.0`으로 명시해야합니다. [FlutterFire 요구 버전 맞추기](https://github.com/estellechoi/TIL/blob/master/docs/flutter/social_login.md#user-content-flutterfire-%EC%9A%94%EA%B5%AC-%EB%B2%84%EC%A0%84-%EB%A7%9E%EC%B6%94%EA%B8%B0)를 참고하여 진행합니다.

<br>

### 3) `onBackgroundMessage()`

`FirebaseMessaging.onBackgroundMessage()`를 사용하여 앱이 `background` 상태일 때 메시지를 수신하는 경우를 핸들링할 수 있습니다. 사용자가 푸시 알림을 클릭하거나 스와이프하여 무시하는 등 어떤 행동을 하더라도 상관없습니다. 메시지가 수신되기만 하면 `onBackgroundMessage()`의 콜백 함수가 실행되기 때문입니다. 이때 실행되는 콜백 핸들링 함수는 앱의 실행 Context에서 떨어져 완전히 독립적으로 실행되기 때문에 앱의 `state` 값이나 UI 업데이트와 같은 작업은 수행할 수 없습니다. HTTP 요청이나, 로컬 스토리지 업데이트와 같은 단순 로직 수행만이 가능하죠.

<br>

다음은 핸들링 함수가 지켜야할 조건입니다.

- 익명 함수를 사용하지 않습니다.
- 별도의 `class`로 묶지 않고, `main()` 메소드 내에 바로 구현합니다.

<br>

아래는 예제 코드입니다. `main()` 메소드 내에서 사용해야하므로 `main.dart` 파일을 열고요, 라이브러리를 임포트합니다. 그 다음 `runApp()`을 실행하기 전에 `FirebaseMessaging.onBackgroundMessage()`를 호출하여 `Future<void>` 타입의 핸들링 함수를 등록합니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

// 익명함수를 사용할 수 없기 때문에 아래와 같이 선언 후에 참조합니다.
Future<void> _handleFirebaseMessage(RemoteMessage message) async {
  await Firebase.initializeApp();
  print("A background message: ${message.messageId}");
}

Future<void> main() async {
  // main 메소드에서 비동기로 데이터를 다룬 다음 runApp을 실행해야하는 경우 필요한 메소드입니다.
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp();
  FirebaseMessaging.onBackgroundMessage(_handleFirebaseMessage);

  runApp(MyApp());
}
```

<br>

### 4) FCM 핸들링을 위한 클래스 만들기

이제 FCM과 관련된 나머지 기능들을 모아둘 `fcm_controller.dart` 파일을 생성한 후, 라이브러리를 임포트하고 아래와 같이 클래스를 만듭니다. `FirebaseMessaging` 인스턴스는 인자로 받기로하고요.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class FCMController {
  final FirebaseMessaging fcm;

  FCMController(this.fcm);

  // ..
}
```

<br>

### 5) iOS 사용자에게 허용 요청하기

> Flutter 앱을 웹으로 배포하는 경우에도 이 단계를 진행해야합니다. Android 앱은 이 단계를 생략합니다.

<br>

사용자의 iOS 디바이스에 푸시 알림을 보내기 위해서는 사용자가 푸시 알림을 허용할지 묻고, 사용자가 `허용`을 선택해야합니다. 사용자가 `허용 안함`을 선택하면, 요청을 다시 보내더라도 사용자에게 요청이 표시되지 않습니다.

<br>

#### 5-1) `getNotificationSettings()`

푸시 알림 허용 요청을 보내기 전에 사용자가 이미 요청을 받은 적이 있는지 확인하기 위해서는 아래와 같이 `getNotificationSettings()` 메소드를 호출하여 사용자의 푸시 알림 설정값을 가져옵니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class FCMController {
  final FirebaseMessaging fcm;

  FCMController(this.fcm);

  Future<void> requestPermission() async {

    // 사용자가 이전에 설정한 알림 허용 상태를 가져옵니다.
    NotificationSettings previousSettings =
        await fcm.getNotificationSettings();

    // 사용자가 결정하지 않은 경우, 혀용 요청을 보냅니다.
    if (previousSettings.authorizationStatus ==
        AuthorizationStatus.notDetermined) {

        // ..
    }

  }
}

```

<br>

`NotificationSettings`가 반환되고요, `authorizationStatus` 속성을 통해 사용자의 설정값을 확인할 수 있습니다. 다음 4가지 값 중 하나입니다.

- `authorized` : 사용자가 허용했음

- `denied` : 사용자가 거절했음

- `noDetermined` : 사용자가 허용여부를 결정하지 않았음

- `provisional` : 사용자가 허용했으나, Provisional 허용임

<br>

#### 5-2) `requestPermission()`

이제 `requestPermission()` 메소드를 호출하여 사용자에게 허용 요청을 보냅니다. 메소드의 인자에는 사용자가 푸시 알림을 허용할 경우 기본 설정값으로 지정될 값들을 넘깁니다. 각 인자에 대한 설명은 FlutterFire 공식문서의 [Permission settings](https://firebase.flutter.dev/docs/messaging/permissions#permission-settings)를 참고합니다. 참고로 [`provisional`](https://firebase.flutter.dev/docs/messaging/permissions#provisional-authorization)은 iOS 12 이상에서 지원하는 설정값입니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class FCMController {
  final FirebaseMessaging fcm;

  FCMController(this.fcm);

  Future<void> requestPermission() async {
    NotificationSettings previousSettings =
        await fcm.getNotificationSettings();

    if (previousSettings.authorizationStatus ==
        AuthorizationStatus.notDetermined) {
      NotificationSettings notificationSettings =
          await fcm.requestPermission(
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
    }
  }
}
```

<br>

마지막으로, 위에서 만든 메소드를 `main.dart` 파일의 루트 위젯에서 위젯이 초기화되면 호출하거나 개인의 전략에 맞게 적절한 시점에 호출합니다.

```dart
class _MyAppState extends State<MyApp> {

  final FirebaseMessaging _fcm = FirebaseMessaging.instance;
  late final FCMController _fcmController;

  @override
  void initState() {
    super.initState();

    // ..

    _fcmController = FCMController(_fcm);

    if (Platform.isIOS) {
      _fcmController.requestPermission();
    }
  }
}
```

<br>

### 6) 알림 전송 후 사용자 동작 핸들링하기

> 푸시 알림을 전송하는 역할은 서버에서 하는데요, 이 내용은 뒤에서 다루겠습니다.

<br>

푸시 알림이 전송되었을 때 사용자의 행동에 따라 진행되어야 할 앱의 동작을 핸들링해봅시다. 아래의 2가지 메소드를 사용할 수 있고요, 공식 문서에서는 2개 메소드를 모두 구현하여 스무스한 UX를 제공하는 것을 권유합니다. 자세한 내용은 공식문서의 [Handling Interaction](https://firebase.flutter.dev/docs/messaging/notifications#handling-interaction)를 참고합니다.

- `getInitialMessage()`

- `onMessageOpenedApp.listen()`

<br>

#### 6-1) `getInitialMessage()`

`terminated` 상태인 앱이 열렸을 때 메시지 정보가 담긴 `RemoteMessage`를 반환합니다. 아래는 예제 코드이고요, `getInitialMessage()` 메소드를 호출하고 다음 동작을 정의하는 메소드를 `FCMController` 클래스에 추가합니다.

```dart
import 'package:firebase_messaging/firebase_messaging.dart';

class FCMController {
  final FirebaseMessaging fcm;

  FCMController(this.fcm);

  // ..

  Future<void> getMessage() async {
    RemoteMessage? message = await fcm.getInitialMessage();

    print('getInitialMessage() called !');
    print(message?.data);
  }
}
```

<br>

그 다음 `main.dart` 파일을 열고, 루트 위젯의 `initState()` 메소드 내에서 호출합니다.

```dart
class _MyAppState extends State<MyApp> {

  final FirebaseMessaging _fcm = FirebaseMessaging.instance;
  late final FCMController _fcmController;

  @override
  void initState() {
    super.initState();

    // ..

    _fcmController = FCMController(_fcm);

    if (Platform.isIOS) {
      _fcmController.requestPermission();
    }

    _fcmController.getMessage();
  }
}
```

<br>

#### 6-2) `onMessageOpenedApp.listen()`

`background` 상태인 앱이 사용자 동작에 의해 `foreground` 상태로 바뀌는 경우, 콜백을 사용하여 핸들링합니다. 콜백의 인자로부터 `RemoteMessage`를 가져올 수 있습니다.

```dart
class _MyAppState extends State<MyApp> {

  final FirebaseMessaging _fcm = FirebaseMessaging.instance;
  late final FCMController _fcmController;

  @override
  void initState() {
    super.initState();

    // ..

    _fcmController = FCMController(_fcm);

    if (Platform.isIOS) {
      _fcmController.requestPermission();
    }

    _fcmController.getMessage();

    FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
      print('FirebaseMessaging.onMessageOpenedApp listened !');
      print(message.data);
    });
  }
}
```

<br>

## 11. 메시지 전송하기

### 1) Firebase 콘솔에서 보내기

당장 디바이스로 메시지를 전송하고 테스트해볼 수 있는 가장 빠른 방법은 Firebase 콘솔의 [Cloud Messaging](https://console.firebase.google.com/project/_/notification) 메뉴에서 알림 작성기를 사용하는 것입니다. `Send your first message` 버튼을 클릭하여 알림 작성기를 시작합니다.

<br>

<img src="./../img/firebase61.png" alt="firebase" />

<br>
<br>

아래와 같이 알림 작성 페이지로 이동했다면, `알림 제목`, `알림 텍스트` 항목을 각각 입력합니다. 우측의 `테스트 메시지 전송` 버튼을 클릭하면 지정된 개발용 디바이스로 테스트 메시지를 전송할 수 있습니다.

<br>

<img src="./../img/firebase65.png" alt="firebase" />

<br>
<br>

버튼을 클릭하시고요, 아래와 같이 창이 나타나면 `FCM 등록 토큰 추가` 입력란에 개발용 디바이스의 토큰값을 입력하여 추가한 후 테스트해볼 수 있습니다.

<br>

<img src="./../img/firebase66.png" alt="firebase" />

<br>
<br>

`다음` 버튼을 클릭하여 타겟 조건을 설정해보세요. 앱 버전, 언어, 국가/지역 등 여러 조건을 추가하여 특정 사용자들에게만 메시지가 전송되도록 타겟팅할 수 있습니다. 사용자를 한 명씩 지목해서 전송할 수도 있는데요, 이 기능은 Firebase 콘솔에서는 불가능하고 Admin SDK를 통해서만 가능합니다.

<br>

<img src="./../img/firebase63.png" alt="firebase" />

<br>
<br>

메시지를 전송할 날짜와 시간, 반복 발송여부를 지정할 수 있습니다.

<br>

<img src="./../img/firebase64.png" alt="firebase" />

<br>
<br>

`추가 옵션(선택사항)` 섹션에서는 키-값 쌍으로 이루어진 맞춤 데이터를 추가할 수 있습니다. 이 데이터는 사용자에게는 보이지 않고요, 프로젝트 코드에서 `firebase_messaging` 라이브러리를 사용하여 가져온 후 값에 따라 다른 동작을 수행하도록 코딩할 수 있습니다.

<br>

- 주의사항

```
Flutter로 개발한 Android 앱에 메시지를 보내는 경우 아래의 키-쌍 데이터를 반드시 추가해야 합니다.
그렇지 않으면 사용자가 푸시 알림을 탭해도 작동하지 않는 이슈가 있기 때문입니다.

- 키 : click_action
- 값 : FLUTTER_NOTIFICATION_CLICK
```

<br>

<img src="./../img/firebase67.png" alt="firebase" />

<br>
<br>

알림 설정이 완료되면 `검토` 버튼을 클릭하고, 나타나는 메시지 검토 창의 내용을 확인한 후 `게시` 버튼을 클릭합니다. 기다리면 아래와 같이 푸시 알림을 받을 수 있고요, iOS 앱의 경우 이뮬레이터에서 테스트할 수 없기 때문에 실제 디바이스에서 앱을 설치한 후 테스트해야합니다.

<br>

<img src="./../img/push_notification_success.png" alt="firebase" width="400" />

<br>
<br>

### 2) Admin SDK 사용하여 보내기

푸시 알림 서버를 직접 구축하고 [Admin SDK](https://firebase.google.com/docs/reference/admin)를 사용하면 메시지 데이터를 커스텀하여 전송할 수 있습니다. Admin SDK는 인증된 외부 환경에서 Firebase와 상호 작용할 수있는 서버 라이브러리 모음입니다. Node.js, Java, Python, Go 및 C # (. NET)을 지원합니다. 자세한 내용은 [서버에 Firebase Admin SDK 추가](https://firebase.google.com/docs/admin/setup) 문서를 참고합니다.

<br>

### 3) REST API 사용하여 보내기

Admin SDK를 사용할 수 없는 상황이라면, Firebase에서 제공하는 REST API를 사용할 수 있습니다.

```
POST https://fcm.googleapis.com/v1/projects/myproject-b5ae1/messages:send HTTP/1.1

Content-Type: application/json
Authorization: Bearer ya29.ElqKBGN2Ri_Uz...HnS_uNreA

{
   "message":{
      "token":"token_1",
      "data":{},
      "notification":{
        "title":"FCM Message"
        "body":"This is an FCM notification message!",
      }
   }
}
```

<br>

## 12. `ImageNotification` 추가하기

FCM을 통해 원하는 이미지를 푸시 알림에 노출시킬 수 있습니다. iOS 앱에서는 `ImageNotification` 라이브러리를 사용해야합니다. FlutterFire 공식문서의 [(Advanced, Optional) Allowing Notification Images](https://firebase.flutter.dev/docs/messaging/apple-integration#advanced-optional-allowing-notification-images)를 참고하여 진행합니다. 이 단계는 선택입니다.

<br>

---

### References

- [FCM via APNs Integration | FlutterFire](https://firebase.flutter.dev/docs/messaging/apple-integration/)
- [Flutter push notifications with Firebase Cloud Messaging](https://blog.logrocket.com/flutter-push-notifications-with-firebase-cloud-messaging/#addingfunctionality)
- [iOS 앱에서 메시지 수신 | Firebase](https://firebase.google.com/docs/cloud-messaging/ios/receive?hl=ko)
- [FCM 메시지 정보 | Firebase](https://firebase.google.com/docs/cloud-messaging/concept-options)
- [Firebase 클라우드 메시징 | Firebase](https://firebase.google.com/docs/cloud-messaging/?authuser=0#implementation_paths)
- [iOS에서 Firebase 클라우드 메시징 클라이언트 앱 설정 | Firebase](https://firebase.google.com/docs/cloud-messaging/ios/client?authuser=0#analytics-enabled)
- [FCM에서 APN 구성](https://firebase.google.com/docs/cloud-messaging/ios/certs?authuser=0)
- [백그라운드 iOS 앱에 테스트 메시지 보내기](https://firebase.google.com/docs/cloud-messaging/ios/first-message?authuser=0)
