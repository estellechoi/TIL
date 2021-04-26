# Flutter 위젯(Widget)

> [Flutter](https://flutter.dev/docs/get-started/codelab) 공식문서를 요약/번역한 튜토리얼 문서입니다.

<br>

## 위젯(Widget)

Flutter에서 UI 레이아웃은 위젯을 사용하여 구현합니다. Flutter에서는 거의 모든 것이 위젯입니다. 이미지, 아이콘, 텍스트, 눈에 보이지 않는 박스 모델, 그리드(Grid) 등 모든 구성요소가 위젯이라고 봐도 무방하죠.

<br>

## 레이아웃 위젯

레이아웃 구성에 최적화된 위젯들을 [레이아웃 위젯](https://flutter.dev/docs/development/ui/widgets/layout)이라고 합니다. 레이아웃 위젯들은 눈에 보이지 않습니다. 뼈대를 세우는 일을 하죠. HTML 문서에서 CSS 없이 마크업만 하는 것과 비슷합니다.

<br>

## 기본 위젯

### [`GestureDetector`](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html) : 제스쳐 핸들링

`GestureDetector` 위젯은 시각적인 형태를 갖지 않고, 사용자의 제스쳐를 감지하는 역할만 수행합니다. 어떤 위젯에 대한 사용자의 제스쳐를 감지하고 싶다면 그 위젯을 `GestureDetector` 위젯으로 감싸면 됩니다. 예를 들어 사용자가 `GestureDetector` 위젯의 자식 위젯을 탭(`tap`)하면 `onTap()` 콜백이 호출됩니다. HTML의 이벤트 핸들러에 대응되는 개념입니다.

```dart
GestureDetector(
  onTap: () {
    print('Tapped!');
  },
  child: Container(
    height: 36.0,
    padding: const EdgeInsets.all(8.0),
    margin: const EdgeInsets.symmetric(horizontal: 8.0),
    decoration: BoxDecoration(
      borderRadius: BorderRadius.circular(5.0),
      color: Colors.lightGreen[500],
    ),
    child: Center(
      child: Text('Tap'),
    ),
  ),
)
```

<br>

`GestureDetector` 위젯을 기본으로 탑재한 위젯들도 많습니다. `IconButton`, `ElevatedButton`, `FloatingActionButton` 등의 버튼 위젯들이 그렇고요. 이런 위젯들은 `onPressed()` 콜백을 제공합니다. Flutter에서 다루는 제스쳐들에 대한 자세한 설명은 [Taps, drags, and other gestures
](https://flutter.dev/docs/development/ui/advanced/gestures) 에서 확인하실 수 있습니다.

<br>

### [`Text`](https://api.flutter.dev/flutter/widgets/Text-class.html)

텍스트를 입력하고 스타일링할 수 있습니다. 줄바꿈이 가능합니다.

<br>

#### `Text`

```dart
Text(
  'Hello World!',
  textAlign: TextAlign.center,
  overflow: TextOverflow.ellipsis,
  style: TextStyle(fontWeight: FontWeight.bold),
)
```

#### `Text.rich`

```dart
const Text.rich(
  TextSpan(
    text: 'Hello', // default text style applied
    children: <TextSpan>[
      TextSpan(text: ' beautiful ', style: TextStyle(fontStyle: FontStyle.italic)),
      TextSpan(text: 'world', style: TextStyle(fontWeight: FontWeight.bold)),
    ],
  ),
)
```

<br>

#### 상호작용

- `Text` 위젯을 탭할 수 있게 하려면 [`GestureDetector`](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html) 위젯으로 감싸고, `GestureDetector.onTap` 핸들러 속성을 사용하세요.

- Material 라이브러리를 사용하신다면 [`TextButton`](https://api.flutter.dev/flutter/material/TextButton-class.html) 위젯을 권장하고요, `Text` 위젯이 꼭 필요하다면 `GestureDetector` 대신 [`InkWell`](https://api.flutter.dev/flutter/material/InkWell-class.html) 위젯을 사용하세요.

- 여러 스타일의 텍스트로 이루어진 섹션을 구현하려면 [`RichText`](https://api.flutter.dev/flutter/widgets/RichText-class.html) 위젯을 사용하여 해당 섹션의 트리구조를 관리하세요. 원하는 부분만 스타일링하기 편합니다. 사용자가 `RichText` 위젯의 특정 부분과 상호작용할 수 있게 하려면 `TextSpan.recognizer` 생성자를 사용하여 [`TapGestureRecognizer`](https://api.flutter.dev/flutter/gestures/TapGestureRecognizer-class.html)를 적용하세요.

<br>

### [`Row`](https://api.flutter.dev/flutter/widgets/Row-class.html)

자식 위젯들을 수평으로 배치합니다. CSS의 Flex Box 모델과 비슷합니다. `MainAxixAlignment`와 `CrossAxisAlignment` 속성을 사용하여 자식 위젯들을 어떻게 정렬할지 결정합니다. `Align`/`Center` 위젯을 사용하여 아이템들을 정렬할 수도 있습니다. 자식 위젯이 열의 남은 공간을 채우게 하고 싶다면 `Expanded` 위젯으로 해당 위젯을 감싸면 됩니다.

```dart
Row(
  children: <Widget>[
    Expanded(
      child: Text('Deliver features faster', textAlign: TextAlign.center),
    ),
    Expanded(
      child: Text('Craft beautiful UIs', textAlign: TextAlign.center),
    ),
    Expanded(
      child: FittedBox(
        fit: BoxFit.contain, // otherwise the logo will be tiny
        child: const FlutterLogo(),
      ),
    ),
  ],
)
```

<br>

#### 상호작용

- `Row` 위젯은 스크롤할 수 없습니다. 스크롤하려면 `ListView` 위젯을 사용하거나 `SingleChildScrollView` 위젯으로 `Row` 위젯을 감싸야 합니다.

<br>

### [`Column`](https://api.flutter.dev/flutter/widgets/Column-class.html)

자식 위젯들을 수직으로 배치합니다. `Row` 위젯과 똑같이 작동하지만 자식 위젯들을 배치하는 방향이 다릅니다.

<br>

```dart
Column(
  children: <Widget>[
    Text('Deliver features faster'),
    Text('Craft beautiful UIs'),
    Expanded(
      child: FittedBox(
        fit: BoxFit.contain, // otherwise the logo will be tiny
        child: const FlutterLogo(),
      ),
    ),
  ],
)
```

<br>

#### 상호작용

- `Column` 위젯 역시 스크롤할 수 없습니다. `ListView` 위젯을 사용하거나 `SingleChildScrollView` 위젯으로 감싸세요.

<br>

### [`Stack`](https://api.flutter.dev/flutter/widgets/Stack-class.html)

자식 위젯을 Z축에 쌓을 때 사용합니다. 여러 위젯들을 겹치게 배치해야할 때 유용하고요, 자식 위젯들은 기본적으로 박스 모서리 기준으로 배치됩니다. `Stack` 위젯은 모든 자식들을 포함하기 위해 자신의 크기를 자동으로 조절합니다. 첫 번째 자식이 가장 아래에 쌓이게 되고요. CSS의 `position` 개념과 매우 비슷합니다. 위젯간 쌓임 알고리즘에 대해 더 알고싶으시면 [`RenderStack` 문서](https://api.flutter.dev/flutter/rendering/RenderStack-class.html)를 확인하세요.

<br>

`Stack` 위젯의 모든 자식 위젯은 `positioned`/`non-positioned` 2 가지 상태를 가집니다. `Positioned` 위젯으로 감싸진 자식은 `positioned` 상태가 되죠. `positioned` 상태의 자식들은 `Stack` 위젯의 각 모서리를 기준으로 지정된 위치에 배치됩니다.

```dart
Stack(
  children: <Widget>[
    Container(
      width: 100,
      height: 100,
      color: Colors.red,
    ),
    Container(
      width: 90,
      height: 90,
      color: Colors.green,
    ),
    Container(
      width: 80,
      height: 80,
      color: Colors.blue,
    ),
    Positioned(
      bottom: 0,
      right: 0,
      chlid: Container(
        width: 80,
        height: 80,
        color: Colors.blue,
      )
    )
  ],
  clipBehavior: Clip.none // overflow handling
)
```

<br>

> [공식문서](https://api.flutter.dev/flutter/widgets/Stack-class.html)에서 그래디언트 스타일의 박스에 텍스트를 배치한 예시를 확인해보세요.

<br>

### [`Container`](https://api.flutter.dev/flutter/widgets/Container-class.html)

기본적인 박스 위젯입니다. `margin`/`padding` 속성을 사용하여 여백을 지정할 수 있고, `decoration` 속성에 `BoxDecoration` 위젯을 사용하여 스타일링 할 수 있습니다. 자식 위젯을 가지는 `Container` 위젯은 자식들을 둘러싼 최소 사이즈로 자신의 사이즈를 맞춥니다. `width`/`height` 속성으로 원하는 사이즈를 직접 지정할 수도 있습니다.

```dart
Container(
  height: 56.0, // in logical pixels
  padding: const EdgeInsets.symmetric(horizontal: 8.0),
  alignment: Alignment.center,
  decoration: BoxDecoration(color: Colors.blue[500]),
  transform: Matrix4.rotationZ(0.1),
  child: Row(
    // <Widget> is the type of items in the list.
    children: <Widget>[
      IconButton(
        icon: Icon(Icons.menu),
        tooltip: 'Navigation menu',
        onPressed: null, // null disables the button
      ),
      Expanded(
        child: title,
      ),
      IconButton(
        icon: Icon(Icons.search),
        tooltip: 'Search',
        onPressed: null,
      ),
    ],
  ),
)
```

<br>

---

### References

- [Introduction to widgets](https://flutter.dev/docs/development/ui/widgets-intro)
- [Layouts in Flutter](https://flutter.dev/docs/development/ui/layout)
