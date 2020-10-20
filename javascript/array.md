# 인덱스 컬렉션(Indexed Collections) - 배열(Array)

<br>

## 인덱스 컬렉션(Indexed Collections)

아래는 [자바스크립트 표준 내장 객체](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) 중에서 인덱스 컬렉션(Indexed Collections)에 해당하는 객체들입니다. 인덱스 값으로 정렬된 데이터의 컬렉션(집합)을 나타내고요, 배열([형식화 배열(Typed Array)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays) 포함)과 배열형 객체를 포함합니다.

- `Array`

- `Int8Array`

- `Uint8Array`

- `Uint8ClampedArray`

- `Int16Array`

- `Uint16Array`

- `Int32Array`

- `Uint32Array`

- `Float32Array`

- `Float64Array`

- `BigInt64Array`

- `BigUint64Array`

<br>

## `Array`

`Array`는 리스트 형태의 객체입니다. 배열을 생성할 때 사용할 수 있고요, `Array`의 프로토타입은 배열을 탐색하고 변형시키는 메소드들을 제공합니다. JavaScript에서 배열은 아래와 같은 특징을 가집니다.

- 길이가 고정되어 있지 않습니다.

- 배열 요소들의 타입이 고정되어있지 않습니다.

<br>

무슨 말이냐면, 배열의 길이가 언제든 인위적으로 변경될 수 있기 때문에 비어있는 요소가 존재할 수 있다는 말입니다. 가령 아래와 같은 배열이 가능한데요, 실제 존재하는 데이터는 4개이지만 이 배열의 길이를 확인해보면 값이 `9`입니다. 이처럼 JavaScript의 배열은 밀집도를 보장하지 않습니다. 이러한 특징이 적합하지 않은 상황에서는 형식화 배열(Typed array)를 사용하세요.

```javascript
const arr = [1, 2, 3, 4, , , , ,];
console.log(arr.length); // output : 7
```

> 위와 같이 배열 객체는 변수에 담아서 사용할 수 있고요, 배열의 인덱스 값은 정수입니다. `arr[0]`으로 위 배열의 첫 번째 요소에 접근할 수 있습니다. 

<br>

또한, 아래와 같이 하나의 배열에 서로 다른 타입을 가진 요소들을 담을 수 있습니다. 배열의 각 요소에는 거의 모든 것을 저장할 수 있습니다. 문자열, 숫자, 객체, 다른 변수, .., 심지어 다른 배열도요.

```javascript
const arr2 = [7, "Hello", true, ["a", "b", "c"]];
```

<br>

### 다중배열

배열 내부의 배열은 다중배열이라고 하는데요, 대괄호 두개를 함께 연결하여 다른 배열 안에 있는 배열의 요소에 접근 할 수 있습니다. 위의 `arr2` 배열 내부에 속해있는 배열 `["a", "b", "c"]`의 마지막 요소인 `"c"`에 접근하려면 다음과 같은 방법이 유효합니다.

```javascript
const c = arr2[3][2];
console.log(c); // output: "c"
```

<br>

## 배열 생성하기 - 리터럴과 생성자 함수

배열을 생성하는 방법에는 두 가지가 있습니다.

- [배열 리터럴(Array literals)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals)

- [`Array()` 생성자 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)

<br>

### 배열 리터럴 표기법

아래와 같이 배열을 생성하면 배열 리터럴 표기법을 사용한 것입니다. 리터럴 표기법으로 배열을 생성하면 명시한 값들로 배열이 초기화되고요, 명시된 요소들의 수가 해당 배열의 길이가 됩니다. 네, 필요한 요소들을 열거하는 것만으로 배열 객체가 생성됩니다.

```javascript
let cities = ["Seoul", "New York", "Barcelona"];
```

> 리터럴(Literal)은 변수에 할당하는 고정 형태의 값을 말합니다. 작성한 문자 그대로 자신을 나타내는 값이 됩니다.

<br>

### `Array()` 생성자 함수

`Array()`는 생성자 함수입니다. `Array` 객체의 생성과 초기화를 담당하는 함수죠.

```javascript
let a = new Array(); // []
```

<br>

하나의 숫자(정수) 인자를 넘겨주면 그 숫자만큼의 길이를 가지는 배열이 생성됩니다. 하지만 이렇게 생성된 배열은 요소가 없는 빈 배열입니다.

```javascript
let b = new Array(3); // [empty × 3]
console.log(b.length); // output : 3
```

<br>

2 개 이상의 인자를 넘겨주면 각 인자가 생성된 배열의 요소들로 초기화됩니다. 넘겨준 인자의 수가 배열의 길이가 됩니다.

```javascript
let c = new Array(1, 2); // [1, 2]
```

<br>

### 리터럴 vs 생성자

리터럴과 생성자 함수. 둘 중 어느 방법이 더 좋은가요? 이 질문에 대해서는 널리 알려진 답(?)이 있습니다. [Oreilly사의 JavaScript Patterns](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=13680905)에서 소개된 내용이고, 이 책의 내용을 레퍼런스로 하는 블로그 글도 많습니다.

결론적으로, 리터럴 패턴을 사용하는 것이 더 좋은 방법입니다. 많은 사람들이 이에 동의하는 근거는 다음과 같습니다.

- 리터럴 패턴이 더 직관적이고 코드도 깔끔합니다.

```javascript
// 생성자
let arr = new Array();

// 리터럴
let arr = [];
```

<br>

---

### References

- [JavaScript reference | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [Standard built-in objects | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
- [Array | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [Grammar and types | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals)
- [Array() constructor | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)
