# [React] Class vs Function 컴포넌트

## 목차

```
1. Class와 Function 컴포넌트의 렌더링
2. Class와 Function 컴포넌트에 데이터(props) 전달하기
3. Class 컴포넌트에서 state 사용하기
4. Function 컴포넌트와 useState Hook
5. Class 컴포넌트에서 라이프사이클 다루기
6. Function 컴포넌트에서 라이프사이클 다루기
7. useEffect 심화
```

<br>

## 1. Class와 Function 컴포넌트의 렌더링

### Class

Class 컴포넌트를 렌더링하려면 `render()` 함수를 오버라이드(Override)해야 합니다.`render()` 함수 내에서 렌더링할 JSX를 반환해주면 되죠.

```javascript
class Comp extends React.Component {
    render() {
        return ();
    }
}
```

<br>

### Function

Function 컴포넌트의 렌더링은 비교적 간단합니다. 함수 자신이 반환하는 것이 렌더링 됩니다.

```javascript
function Comp() {
    return ();
}
```

<br>

## 2. Class와 Function 컴포넌트에 데이터(`props`) 전달하기

### Class

Class 컴포넌트에서는 `props` 속성을 통해 접근할 수 있습니다. 예를 들어 Class 컴포넌트에 아래와 같이 `props`를 부여한다고 가정해봅시다.

```jsx
<Comp name="Yujin" number={7}></Comp>
```

<br>

위와 같이 전달한 `props` 들은 `this.props` 객체에 담겨있습니다.

```javascript
class Comp extends React.Component {
	render() {
		return (
			<div>
				{this.props.name} likes number {this.props.number}.
			</div>
		);
	}
}
```

<br>

### Function

Function 컴포넌트에서는 인자로 `props` 객체를 받습니다. `this`로 함수 자신에 접근할 필요없이, 전달받은 인자를 참조하면 됩니다.

```javascript
function Comp(props) {
	return (
		<div>
			{props.name} likes number {props.number}.
		</div>
	);
}
```

<br>

