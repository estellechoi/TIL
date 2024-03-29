# Flutter에서 `provider`로 App State 관리하기

<br>

1. Flutter의 Declarative UI
2. Ephemeral State
3. App State
4. `provider`

<br>

## 1. Flutter의 Declarative UI

Flutter는 `state` 값이 변할 때마다 `state`의 현재 값을 반영하여 UI를 다시 빌드합니다. 이때 뷰(View) 전체를 다시 구성하지 않고요, 위젯 트리 내에서 변경된 `state` 값이 관여하는 특정 부분만 재구성하여 다시 렌더링하는 방식입니다. 이러한 UI 빌드 방식을 선언형(Declarative) UI 프로그래밍이라고 말합니다. `StatefulWidget` 내에서 `setState()`를 호출하여 `state` 값을 변경하고 위젯의 일부를 업데이트합니다. 이때 해당 위젯 인스턴스가 처음 생성될 때 지정한 인자 값들은 변하지 않고, 오직 `state` 값만이 변경되면서 위젯을 재구성합니다.

<br>

기존의 네이티브 앱 프레임워크에서 사용하는 Imperative UI 프로그래밍 방식과 비교해보면 확연히 와닿습니다. Imperative 방식은 UI에 변경사항이 생길 때마다 기존의 뷰 인스턴스를 제거하고, 새로운 뷰 인스턴스를 생성합니다. 새로운 인스턴스를 사용하여 화면을 통째로 다시 렌더링하기 때문에 뷰의 모든 구성이 재설정되는 방식이죠.

```dart
// 기존 뷰 인스턴스들을 제거
parentView.clearChildren();

// 새로운 뷰 인스턴스 생성 후 추가
ChildView c2 = ChildView(
    // ..
);
parentView.add(c2);

```

<br>

