# 함수형 프로그래밍 & JavaScript FP Cheat Sheet

> This doc is WIP ...

<br>

1. FP(함수형 프로그래밍)이란
2. JavaScript FP Cheat Sheet

<br>

## 1. FP(함수형 프로그래밍)이란

### 1-1. FP

FP(함수형 프로그래밍)이란 함수 사용이 강조되는 프로그래밍 방법론입니다. [Side effect](<https://en.wikipedia.org/wiki/Side_effect_(computer_science)>)가 없는 [순수함수](https://en.wikipedia.org/wiki/Pure_function)의 사용이 핵심입니다. 여기에서 Side effect가 없다는 말은, 함수에 전달된 인자에만 의존하며 함수 스코프 외부에 어떠한 변경도 일으키지 않는 것을 의미합니다. FP는 순수함수 외에도 다음의 개념들을 특징으로 합니다.

- 선언적 프로그래밍: 서술 파트와 평가 파트 분리
- 참조 투명성: 함수 인수와 리턴값 사이의 순수한 매핑 관계
- 불변성: 배열 등 객체의 원본을 변경하지 않음

<br>

### 1-2. 메소드 vs 함수

FP를 접하기 전에, 저는 `메소드 = 함수`라고 생각했었는데요.. OOP(객체지향 프로그래밍)에서 말하는 `메소드`와 FP에서 말하는 `함수`는 다른 것입니다. `메소드`는 OOP에서 어떤 객체에 종속된 함수입니다. 예를 들어, JavaScript의 `map`, `filter`와 같은 것들은 `Array` 객체의 메소드입니다. 따라서 `Array` 객체의 인스턴스를 만들어야만 사용할 수 있죠.

```javascript
const list = document.querySelectorAll("*"); // Array-like
list.map((item = item)); // Uncaught TypeError: list.map is not a function
```

<br>

반면, FP에서는 객체를 먼저 만드는 것이 아니라, 함수를 먼저 만들기 때문에 외부 다형성을 지원합니다. 예를 들어 다음과 같이 만든 `_map` 함수는 대상이 Iterable 하기만 하다면, `Array` 타입이 아니더라도 사용할 수 있습니다. 또한, 리턴값을 결정하는 로직이 담긴 보조함수 `iter`를 인자로 받기 때문에 내부 다형성도 지원합니다.

```javascript
const _map = (list, iter) => {
	const newList = [];
	for (let i = 0; i < list.length; i++) {
		newList.push(iter(list[i]));
	}
	return newList;
};

_map(list, (item) => item); // Working !
```

<br>

## 2. JavaScript FP Cheat Sheet

### 2-1. `curry`

```javascript
const _curry = (fn) => (a) => (b) => fn(a, b);

/*
 *  function _curry(fn) {
 *     return function(a) {
 *         return function(b) {
 *            return fn(a, b);
 *          }
 *      }
 *  }
 */
```

<br>

```javascript
const add = _curry((a, b) => a + b);
const add10 = add(10);

add10(2); // 10 + 2 → 12
```

<br>

### 2-2. `curryr`

```javascript
const _curryr = (fn) => (a) => (b) => fn(b, a);
```

<br>

```javascript
const sub = _curryr((a, b) => a - b);
const sub10 = sub(10);

sub10(12); // 12 - 10 → 2
```

<br>

```javascript
const _get = _curryr((obj, key) => (obj === null ? undefined : obj[key]));
const getName = _get("name");

// user: { name: string } 라고 가정
getName(user); // 'Yujin'
```

<br>

### 2-2. `each`

```javascript
const _each = (list, iter) => {
	for (let i = 0; i < list.length; i++) {
		iter(list[i]);
	}
};
```

<br>

### 2-3. `reduce`

```javascript
// const _rest = (list, num) => Array.prototype.slice.call(list, num || 1);
const _rest = (list, num) => Array.from(list).slice(num || 1);

const _reduce = (list, iter, accm) => {
	if (!accm) {
		accm = list[0];
		list = _rest(list);
	}

	_each(list, (item) => {
		accm = iter(accm, item);
	});
	return accm;
};
```

<br>

```javascript
_reduce([1, 2, 3], (a, b) => a + b); // 1 + 2 + 3 → 6
```

<br>

---

### References

- [클린 아키텍처 - 로버트 C. 마틴 (p.27)](http://www.yes24.com/Product/Goods/77283734)
- [함수형 자바스크립트 - 루이스 아텐시오](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=131767959)
- [Difference between Programming Paradigm, Design Pattern and Application Architecture? | Stack Overflow](https://stackoverflow.com/questions/4787799/difference-between-programming-paradigm-design-pattern-and-application-architec)
- [자바스크립트로 알아보는 함수형 프로그래밍 (ES5) | 인프런](https://www.inflearn.com/course/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)
