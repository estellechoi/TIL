# 스코프(Scope)

## 스코프(Scope)

스코프(Scope)는 참조 대상 식별자(Identifier)를 찾아내기 위한 규칙입니다. 모든 변수(함수)는 자신이 어디에서 선언되었는지에 따라 유효한 범위, 스코프를 갖습니다. 유효한 범위란, 다른 코드에서 자신을 참조할 수 있는 범위를 말합니다.

> 참조 대상 식별자(Identifier) : 변수(함수)를 참조할 때 변수의 이름과 같이 참조하고자 하는 대상을 다른 대상과 구분하여 식별할 수 있는 유일한 식별자

<br>

이게 무슨 말이냐면, 아래의 코드처럼 똑같은 이름을 가진 변수가 2번 선언되었을 때 각 변수의 스코프에 따라 둘 중 어떤 변수의 식별자를 참조해야 할지 결정한다는 뜻입니다.

```javascript
var str = "string";

function change() {
	var str = "change";
	console.log(str);
}

change(); // output: 'change'
console.log(str); // output: 'string'
```

<br>

위 코드에서 전역에 선언된 변수 `str`은 어디에서든 참조할 수 있는 스코프를 가집니다. 하지만 함수 `change` 내에서 선언된 변수 `str`은 자신이 선언된 함수 `change` 내에서만 참조할 수 있는 스코프를 가집니다. 이 함수의 외부에서는 스코프를 벗어나므로 참조할 수 없습니다.

<br>

> 만약 스코프라는 개념이 없다면, 위의 두 변수는 `str`이라는 똑같은 이름을 가지므로 충돌이 발생할 겁니다. 이런 경우, `str`이라는 이름은 아주 아주 규모가 큰 프로그램에서도 단 한 번만 사용할 수 밖에 없겠죠.

<br>

## JavaScript의 스코프

JavaScript에서 스코프를 크게 두 가지로 분류할 수 있습니다.

- 전역 스코프(Global scope) : 코드 어디에서든 참조 가능한 범위

- 지역 스코프(Local scope) : 함수 또는 코드 블록(`{ .. }`) 내에서만 참조 가능한 범위, 함수/블록 자신과 하위 함수/블록에서만 참조 가능

  > JavaScript에서 지역 스코프는 기본적으로 함수 레벨 스코프(Function-level scope) 입니다. 다만, ES6에서 도입된 `let`, `const` 키워드로 선언된 변수는 블록 레벨 스코프(Block-level scope)를 따릅니다.

<br>

위와 같은 관점에서 변수도 전역 변수와 지역 변수로 나눌 수 있습니다.

<br>

### 전역 스코프(Global scope)

전역에 변수를 선언하면 이 변수는 전역 스코프를 갖습니다. 따라서 프로그램의 어디에서든 참조할 수 있으며, 전역 객체에 할당된 `window` 객체의 프로퍼티가 됩니다.

> 서버 사이드 환경(Node.js)에서 전역 객체는 `global` 객체 입니다.

<br>

### 함수 레벨 스코프(Function-level scope)

JavaScript는 함수 레벨 스코프를 따릅니다. 함수 내에서 선언된 변수(매개변수 포함)들은 함수 밖에서는 참조할 수 없습니다.

<br>

```javascript
var global = "window"; // global variable

function print() {
	var local = "print"; // local/function-level variable
}

console.log(global);
console.log(local); // error - local is not defined
```

위의 코드에서 변수 `local`은 함수 내에서 선언되었기 때문에 함수 레벨 스코프를 가집니다. 따라서 함수 밖에서는 참조할 수 없습니다.

<br>

### 블록 레벨 스코프(Block-level scope)

어떤 언어들은 블록 스코프(Block-level scope)를 따릅니다. 예를 들어, C언어는 블록 레벨 스코프를 따릅니다. 블록 레벨 스코프란 코드 블록(`{ .. }`) 내에서 유효한 스코프를 말합니다.

> 코드 블록은 `if`, `for`, `while`, `try`/`catch` 등의 Statement에서 쓰이는 블록을 말합니다.

<br>

#### `var`: 함수 레벨 스코프

```javascript
if (true) {
	var val = "Block";
	console.log(val);
}

console.log(val); // output: 'Block'
```

JavaScript에서는 기본적으로 블록 레벨 스코프를 따르지 않기 때문에 `var` 키워드로 선언된 변수 `val`은 전역 스코프를 가집니다. 따라서 위의 코드는 잘 동작합니다.

<br>

#### `let`, `const`: 블록 레벨 스코프

ES6에서는 블록 레벨 스코프를 지원하기 위해 `let`, `const` 키워드를 도입했습니다. 이 두 키워드로 변수를 선언하면 블록 레벨 스코프를 따르게 됩니다.

```javascript
if (true) {
	const val = "Block";
	console.log(val);
}

console.log(val); // error - val is not defined.
```

<br>

## 렉시컬 스코프(Lexical Scope)

## 렉시컬 스코프(Lexical Scope)

> lexical : 사전의, 사전식의

렉시컬 스코프(Lexical scope)는 정적 스코프(Static scope)라고도 합니다. 반대로 동적 스코프(Dynamic scope)가 있는데요, 두 스코프의 개념은 아래와 같습니다.

- 렉시컬 스코프 : 함수가 선언된 스코프를 상위 스코프로 함 (Where defined)

- 동적 스코프 : 함수가 호출된 스코프를 상위 스코프로 함 (Where called)

<br>

JavaScript를 포함한 대부분의 언어들에서 렉시컬 스코프를 채택했습니다. 반대로 동적 스코프(Dynamic scope)는 Perl, Bash 등 오래된 언어들이 사용하는 방식입니다.

<br>

> Stackoverflow에 [What is lexical scope?](https://stackoverflow.com/questions/1047454/what-is-lexical-scope)를 주제로 사람들이 묻고 답한 내용이 있습니다.

<br>

---

### References

- [스코프 | poiemaweb.com](https://poiemaweb.com/js-scope)
- [Lexical Scope and Dynamic Scope | bestalign's dev blog](https://bestalign.github.io/2015/07/12/Lexical-Scope-and-Dynamic-Scope/)
- [University of Washington CSE341 2014 Spring - Lecture 9](https://courses.cs.washington.edu/courses/cse341/14sp/slides/lec09.pdf)