ES6에서 소개된 [구조 분해 할당](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 문법을 사용하면 아래와 같이 리팩토링할 수 있습니다.

```javascript
function Comp({ name, number }) {
	return (
		<div>
			{name} likes number {number}.
		</div>
	);
}
```

<br>

## 3. Class 컴포넌트에서 `state` 사용하기

### `state` 초기화

컴포넌트 내부에서 사용할 (상태)값은 `React.Component`로부터 상속받은 `state` 속성에 담아서 사용합니다. `state` 값이 바뀔 때마다 `render()` 함수가 호출되고요, 따라서 업데이트된 `state` 값이 포함된 JSX가 다시 렌더링 됩니다.

```javascript
class Comp extends React.Component {
	state = {
		name: this.props.name,
		number: this.props.number,
	};

	render() {
		return (
			<div>
				{this.state.name} likes number {this.state.number}.
			</div>
		);
	}
}
```

<br>

> `state` 값을 지정하지 않으면 초기값은 `null`입니다. `this.state`로 접근해서 확인해보세요.

<br>

### `state` 값 업데이트하기

`state` 값을 변경하려면 `setState()` 함수를 호출합니다. 변경할 `state`의 속성과 새 값으로 구성된 객체를 인자로 전달하고요.

버튼을 클릭하여 숫자를 바꾸는 예제를 통해 알아볼게요. `<button>` 요소의 `onClick` 속성에 이벤트 발생시 호출할 함수를 지정해줍니다. 아래 예제에서는 함수 선언식을 사용해 정의된 `changeNumber` 함수를 지정했습니다.

> `changeNumber`는 화살표 함수가 아니므로 이 함수 내에서 `this`는 함수 자기자신을 의미합니다. `React.Component`의 `setState()` 메소드를 호출하기 위해서는 `bind()` 함수를 이용하여 외부 `this`를 바인딩하세요.

```javascript
class Comp extends React.Component {
	state = {
		name: this.props.name,
		number: this.props.number,
	};

	function changeNumber () {
		this.setState({ number: Math.random() });
	}.bind(this);

	render() {
		return (
			<div>
				{this.state.name} likes number {this.state.number}.
			</div>
			<button type="button" onClick={changeNumber}>Click me</button>
		);
	}
}
```

<br>

하지만 Function 컴포넌트에서는 `React.Component` 클래스를 상속받을 수 없기 때문에 위와 같이 `state` 속성도 사용할 수 없습니다. 때문에 상위 컴포넌트에서 전달한 `props` 값들을 보여주는 간단한 컴포넌트를 만들 때 사용되었죠. Hook이 도입되기 전까지는요.

<br>

## 4. Function 컴포넌트와 `useState` Hook

React 16.8부터는 Hook 개념이 도입되었습니다. React의 내장 Hook도 있고, 커스텀 Hook을 만들어 사용할 수도 있죠. 내장 Hook 중 `useState`는 Function 컴포넌트에서 (Class 컴포넌트에서만 사용할 수 있었던) `state`의 사용을 지원합니다.

<br>

### `useState` 사용하기

먼저 아래와 같이 `import`를 하고요.

```javascript
import react, { useState } from "react";
```

<br>

`useState()`를 호출하면 무조건 길이가 `2`인 배열이 반환됩니다. 이 배열의 첫 번째 원소는 컴포넌트에서 사용할 데이터(상태)이고요, 두 번째 원소는 데이터(상태)를 업데이트하는 함수입니다. `useState()` 호출시 인자를 전달하면 `state`의 초기값을 지정할 수 있고요, 인자를 넘기지 않으면 `state` 값은 `undefined`입니다.

```javascript
function Comp({ name, number }) {
	// props로 받은 값으로 state 초기값을 지정
	const [num, setNum] = useState(number);

	return (
		<div>
			{name} likes number {num}.
		</div>
	);
}
```

<br>

### `state` 값 업데이트하기

`class` 컴포넌트와 마찬가지로 `setState()` 함수를 호출하면 됩니다. 하지만 컴포넌트에 종속되어있지 않기 때문에 `this`로 접근할 필요가 없습니다. 또한, 여러 개의 `state`가 있더라도 각각 자신을 업데이트하는 `setState()` 함수를 갖기 때문에 인자로 변경하고자하는 새 값만 전달하면 되죠.

```javascript
function Comp({ name, number }) {
	// props로 받은 값으로 state 초기값을 지정
	const [num, setNum] = useState(number);

	function changeNumber() {
		// setState(arg);
		setNum(Math.random());
	}

	return (
		<div>
			{name} likes number {num}.
		</div>
		<button type="button" onClick={changeNumber}>Click me</button>
	);
}
```

<br>

## 5. Class 컴포넌트에서 라이프사이클 다루기

아래는 React 컴포넌트의 라이프사이클을 나타내는 그림입니다.

![Class Lifecycle](./../img/react-class-lifecycle.png)

<br>

### 마운트(Mount)

아래와 같이 `ReactDOM.render()`가 호출되면 마운트(Mount) 함수들이 차례로 호출됩니다. Class 컴포넌트가 최초 렌더링될 때 호출되는 함수들이죠.

```javascript
const root = document.getElementById("root");
ReactDOM.render(<App />, root);
```

<br>

다음은 마운트 함수들입니다.

- `constructor()`

- `componentWillMount()`

- `render()`

- `componentDidMount()`

<br>

`componentWillMount()` 호출 이전의 `constructor()`에서는 `state` 값을 초기화하고요, `props`는 `super()`의 인자로 넘깁니다. 이 작업들은 원래 `getDefaultProp()`-`getInitialState()` 함수가 차례로 호출되면서 처리했었는데 현재는 Deprecated 되었습니다. 대신 `constructor()`에서 처리하죠. 그 다음 `componentWillMount()`-`render()`-`componentDidMount()` 순으로 라이프사이클이 진행됩니다.

<br>

만약 렌더링 직전, 컴포넌트 마운팅이 시작될 때 무언가 처리하고 싶다면 `componentWillMount()` 함수를 오버라이드하면 되겠죠. 아래 예제를 보세요.

```javascript
class Comp extends React.Component {
	componentWillMount() {
		console.log(`%ccomponentWillMount`, "color: dodgerblue");
	}

	render() {
		console.log(`%crender`, "color: purple");
		return <div></div>;
	}
}
```

<br>

콘솔 출력 결과는 다음과 같습니다. 호출된 순서를 확인해보세요.

<br>

<img src="./../img/react-lifecyclelog.png" alt="React Lifecycle Log" width="700"/>

<br>

### 업데이트(Update)

`setProps()`/`setState()`가 호출되어서 컴포넌트가 다시 렌더링되어야하는 경우, 아래의 업데이트(Update) 사이클 함수들이 차례로 호출됩니다.

- `componentWillReceiveProps(nextProps)` : `setProps()` 호출시 이 함수부터 차례로 호출됩니다.

- `shouldComponentUpdate(nextProps, nextState)` : `setState()` 호출시 이 함수부터 차례로 호출됩니다.

- `componentWillUpdate(nextProps, nextState)`

- `render()` : 마운트(Mount) 시점에 호출되었던 `render()`와 동일한 함수입니다.

- `componentDidUpdate(preProps, preState)`

<br>

> 위 그림에서 `componentWillReceiveProps(nextProps)`는 생략했습니다.

<br>

### 언마운트(Unmount)

컴포넌트가 DOM에서 제거될 때 호출됩니다.

- `componentWillUnmount()`

<br>

## 6. Function 컴포넌트에서 라이프사이클 다루기

위에서 보았듯이, Class 컴포넌트는 각 라이프사이클 단계마다 원하는 로직을 지정할 수 있도록 함수들을 제공합니다. 하지만 Function 컴포넌트에서는 이 함수들을 사용할 수 없죠. 애초에 `class`가 아닌 `function` 이기 때문에 `React.Component` 클래스를 상속받을 수 없으니까요. 그럼 Function 컴포넌트에서는 어떻게 각 라이프사이클 단계에 접근해서 컴포넌트를 컨트롤할까요? `useEffect` Hook을 사용합니다.

<br>

### `useEffect` 사용하기

아래 예제는 위에서 사용했던 Class 컴포넌트 예제입니다. 이 컴포넌트를 Function 컴포넌트로 전환하면서 `useEffect` Hook을 사용해보죠.

```javascript
class Comp extends React.Component {
	componentWillMount() {
		console.log("%ccomponentWillMount", "color: dodgerblue");
	}

	render() {
		console.log("%crender", "color: purple");
		return <div></div>;
	}
}
```

<br>

먼저 `useEffect` Hook을 사용하기 위해 `import` 합니다.

```javascript
import React, { useEffect } from "react";
```

<br>

Function 컴포넌트로 전환하기 위해 `function` 함수 선언식을 사용해 컴포넌트를 만들고요. 더이상 `render()` 함수를 사용할 수도, 필요도 없으므로 지웁니다. `componentWillMount()`도 사용할 수 없으므로 지울게요.

```javascript
function Comp() {
	return <div></div>;
}
```

<br>

그리고 `useEffect()`를 실행시켜보죠. 첫 번째 인자로 함수를 전달합니다.

```javascript
function Comp() {
	useEffect(() => {
		console.log("%cuseEffect() was called", "color: pink");
	});

	console.log("%crender", "color: purple");

	return <div>{console.log("%crendering", "color: red")}</div>;
}
```

<br>

콘솔 출력 결과는 아래와 같습니다. `console.log("%crender", "color: purple")`이 먼저 실행되고, JSX가 렌더링되면서 `console.log("%crendering", "color: red")`가 실행되고요, 마지막으로 `useEffect()` Hook이 렌더링 후에 실행되네요.

<br>

<img src="./../img/react-func-comp.png" alt="React useEffect log" width="700" />

<br>

그러니까, 컴포넌트 렌더링 후에 어떤 일을 처리하고 싶다면 Function 컴포넌트에서는 `componentDidMount()` 대신 `useEffect()`를 사용하면 됩니다. 그런데 이 `useEffect()`는 `componentDidMount()`와 완전히 동일하지는 않습니다. `state`의 업데이트가 일어날 때마다 렌더링 직후 함께 호출되거든요. 오히려 `componentDidMount()` + `componentDidUpdate()`를 합한 것과 비슷한 역할을 하네요.

<br>

## 7. `useEffect` 심화

`useEffect`는 Function 컴포넌트에서 각 라이프사이클 단계에 접근하여 원하는 일을 처리할 수 있도록 해줍니다. 이 `useEffect`에 대해 자세히 알아봅시다.

<br>

### `useEffect`는 여러번 사용해도 됩니다

아래와 같이 `useEffect`는 여러번 사용할 수 있습니다. 기본적으로 코드를 작성한 순서대로 `useEffect()` 함수들이 실행되고요.

```javascript
function Comp() {
	useEffect(() => {
		console.log("%cuseEffect() was called", "color: pink"); // 3
	});

	useEffect(() => {
		console.log("%cuseEffect() was called", "color: orange"); // 4
	});

	console.log("%crender", "color: purple"); // 1

	return <div>{console.log("%crendering", "color: red")}</div>; // 2
}
```

<br>

---

### References

- [React Top-Level API | React](https://reactjs.org/docs/react-api.html)
- [Components and Props | React](https://reactjs.org/docs/components-and-props.html)
- [Using the State Hook | React](https://reactjs.org/docs/hooks-state.html)
- [Using the Effect Hook | React](https://reactjs.org/docs/hooks-effect.html)
- [React.Component | React](https://reactjs.org/docs/react-component.html#constructor)
- [React component lifecycle, API 정리](https://gseok.gitbooks.io/react/content/bd80-bd84-bd80-bd84-c9c0-c2dd-b4e4/react-component-lifecycle-api-c815-b9ac.html)