Flutter 공식 문서 [Introduction to declarative UI](https://flutter.dev/docs/get-started/flutter-for/declarative)가 도움이 되었습니다.

<br>

## 2. Ephemeral State

Ephemeral State는 하나의 위젯 내에서 유효한 `state`를 의미합니다. 흔히 로컬 `state`라고 합니다. `setState()` 메소드의 콜백을 통해 `state` 값을 변경할 수 있고요, 반드시 `setState()` 메소드를 사용해야만 변경된 `state` 값이 반영된 UI가 다시 렌더링됩니다.

<br>

```dart
bool _is = false;

setState(() {
    _is = true;
});
```

<br>

## 3. App State

특정 위젯에 얽매이지 않고 앱의 모든 곳에서 사용되는 `state`는 App State라고 합니다. 보통 로그인 정보, 쇼핑몰 장바구니 정보 등 여러 위젯에서 공유해야하는 값들을 App State로 지정하고 관리합니다.

<br>

중요한 것은 App State는 앱의 모든 위젯들보다 상위에 존재해야한다는 것입니다. App State가 변경될 때마다 이 값을 사용하는 모든 위젯들이 다시 UI를 빌드하는 방식이기 때문이죠. 만약 위젯 트리가 매우 복잡해서 10 개의 위젯이 겹쳐있고, 가장 하위 위젯에서 사용자가 버튼을 클릭했을 때 가장 상위 위젯에 있는 App State가 변경되어야 한다고 가정해봅시다. 하위 위젯에서 상위 위젯으로 콜백을 사용하여 변경할 값을 전달할 수는 있지만, 이 과정이 10번 반복됩니다. 그리고 중간에 위젯이 추가된다면 해당 콜백을 전달하는 인자를 매번 추가해줘야 하고요. 그래서 이런 문제를 간단하게 해주는 라이브러리를 사용하는 것이 권장되고요, 공식 문서 예제에서는 [`provider`](https://pub.dev/packages/provider) 라이브러리를 사용합니다. 이 외에도 [다양한 방법](https://flutter.dev/docs/development/data-and-backend/state-mgmt/options)으로 App State를 관리할 수 있습니다. [`redux`](https://pub.dev/packages/redux)를 사용하는 것이 적합한지 고민중이라면, 공식 문서에서 소개한 [You Might Not Need Redux: The Flutter Edition](https://proandroiddev.com/you-might-not-need-redux-the-flutter-edition-9c11eba006d7) 문서가 도움이 됩니다.

<br>

## 4. `provider`

라이브러리를 `pubspec.yaml` 파일에 추가하여 설치하고 임포트합니다.

```dart
import 'package:provider/provider.dart';
```

<br>

`provider`로 App State를 관리하는 일에는 다음 3가지 개념이 사용됩니다.

- `ChangeNotifier`

- `ChangeNotifierProvider`

- `Consumer`

<br>

### 4-1) `ChangeNotifier`

`ChangeNotifier`는 Flutter에서 기본으로 제공하는 클래스이고요, 이름 그대로 다른 곳에 변경을 알리는 단순한 역할을 합니다. `package:flutter/foundation.dart`를 임포트 한 후 `state` 변경이 일어나는 (가령 `AppState`) 클래스에서 `ChangeNotifier`를 상속하고요, 변경이 일어날 때마다 `notifyListeners()` 메소드를 호출하여 변경 알림을 보내면 됩니다.

```dart
import 'package:flutter/foundation.dart';

class AppState extends ChangeNotifier {
    final List<String> _strings = [];

    // ..

    void addItem(String string) {
        _strings.add(string);
        notifyListeners();
    }
}
```

<br>

그 다음, 해당 클래스의 인스턴스를 사용하는 곳에서 변경 알림을 받을 수 있도록 `addListener()`를 사용하여 리스너를 등록합니다.

```dart
final AppState appState = AppState();
appState.addListener(() {
    print('App state changed !');
});
```

<br>

### 4-2) `ChangeNotifierProvider`

`ChangeNotifierProvider`는 `provider` 라이브러리를 설치해야 사용할 수 있고요, 자손 위젯들이 동일한 `ChangeNotifier`를 공유할 수 있도록 `ChangeNotifier`를 사용하는 위젯의 인스턴스를 생성하여 제공하는 위젯입니다. 앱의 모든 위젯에서 `ChangeNotifier` 인스턴스에 접근할 수 있도록 하려면 `ChangeNotifierProvider`는 앱의 루트 위젯에 등록해야합니다. 그리고 `create` 속성에 하위 위젯들이 공유할 `AppState` 인스턴스를 반환하는 함수를 지정합니다. `AppState` 인스턴스가 더이상 필요하지 않게되면 `ChangeNotifierProvider`는 자동으로 `dispose()` 메소드를 호출하여 인스턴스를 제거합니다.

```dart
Future<void> main() async {
  // main 메소드에서 비동기로 데이터를 다룬 다음 runApp을 실행해야하는 경우
  WidgetsFlutterBinding.ensureInitialized();
  await DotEnv.load(fileName: '.env');

  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
      return ChangeNotifierProvider(
          create: (BuildContext ctx) => AppState()
          child: MaterialApp(
              // ..
          )
      );
  }
}
```

<br>

### 4-3) `Consumer`

이제 앱의 모든 위젯에서 `AppState` 인스턴스에 접근할 수 있게 되었습니다. 실제로 이 인스턴스에 접근하려면 UI 업데이트가 필요한 곳에서 `Consumer` 위젯을 사용하면 되고요, `Consumer<AppState>`와 같이 접근하고자 하는 인스턴스의 타입을 반드시 명시해야 합니다. 이제 `AppState`에서 변경이 발생할 때마다 `Consumer` 위젯의 `builder` 함수가 호출되고요, `builder` 함수의 두 번째 인자를 통해 `AppState` 인스턴스에 접근하여 변경된 값을 사용할 수 있습니다.

```dart
 @override
  Widget build(BuildContext context) {
      return Consumer<AppState>(
          builder: (context, appState, child) {
              return
              Column(
                  children: <Widget>[
                      Text(appState.loggedIn ? 'Sign out' : 'Sign in')
                  ]
              );
          }
      );
  }
```

<br>

`builder` 함수의 세 번째 인자(`child`)는 `AppState` 값이 변하더라도 UI 업데이트가 필요없는 부분입니다. `Consumer` 위젯의 `child` 인자로 고정된 UI를 지정해놓으면 `builder` 함수가 실행될 때 받아올 수 있습니다. `Consumer` 위젯의 `child` 인자를 따로 지정하지 않으면 이 값은 `null`입니다.

```dart
 @override
  Widget build(BuildContext context) {
      return Consumer<AppState>(
          builder: (context, appState, child) {
              return
              Column(
                  children: <Widget>[
                      if (child != null) child, // FixedWidget()
                      Text(appState.loggedIn ? 'Sign out' : 'Sign in')
                  ]
              );
          },
          // UI 업데이트가 필요없는 부분을 지정
          child: FixedWidget()
      );
  }
```

<br>

### 4-4) `Provider.of`

인스턴스의 값은 필요하지 않고 단순히 메소드만 호출할 때는 `Provider.of`를 사용하고요, `listen` 값을 `false`로 지정하여 인스턴스를 참조합니다.

```dart
Provider.of<AppState>(context, listen: false).clearAll();
```

<br>

---

### References

- [State management | Flutter](https://flutter.dev/docs/development/data-and-backend/state-mgmt/intro)
- [List of state management approaches | Flutter](https://flutter.dev/docs/development/data-and-backend/state-mgmt/options)
- [Simple app state management | Flutter](https://flutter.dev/docs/development/data-and-backend/state-mgmt/simple)
- [\[Flutter\] Provider로 앱 상태 관리하기](https://eunjin3786.tistory.com/255)
