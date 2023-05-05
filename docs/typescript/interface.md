# Type과 Interface

1. Type과 Inteface의 차이?
2. Interface
3. Interface 속성 체크
4. Interface 확장
5. Type Alias와의 차이점

<br>

## 1. Type과 Inteface의 차이?

TypeScript(이하 TS)에서 `type`과 `interface`의 차이가 무엇이냐는 질문을 받았습니다. 늘 TS가 너무 좋다고 생각했으면서, 이 간단해보이는 질문에 적절한 답을 하지 못했습니다. 🫥 기본적으로 `type`은 Type Alias로서의 역할을 하는 것인데, 그래서 `interface`로 정의할 때와 사용 측면에서 무엇이 다른지 정리해보려고합니다. 먼저 Interface의 정의와 규칙들을 제대로 확인하는 것으로 시작해보겠습니다.

<br>

## 2. Interface

[TS 공식문서](https://www.typescriptlang.org/docs/handbook/2/objects.html)에 따르면 Interface의 정의는 이렇습니다: TS에서 값의 형태를 체크할 때, Interface는 그 형태를 네이밍하는 역할을 하고, 개발자가 작성하는 코드, 혹은 외부 코드에서 일종의 계약을 정의하는 방법입니다. 객체 자체를 정의하기보다, 객체가 최소 어떤 속성들을 포함해야하는지 Requirement를 정의한다는 면에서 계약이라는 워딩을 사용한 것으로 추측해봅니다.

> One of TypeScript’s core principles is that type checking focuses on the shape that values have. This is sometimes called “duck typing” or “structural subtyping”. In TypeScript, interfaces fill the role of naming these types, and are a powerful way of defining contracts within your code as well as contracts with code outside of your project.

<br>

다음과 같은 말이 덧붙여져있었기 때문입니다: TS 컴파일러는 최소한 있어야하는 속성들이 있는지만, 그리고 그들의 타입이 요구된 타입인지만 검사합니다.

> Notice that our object actually has more properties than this, but the compiler only checks that at least the ones required are present and match the types required.

<br>

[TypeScript Playground](https://www.typescriptlang.org/play)에서 확인해보면, v5에서 위의 설명대로 동작합니다.

<br>

다음과 같이 Optional 속성들도 정의할 수 있습니다.

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}
```

<br>

처음 값이 생성된 후 변경되면 안되는 속성들도 정의할 수 있습니다.

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}
```

<br>

`ReadonlyArray` 타입을 사용해서 Array도 변경되지 않도록 타이핑할 수 있습니다.

```typescript
let arr: ReadonlyArray<number> = [1, 2, 3, 4];

arr[0] = 12; // error
arr.push(5); // error
arr.length = 100; // error
```

<br>

주의할만한 점은 TS의 `readonly` 선언이 객체의 속성들을 통제하는 방법이라는 것입니다. 따라서 `arr`의 속성들을 변경하는 것이 아니라 아예 새로운 값을 할당하는 경우는 체크하지 않습니다.

```typescript
arr = [5, 6, 7];
console.log(err); // [5, 6, 7]
```

<br>

새로운 값을 할당할 수 없게하고, 배열 메소드로 속성 변경까지 못하게 하려면 다음과 같이 `const` 변수 선언과 TS의 `ReadonlyArray` 타이핑을 함께 사용해야 합니다.

```typescript
const arr: ReadonlyArray<number> = [1, 2, 3, 4];

arr = [5, 6, 7]; // error
```

<br>

## 3. Interface 속성 체크

Interface는 분명 Required 속성들만 체크하고, 이 외 속성에 대해서는 체크하지 않기 때문에 정의되지 않은 속성들이 포함되어 있어도 허용한다고 했습니다. 그런데 엄격해지는 경우도 있는데요, 객체 리터럴 값을 할당하려고 할 때는 정의되지 않은 속성이 포함되었는지 체크합니다. TS 문서를 읽어보면 이것은 Interface의 규칙이라기보다, 버그로 간주하고 추가적으로 경고를 주는 것으로 보입니다.

```typescript
interface Person {
  name: string;
  age: number;
}

const person: Person = { name: "Yujin", age: 25, city: "Seoul" }; // error!
```

<br>

그런데 이렇게 변수를 할당할 때는, 원래 Interface 규칙대로 정의되지 않은 속성을 허용합니다.

```typescript
const yujin = { name: "Yujin", age: 25, city: "Seoul" };

const person: Person = yujin; // ok
```

<br>

## 4. Interface 확장

Interface를 확장하는 방법은 `extends`를 사용하는 것입니다.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Member extends Person {
  city: string;
}
```

<br>

이것은 Type Alias를 사용할 때도 똑같습니다.

```typescript
type Person = {
  name: string;
  age: number;
};

type Member = Person & {
  city: string;
};
```

<br>

## 5. Type Alias와의 차이점

[공식문서](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)에 너무 명쾌하게 명시되어 있었습니다. 아마 그간 많은 개발자들이 질문하고 얘기해온 주제이기 때문인 것 같습니다. 그 내용은 바로, Type Alias는 Interface의 기능을 거의 모두 동일하게 제공하지만, Interface처럼 선언적 확장을 할 수 없다는 것이었습니다.

> Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an interface are available in type, the key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.

<br>

### 5-1. Type Alias로는 못하는 선언적 확장

Interface는 선언적 확장(Declaration merging)이 가능합니다. 이게 무슨 말이냐면, 다음과 같이 같은 이름으로 다시 선언함으로써 속성들을 확장해갈 수 있습니다.

```typescript
interface Person {
  name: string
}

interface Person {
  age: number
}

const person: Person = { name: 'Yujin'; age: 25 }; // ok
```

<br>

그런데 `type`은 그게 안됩니다. 애초에 역할이 객체 타입이 아니라, Alias이므로 당연히 하나의 명확한 타입을 나타내야하기 때문일 것 같습니다.

```typescript
type Person = {
  name: string;
};

type Person = {
  age: number;
};

// Duplicate identifier 'Person'.(2300)
```

<br>

### 5-2. Type Alias는 Interface 외에도

`type`은 애초에 `interface`로 타이핑하는 객체 외에도, 모든 타입에 대해 Alias로서 역할을 합니다.

```typescript
type Alignment = "left" | "center" | "right";
```

<br>

[Type Alias의 공식문서 정의](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)는 이렇습니다: (객체뿐만 아니라) 모든 타입에 대한 (새로운) 이름입니다.

> A type alias is exactly that - a name for any type.

<br>

---

### References

- [Everyday Types | TypeScript](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [Object Types | TypeScript](https://www.typescriptlang.org/docs/handbook/2/objects.html)
