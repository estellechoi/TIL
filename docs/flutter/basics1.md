# Flutter 튜토리얼 1 : Stateful/Stateless 위젯, 무한 스크롤, 콜백

> [Flutter](https://flutter.dev/docs/get-started/codelab) 공식문서를 요약/번역한 튜토리얼 문서입니다. 이 튜토리얼에서는 간단한 캘린더 앱을 만들어볼 건데요, 이름은 Hinoki Calendar라고 하겠습니다.

<br>

## Flutter와 Material

> [Material](https://material.io/guidelines) is a visual design language that is standard on mobile and the web.

<br>

Flutter는 Material Design에 기반한 다양한 위젯(Widget)을 제공합니다. Flutter를 처음 사용해보신다면, Material 라이브러리를 사용하여 간단한 앱을 개발해보는 것이 사용법을 익히기 편하겠다는 생각입니다. Flutter에서 Material 위젯들을 사용하려면 프로젝트 루트 경로에 있는 `pubspec.yaml`파일을 열어보시고요, `flutter` 섹션에 `uses-material-design: true` 설정을 추가하세요. Android Studio 편집기를 사용하여 Flutter 프로젝트를 생성하셨다면 아마 이 설정이 기본으로 추가되어 있을겁니다.

<br>

## 위젯(Widget)

앱을 실행시키는 코드는 `lib/main.dart` 파일에 있습니다. Material 위젯들을 사용하기 위해 파일 상단에 해당 라이브러리를 임포트해주세요.

```dart
import 'package:flutter/material.dart';
```

<br>

앱은 `main()` 메소드 내에서 실행시킵니다. `MyApp`은 최상위 위젯의 이름이고요.

```dart
void main() => runApp(MyApp());
```

<br>

이 최상위 위젯은 사실 `MyApp`이라는 이름의 클래스입니다. 이 클래스는 `StatelessWidget`이라는 추상(`abstract`) 클래스를 상속함으로써 위젯으로서 기능하게 되죠. 앱에서 실제로 렌더링시킬 내용들은 위젯의 `build()` 메소드를 오버라이드(`@override`)함으로써 정의합니다. 이 메소드의 리턴 타입(Type)은 `Widget`입니다. React를 사용해보셨다면 React 컴포넌트에서 가상 DOM을 `return`하는 부분과 비슷하고요, Vue를 사용해보셨다면 컴포넌트의 `<template>` 부분과 비슷하다고 볼 수 있겠네요.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      // ...
  }
}
```

<br>

이제 `build()` 메소드에서 원하는 위젯을 반환하면 됩니다. Material 라이브러리에서 제공하는 `MaterialApp` 위젯을 사용해보죠. 아래와 같이 `MaterialApp` 클래스의 인스턴스를 생성하여 반환하는 코드를 추가합니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          // ...
      )
  }
}
```

<br>

`MaterialApp` 위젯의 인스턴스를 생성할 때 `title`과 `home` 속성을 지정하여 인자로 넘겨봅시다. `title` 값은 위젯의 제목으로 사용되고요, `home` 값은 해당 위젯이 실제 스크린에 어떻게 렌더링될지 결정합니다. 따라서 또 다른 하위 위젯을 `home`의 값으로 지정해주면 됩니다. Material 라이브러리의 `Scaffold` 위젯을 사용해보죠.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          title: 'Material App',
          home: Scaffold(
            // ...
          )
      )
  }
}

```

<br>

## `Scaffold` 위젯

`Scaffold` 위젯은 기본으로 앱의 상단 바와 나머지 부분으로 나뉘어진 UI를 제공합니다. `appBar`와 `body` 속성을 사용하여 각 부분에 들어갈 또다른 하위 위젯을 지정해줍니다. 아래의 예제 코드에서는 `appBar` 속성에 `AppBar` 위젯을 지정했고요, `AppBar` 위젯의 `title` 속성에는 `Text` 위젯을 사용했습니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          title: 'Material App',
          home: Scaffold(
            appBar: AppBar(
              title: Text('Hinoki Calendar')
            ),
            body: Center(
              child: Text('Calendar should be here...')
            )
          )
      )
  }
}

```

