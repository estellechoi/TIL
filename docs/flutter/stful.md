# Stateful / Stateless 위젯

> [Flutter](https://flutter.dev/docs/get-started/codelab) 공식문서를 요약/번역한 튜토리얼 문서입니다.

<br>

## Stateful 위젯

State 위젯은 State 값이 변할 때마다 `build()` 메소드가 실행됩니다. 아래는 [`ElevatedButton`](https://api.flutter.dev/flutter/material/ElevatedButton-class.html) 위젯을 사용한 예제이고요. 사용자가 `ElevatedButton` 위젯을 누르면 `_increment()` 메소드가 실행되고, 따라서 `setState()` 메소가 실행되어 위젯이 다시 빌드됩니다. 이때 변경된 `_counter` 값이 반영되죠.

```dart
class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      children: <Widget>[
        ElevatedButton(
          onPressed: _increment,
          child: Text('Increment'),
        ),
        Text('Count: $_counter'),
      ],
    );
  }
}
```

<br>

아래의 Stateful 위젯은 위의 State 위젯에 대한 설정 클래스 정도로 볼 수 있습니다. 부모 위젯으로부터 받은 값과 State 위젯이 빌드될 때 사용되는 값들을 갖고있죠. Flutter에서 State 위젯과 Stateful 위젯이 나뉘어져 있는 이유는 두 위젯의 라이프사이클이 다르기 때문입니다. 사실상 일반적인 Stateful/Stateless 위젯은 `build()` 메소드가 실행될 때 사용자의 디바이스에서 UI를 구성하는 일시적 객체입니다. 반면 State 위젯은 `build()` 메소드가 실행되지 않을 때도 존재하면서 필요한 정보들을 저장하고 있죠.

```dart
class Counter extends StatefulWidget {
  // Fields in a Widget subclass are always marked "final".
  @override
  _CounterState createState() => _CounterState();
}
```

<br>

## Stateless 위젯

State 위젯에서 `build()` 메소드가 실행되면(변경이 감지되면) Stateful 위젯으로 그 내용이 전파됩니다. 이때 Stateful 위젯은 변경을 반영합니다. 마치 콜백처럼 작동하는거죠. 반면, State 값 자체는 자식 위젯에게 전파되어 자식 위젯이 변경된 값을 반영하도록 합니다. 자식 위젯은 필요한 값을 단순히 인자로 받아쓰기 때문에 Stateless 위젯을 사용합니다. `_CounterState` 클래스를 수정하면서 이 내용을 살펴보겠습니다.

<br>

`Row` 위젯의 자식들을 `CounterIncrementor` 위젯과 `CounterDisplay` 위젯으로 변경해봅시다. 기존의 `ElevatedButton`, `Text` 위젯은 각각 이 두 위젯이 다시 반환해줄거고요, 이 두 위젯은 아직 존재하지 않습니다.

```dart
class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      // 자식들을 새로운 위젯들로 변경합니다.
      children: <Widget>[
        // ElevatedButton -> CounterIncrementor
        CounterIncrementor(
          onPressed: _increment
        ),
        // Text -> CounterIncrementor
        CounterDisplay(
          count: _counter
        )
      ],
    );
  }
}
```

<br>

`CounterIncrementor` 위젯을 Stateless 위젯으로 만듭니다. State 값의 변경이 없고, 필요한 값을 인자로 받기 때문이죠. 기존에 사용했던 `ElevatedButton` 위젯을 반환하고요, `ElevatedButton` 위젯의 `onPressed` 콜백은 `CounterIncrementor` 위젯 인스턴스를 생성할 때 인자로 받도록 합니다.

```dart
class CounterIncrementor extends StatelessWidget {
  CounterIncrementor({this.onPressed});

  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text('Increment'),
    );
  }
}
```

<br>

`CounterDisplay` 위젯도 Stateless 위젯으로 만듭니다. 기존의 `Text` 위젯을 반환하고요, `count`를 인자로 받도록 합니다. `count`는 `_CounterState` 위젯에서 `setState()` 메소드에 의해 변경되는 값이죠.

```dart
class CounterDisplay extends StatelessWidget {
  CounterDisplay({this.count});

  final int count;

  @override
  Widget build(BuildContext context) {
    return Text('Count: $count');
  }
}
```

<br>

위 내용들을 종합하여 간단한 장바구니 기능을 구현할 수 있습니다. 장바구니 리스트에 포함된 하나의 아이템을 담을 위젯을 구현해보죠. 필요한 값들을 모두 인자로 받아 사용할 것이기 때문에 Stateless 위젯으로 만들면 됩니다.

```dart
class ShoppingListItem extends StatelessWidget {
  // 인스턴스 생성시 필요한 값들을 인자로 받습니다.
  ShoppingListItem({ this.product, this.inCart, this.onCartChange}): super(key: ObjectKey(product));

  // 인자로 받는 값들은 final 로 선언합니다.
  final Product product;
  final bool inCart;
  final CartChangedCallback onCartChange;

  // 장바구니에 담겼으면 black54, 그렇지 않으면 기본 테마 색을 반환하는 메소드를 만드시고요.
  Color _getColor(BuildContext context) {
    return inCart ? Colors.black54 : Theme.of(context).primaryColor;
  }

  // 장바구니에 담기지 않았으면 null, 장바구니에 담겼으면 TextStyle 위젯을 반환하는 메소드도 만들고요.
  TextStyle _getTextStyle(BuildContext context) {
    if (!inCart) return null;

    return TextStyle(
      color: Colors.black54,
      decoration: TextDecoration.lineThrough
    )
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      // 위젯을 탭하면 인자로 받은 콜백을 실행합니다.
      onTap: () {
        onCartChange(product, inCart);
      },
      // 인자로 받은 inCart 값에 따라 배경색이 정해지도록 합니다.
      leading: CircleAvatar(
        backgroundColor: _getColor(context),
        child: Text(product.name[0]) // ?
      ),
      // 인자로 받은 inCart의 값이 false 이면 style 속성의 값은 null이 됩니다.
      title: Text(product.name, style: _getTextStyle(context))
    )
  }

}
```

<br>

사용자가 아이템을 탭하면 `ListTile` 위젯의 `onTap()` 메소드가 실행됩니다. `inCart`의 값을 직접적으로 변경하지 않고, 인자로 받은 `onCartChange()` 콜백을 실행시켜서 상위 위젯에서 State 값이 바뀌도록 합니다. 상위 위젯은 State 값을 저장해야하므로 Stateful 위젯이겠죠. 상위 위젯은 변경된 State 값을 저장함과 동시에 자신의 `build()` 메소드를 다시 실행시킵니다. 이때 `ShoppingListItem` 위젯의 인스턴스가 다시 생성되기 때문에 바뀐 값들을 인자로 받게되죠. React의 `prop` 개념과 매우 유사합니다. 상위 위젯은 아마 아래와 같을겁니다.

```dart
class ShoppingList extends StatefulWidget {
  // 상위 위젯으로부터 전체 상품 리스트 데이터를 인자로 받습니다.
  ShoppingList({ key key, this.products }): super(Key: key);

  // 이 값은 _ShoppingListState 에서 widget.products 로 접근합니다.
  final List<Product> products;

  @override
  _ShoppingListState createState() => _ShoppingListState();
}

class _ShoppingListState extends State<ShoppingList> {
  // 장바구니 리스트를 저장할 Product 타입의 셋을 선언합니다.
  Set<Product> _shoppingCart = Set<Product>;

  // 카트에 없는 상품이면 _shoppingCart 셋에 추가하고, 이미 있는 상품이면 제거하는 메소드입니다. 하위 위젯의 콜백으로 사용할 메소드이죠.
  void _handleCartChanged(Product product, bool inCart) {
    setState(() {
      if (!inCart)
        _shoppingCart.add(product);
      else
        _shoppingCart.remove(product);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Shopping List')
      ),
      body: ListView(
        padding: EdgeInsets.symmetric(vertical: 8.0),
        // products는 ShoppingList 클래스에서 상위 위젯으로부터 인자로 받았던 데이터입니다.
        children: widget.products.map((Product product) {
          // ShoppingListItem은 Stateless 위젯이죠. 필요한 값들은 모두 인자로 받아서 사용합니다. 제스쳐 콜백까지도요.
          return ShoppingListItem(
            product: product,
            inCart: _shoppingCart.contains(product),
            onCartChanged: _handleCartChanged
          );
        }).toList()
      )
    );
  }
}
```

<br>

## Stateful 위젯의 `createState()`/`dispose()` 메소드

### `createState()`

`ShoppingList` 위젯의 `createState()` 메소드는 이 위젯이 처음 생성될 때 한 번 호출됩니다. `_ShoppingListState` 위젯의 인스턴스를 생성하죠. 상위 위젯에서 빌드를 재실행하여 `ShoppingList` 위젯의 새 인스턴스를 생성하더라도, `createState()` 메소드는 더이상 호출되지 않습니다. 대신 이전에 생성된 `_ShoppingListState` 인스턴스를 재사용하죠. 그래도 `_ShoppingListState` 내에서 새로 생성된 `ShoppingList` 인스턴스의 속성 값들은 반영됩니다. `widget` 객체로 `ShoppingList`의 속성 값들을 참조하기 때문입니다.

<br>

`createState()` 메소드가 호출되면 Flutter는 State 정보를 저장할 새로운 객체를 생성하여 트리에 추가하고, `initState()` 메소드를 호출합니다. `initState()` 메소드는 오버라이드(`@override`)하여 원하는 작업을 추가할 수 있습니다. 애니메이션을 설정할 때 유용합니다. Implementations of initState are required to start by calling `super.initState`.

<br>

### `dispose()`

State 정보를 저장하고 있는 객체가 더이상 필요하지 않을 때, Flutter는 `dispose()` 메소드를 호출합니다. `dispose()` 메소드를 오버라이드하여 원하는 작업을 추가하세요.

<br>

## State 위젯의 `didUpdateWidget()` 메소드

`widget` 객체의 값이 변경되는 것을 감시하고 싶으면 [`didUpdateWidget()`](https://api.flutter.dev/flutter/widgets/State-class.html#didUpdateWidget) 메소드를 사용하세요.

<br>

## 키(Key)

[키(Key)](https://api.flutter.dev/flutter/foundation/Key-class.html)는 같은 타입의 위젯 인스턴스를 다수 빌드해야하는 위젯에서 유용합니다. 예를 들어 위의 `ShoppingList` 위젯은 `ListView` 내에서 `ShoppingListItem` 위젯들을 여러개 빌드해야하죠. 여러 개의 같은 타입의 위젯들 각각에 고유한 키(Key) 값을 부여해서 싱크를 돕습니다. 반면, [전역 키(Global Key)](https://api.flutter.dev/flutter/widgets/GlobalKey-class.html)는 모든 위젯들을 통틀어 단 하나의 유일한 값을 가져야 유효합니다. 특정한 단 하나 위젯의 State 정보에 접근할 때 사용될 수 있습니다.

<br>

---

### References

- [Introduction to widgets](https://flutter.dev/docs/development/ui/widgets-intro)
- [Layouts in Flutter](https://flutter.dev/docs/development/ui/layout)