<br>

## 외부 라이브러리 사용하기

> [pub.dev](https://pub.dev/)에서 외부 라이브러리를 검색할 수 있습니다.

<br>

캘린더 UI를 사용하기 위해 [`table_calendar`](https://pub.dev/packages/table_calendar)라는 외부 라이브러리를 사용해보겠습니다. 먼저 라이브러리를 설치해야합니다. `pubspec.yaml` 파일을 열어보시고요, `dependencies` 섹션을 찾아주세요. 이미 `flutter` 라이브러리가 추가되어 있네요. 여기에 `table_calendar` 라이브러리를 원하는 버전 정보와 함께 추가하세요. 해당 섹션의 모습은 아래와 같거나 다른 라이브러리들과 함께 채워져있을 겁니다.

> 마치 NPM의 `package.json` 파일과 비슷하네요.

<br>

```yaml
dependencies:
  flutter:
    sdk: flutter
  table_calendar: ^2.3.3
```

<br>

Android Studio 편집기를 사용하신다면 `Pub get` 버튼을 클릭하여 `pubspec.yaml` 파일에 명시한 라이브러리들을 설치해주세요. 라이브러리가 설치되면서 프로젝트에 `pubspec.lock` 파일이 자동으로 추가됩니다. 이 파일은 라이브러리의 버전을 고정하여 의도하지 않은 버전의 라이브러리가 설치되는 것을 방지하는데 사용됩니다.

<br>

다시 `lib/main.dart` 파일로 돌아오시고요, 파일 상단에 `table_calendar` 라이브러리를 임포트합니다.

```dart
import 'package:table_calendar/table_calendar.dart';
```

<br>

## Stateful/Stateless 위젯

위에서 임포트한  `table_calendar` 라이브러리를 사용하여 `TableCalendar`라는 이름의 Stateful 위젯을 만들어봅시다. Stateful 위젯은 Stateless 위젯과는 달리 State의 값이 바뀔 때마다 바뀐 값을 반영하여 다시 렌더링되는 위젯입니다. React의 `useState`와 유사한 개념이죠. 최상위 위젯인 `MyApp` 클래스가 `StatelessWidget` 클래스를 상속했던 것을 기억하시나요? 네, `MyApp`은 Stateless 위젯입니다.

<br>

Stateful 위젯을 만들기 위해서는 최소 2 개의 클래스가 필요합니다. `StatefulWidget` 그리고 `State` 클래스입니다.
Android Studio 편집기를 사용하신다면 자동완성 기능을 사용할 수 있습니다. `stful`을 입력하고 엔터 키를 눌러보세요. 2 개의 클래스가 만들어지고요, 각각 `StatefulWidget`과 `State` 클래스를 상속하고 있습니다. 첫 번째 클래스의 이름 부분에 커서가 깜빡이나요? `TableCalendar`라고 이름을 작성해보세요. 두 번째 클래스의 이름은 앞에 언더바(`_`)가 붙은 상태로 자동입력됩니다. (`_TableCanlendarState`)

<br>

> 기본적으로 `State` 클래스의 이름은 앞에 언더바(`_`)를 붙여 지정합니다. 위의 `_TableCanlendarState` 클래스처럼요. 이는 Dart에서 프라이버시를 지켜주기 때문에 권장되는 방식입니다.

<br>

코드는 아래와 같습니다.

```dart
class TableCanlendar extends StatefulWidget {
  @override
  _TableCanlendarState createState() => _TableCanlendarState();
}

class _TableCanlendarState extends State<TableCanlendar> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

<br>

이제 State 클래스인 `_TableCanlendarState` 클래스 내부를 [라이브러리 문서](https://pub.dev/packages/table_calendar)를 참고하여 아래와 같이 수정하세요.

```dart
class _TableCanlendarState extends State<TableCanlendar> {

  CalendarController _calendarController;

  @override
  void initState() {
    super.initState();
    _calendarController = CalendarController();
  }

  @override
  void dispose() {
    _calendarController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return TableCalendar(
      calendarController: _calendarController,
    );
  }
}
```

<br>

`TableCalendar` 위젯이 만들어졌습니다! 이제 이 위젯을 사용하기 위해 `MyApp` 클래스로 돌아가보죠.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          title: 'Material App',
          home: Scaffold(
            appBar: AppBar(
              title: Text('Hinoki Calendar')
            ),
            body: Center(
              child: Text('Calendar should be here...')
            )
          )
      )
  }
}

```

<br>

`Scaffold` 위젯의 `body` 속성에 `Center` 위젯이 지정되어 있네요. 달력을 가운데 배치하기 위해 `Center` 위젯은 그대로 사용할거고요, `Center` 위젯의 `child` 속성에 `Text` 위젯을 `TableCalendar` 위젯으로 바꿔봅니다.

<br>

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          title: 'Material App',
          home: Scaffold(
            appBar: AppBar(
              title: Text('Hinoki Calendar')
            ),
            body: Center(
              child: TableCalendar()
            )
          )
      )
  }
}

```

<br>

이제 iOS 시뮬레이터를 사용하여 앱을 실행하면 아래와 같은 화면을 보실 수 있습니다 !

<div style="display: flex; justify-content: center;">
  <img src="./../img/hinoki1.png" alt="스크린 예시" width="400" />
</div>

<br>

## `FloatingActionButton` 위젯

우측 하단의 파란색 버튼은 역시 Material 라이브러리에서 제공하는 위젯입니다. `Scaffold` 위젯 인스턴스를 생성할 때 `appBar`와 `body` 속성을 사용했었죠. `floatingActionButton` 속성을 추가하고 값으로 `FloatingActionButton` 위젯을 지정해보세요. 아래는 State 값을 변경시키는 `_incrementCounter`라는 함수가 있다고 가정한 예시 코드입니다.

> State 값을 변경시키는건 뒤에서 다룹니다.

<br>

```dart
floatingActionButton: FloatingActionButton(
  onPressed: _incrementCounter,
  tooltip: 'Increment',
  child: Icon(Icons.add),
)
```

<br>


## `ListView` 위젯을 사용하여 무한 스크롤 UI 구현하기

이번에는 무한 스크롤 UI를 만들어보겠습니다. 리스트 형태의 UI는 Material 라이브러리의 `ListView` 위젯을 사용합니다. 일단 `Scaffold` 위젯의 `body` 속성에 지정했던 `TableCalendar` 위젯은 잠시 지워보시고요. 대신 `MyList`라는 이름의 위젯을 지정하세요. (아직 이 위젯은 존재하지 않습니다)

```dart
body: MyList()
```

<br>

이제 `MyApp` 클래스에서 벗어나셔서 `TableCalendar` 위젯을 만들었을 때처럼 새로운 Stateful 위젯을 생성하세요. 이 위젯의 이름을 `MyList`로 합니다.

```dart
class MyList extends StatefulWidget {
  @override
  _MyListState createState() => _MyListState();
}

class _MyListState extends State<MyList> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

```

<br>

`_MyListState` 클래스 내에 `ListTile` 위젯을 반환하는 `_buildRow`라는 이름의 함수를 만드세요. `String` 타입을 인자로 받도록 하시고요. `ListTile` 인스턴스에 지정할 속성은 아래 예시 코드를 참고해주세요.

> `Text` 위젯의 두 번째 인자에 style 속성을 지정할 수 있습니다. 이 때 값으로 `TextStyle` 위젯을 사용할 수 있죠.

<br>

```dart
class _MyListState extends State<MyList> {
  // returns ListTile
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

<br>

이제 본격적으로 `build` 메소드 부분을 건드려보겠습니다. 아래와 같이 `ListView` 위젯을 반환하도록 작성하시고요.

```dart
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      // ...
    );
  }
```


<br>

`padding` 속성을 사용해서 여백 크기를 지정할 수 있습니다.

```dart
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      padding: EdgeInsets.all(16.0)
    );
  }
```

<br>

`itemBuilder` 속성은 리스트 UI의 각 아이템 부분에 무엇을 보여줄지 결정합니다. 위에서 만들었던 `_buildRow` 함수를 사용해보죠. `ListTile` 위젯을 반환하는 함수였죠. 

```dart
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      padding: EdgeInsets.all(16.0),
      itemBuilder: (context, i) {
        return _buildRow('This is a row.');
      }
    );
  }
```

<br>

자 이제 간단한 `MyList` 위젯이 완성되었습니다. 여기까지 하고 앱을 실행시켜볼까요? 시뮬레이터 내에서 마우스를 사용하여 스크롤해보세요.

> `Scaffold` 위젯의 `body` 속성에 `MyList` 위젯을 지정해야 합니다.

<br>

<img src="./../img/hinoki2.png" alt="flutter app" width="400" />

<br>

## `itemBuilder` 속성으로 콜백 알아보기

`ListView.builder`의 `itemBuilder` 속성을 사용하여 리스트의 아이템마다 다른 내용을 보여주도록 할 수 있습니다. `itemBuilder` 속성은 JavaScript의 `map` 메소드처럼 작동합니다. 콜백 함수의 인자로 컨텍스트와 인덱스를 받고 최종적으로 남길 내용을 반환하는 방식이죠.

<br>

`_rowItems`라는 이름의 배열을 만들고, 배열의 요소들을 원하는 값으로 채워주세요. 단, `_buildRow` 함수를 사용하기 위해 요소들의 값은 `String` 타입을 지켜줍니다. 먼저, 콜백의 두 번째 인자인 인덱스 값(`i`)을 사용해서 `_rowItems` 배열의 값들을 콘솔에 출력해볼게요.

```dart
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  final _rowItems = ['Yujin', 'Yongki', 'Yubin', 'Donghyun'];

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      padding: EdgeInsets.all(16.0),
      itemBuilder: (context, i) {
        if (i < 4) print(_rowItems[i]);
        return _buildRow('This is a row.');
      }
    );
  }
```

<br>

아래와 같이 콘솔에 내용이 출력됩니다.

```bash
flutter: Yujin
flutter: Yongki
flutter: Yubin
flutter: Donghyun
```

<br>

이제 무한 스크롤 UI를 위해 코드를 조금 수정해볼게요. `_rowItems`의 초기값을 빈 배열(`[]`)로 수정하시고요, `itemBuilder` 속성의 콜백 함수에서 인덱스 `i`의 값이 `_rowItems` 배열의 길이 이상일 때 `['Yujin', 'Yongki', 'Yubin', 'Donghyun']`를 추가해주세요. 기존 배열에 새로운 배열의 요소들을 추가하는 경우이므로 `addAll()` 메소드를 사용하시면 됩니다. JavaScript의 `concat()` 메소드와 비슷하죠.

```dart
  Widget _buildRow(String text) {
    return ListTile(
      title: Text(text, style: TextStyle(fontSize: 18.0))
    )
  }

  final _rowItems = [];

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      padding: EdgeInsets.all(16.0),
      itemBuilder: (context, i) {
        if (i >= _rowItems.length) {
          _rowItems.addAll(['Yujin', 'Yongki', 'Yubin', 'Donghyun'].cast<String>());
        }
        return _buildRow(_rowItems[i]);
      }
    );
  }
```

<br>

이제 앱을 실행하고 스크롤을 해보세요.

<img src="./../img/hinoki3.png" alt="Flutter app" width="400" />


<br>


---

### References

- [Write your first Flutter app, part 1](https://flutter.dev/docs/get-started/codelab)
