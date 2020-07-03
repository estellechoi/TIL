# 이벤트 전파와 이벤트 핸들링

> 이 글은 여러 문서와 아티클을 참고해서 제가 직접 작성한 글입니다.

<br>

## 이벤트 리스너 등록

DOM에서 발생하는 이벤트를 감지하고 그 이벤트에 대한 후속 처리를 하려면 `addEventListener()`라는 웹 API를 이용하면 됩니다. 말 그대로 이벤트 발생시 이를 감지할 이벤트 리스너(Event Listener)를 등록할 때 사용하는 API 입니다. `addEventListener()`는 `EventTarget`이라는 DOM 인터페이스에서 제공하는 메소드 중 하나입니다.

이 인터페이스가 제공하는 메소드에는 이런 것들이 있습니다.

- `addEventListener()` : 특정 이벤트에 대해 이를 핸들링할 이벤트 핸들러를 등록
- `removeEventListener()` : `EventTarget` 인터페이스에 등록된 이벤트 리스너를 제거
- `dispatchEvent()` : `EventTarget` 인터페이스에 이벤트를 전송

<br>

예를 들어,

```html
<a>Click Me</a>
```

<br>

위의 `<a>` 요소를 클릭하는 이벤트가 발생했을 때 이벤트를 핸들링하려면 이런 코드를 작성하면 됩니다.

```javascript
const linkBtn = document.querySelector("a");

linkBtn.addEventListener("click", printEvent); // 이벤트 리스너 등록

function printEvent(event) {
	console.log(event);
}
```

`linkBtn` 요소에 `click` 이벤트에 대한 이벤트 리스너를 등록하고, `click` 이벤트가 발생하면 콜백 함수를 이용해 이벤트를 핸들링하는 코드입니다. 콜백 함수 `printEvent`의 인자로 들어오는 `event` 객체에는 이벤트와 관련된 정보들이 담겨있습니다.

<br>

예를 들어, 이런 것들을 알 수 있습니다.

- `Event.type` : 이벤트의 이름 (대소문자 구별)

- `Event.bubbles` : 이벤트가 DOM을 거쳐 버블링되는지 여부

- `Event.target` : 이벤트가 원래 디스패치된 대상을 참조 (실제로 사용자 이벤트가 일어난 노드)

- `Event.currentTarget` : 이벤트 핸들링을 위해 현재 등록된 대상을 참조 (이벤트가 현재 전달되기로한 객체)

<br>

## 이벤트 전파(Event Propagation)

사용자가 버튼을 클릭하면 `click` 이벤트가 발생한다고 가정해보겠습니다. 얼핏 보면 이 `click` 이벤트는 사용자가 클릭한 버튼에서 한순간 발생했다 사라지는 것 같지만, 실제로는 그렇지 않습니다.

DOM에서 사용자 이벤트를 감지하면 `Event` 객체가 생성되는데요, 이를 이벤트가 디스패치(Dispatch)되었다고 합니다. 이 `Event` 객체는 DOM의 트리 구조 내에서 특정한 매커니즘에 따라 이동합니다. 이것을 이벤트 전파(Event Propagation)라고 합니다. 그러다가 어떤 요소(버튼)에 등록된 이벤트 리스너(Event Listener)에 의해 감지됩니다. 이 때문에 우리는 이벤트 리스너가 등록된 바로 그 요소에서 이벤트가 발생한 것처럼 느끼는 것이죠.

<br>

> 이벤트 디스패치(Dispatch)에 대한 정확한 정의를 확인하려면, [여기](https://www.w3.org/TR/DOM-Level-3-Events/#dispatch)를 보세요.

<br>

이벤트를 잘 핸들링하려면 이벤트 전파(Event Propagation)를 이해하는 것이 중요합니다. 다음은 도움을 받은 원문에서 발췌한 이벤트 전파의 정의입니다.

<b><i>"Event propagation is a mechanism that defines how events propagate or travel through the DOM tree to arrive at its target and what happens to it afterward."</i></b>

<br>

### 이벤트 전파의 3 단계

이벤트가 전파되는 과정을 순서에 따라 3 단계로 나눌 수 있습니다.

1. 캡쳐링(Capturing) 단계

2. 타겟(Target) 단계

3. 버블링(Bubbling) 단계

<br>

아래의 DOM 트리에서 `<td>` 요소를 클릭한다고 가정하겠습니다. 이때 발생한 이벤트는 위의 3 단계 순서와 같이 전파되는데요, 그 모습은 아래 그림과 같습니다.

![Event Flow](./../img/eventFlow.svg)

<br>

## 이벤트 버블링(Event Bubbling)

이벤트 버블링(Event Bubbling)은 어떤 요소에서 이벤트가 발생했을 때 그 이벤트가 더 상위의 요소들로 전달되는 전파 방식입니다. 위에서 살펴본 이벤트 전파의 과정 중 3 단계 전파 과정입니다.

> 반대로, 이벤트 캡쳐링(Event Capturing)은 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.

> 현대의 브라우저들은 기본적으로 이벤트 버블링을 채택하고 있습니다. 이벤트 캡쳐링을 위해서는 `addEventListener()` 메소드의 3 번째 인자의 값을 `{ capture: true }`로 합니다. 이 3 번째 인자에는 브라우저 호환성 이슈가 있습니다. 관련 문서를 찾아보거나 [저의 다른 글](https://github.com/estellechoi/TIL/blob/master/Dom/domEvent.md)을 참조하세요.

그림으로 봅시다.

![Event Bubbling](./../img/bubbling.png)

<br>

아래의 예제로 더 자세히 알아보겠습니다.

```html
<ul>
	<li>
		<span class="product-name">Orange</span>
	</li>
	<li>
		<span class="product-name">Banana</span>
	</li>
	<li>
		<span class="product-name">Apple</span>
	</li>
	<li>
		<span class="product-name">Mango</span>
	</li>
</ul>
```

<br>

각각의 `<li>` 요소를 클릭했을 때 이벤트 로그가 찍히는 코드를 작성해야 한다고 가정하겠습니다. 이를 위해서는 4개의 `<li>` 요소에 일일이 `addEventListener`를 이용해 이벤트를 등록해야 할 것 같네요.

<br>

```javascript
const lists = document.querySelectorAll("ul > li");

for (let i = 0, len = lists.length; i < len; i++) {
	lists[i].addEventListener("click", printLog);
}

function printLog(event) {
	console.log(event.currentTarget);
}
```

이 코드는 잘 동작합니다. 각 리스트를 클릭할 때마다 `printLog` 함수가 실행됩니다. 브라우저가 4개의 이벤트 리스너를 기억하고 있기 때문입니다. 하지만 이런식으로 이벤트를 등록하는 것에는 다음과 같은 문제들이 있습니다.

- `<li>`가 4개가 아니라 훨씬 더 많다면 브라우저가 기억해야 할 이벤트 리스너도 그만큼 많아집니다. 이는 비효율적입니다.
- 또한, 만약 `<li>`가 동적으로 추가된다면 어떻게 될까요? 해당 요소를 동적으로 추가할 때마다 이벤트 리스너도 함께 등록해야 합니다.

<br>

위의 코드는 다음과 같이 개선할 수 있습니다.

```javascript
ul.addEventListener("click", function (event) {
	console.log(event.currentTarget, event.target);
});
```

상위에 있는 `<ul>` 요소에 이벤트 리스너를 등록했지만, 그 하위에 있는 `<li>` 요소들 중 하나를 클릭해도 이벤트 리스너가 이벤트를 감지합니다. 이는 이벤트 버블링 때문입니다. 어떤 요소에 이벤트가 발생하면, 그 이벤트는 그것을 감싸고 있는 상위 요소들로 올라가면서 이벤트 리스너가 있는지 찾습니다.

<br>

## 이벤트 캡쳐링(Event Capturing)

이벤트 캡쳐링은 위에서 언급했듯이, 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다. 어떤 요소에서 이벤트가 발생했을 때, 최상위 요소인 `<html>`에서부터 이벤트가 실제로 발생한 요소까지 탐색하여 찾아옵니다. 이벤트 캡쳐링을 사용하려면 이벤트를 등록할 때 `addEventListener()` 메소드의 3 번째 인자에 `{ capture: true }` 값을 주면 됩니다.

> `addEventListener()` 메소드의 3 번째 인자에는 옵션 객체가 들어옵니다. 옵션 객체에서 해당 리스너에 대한 여러 설정들을 지정할 수 있는데, `capture`는 그 설정 옵션들 중 하나입니다. 이 옵션의 기본값은 보통 `false` 입니다.

<br>

위에서 봤던 그림을 다시 한번 보겠습니다.

![Event Bubbling](./../img/bubbling.png)

<br>

위의 DOM 트리를 코드로 하면 다음과 같습니다.

```html
<div id="wrap">
	<p class="hint">
		<a href="#">Click Me</a>
	</p>
</div>
```

<br>

위의 DOM 트리에서 `<a>` 요소를 클릭했다고 가정해봅시다. `<a>` 요소에서 바로 이벤트가 발생할 것 같지만, 그렇지 않습니다. 최상위 요소인 `<html>` 요소에서 가장 먼저 이벤트가 발생합니다. 그 다음 `<body>`로, `<a>` 요소를 감싸고 있는 `<p>` 요소까지 이벤트 전파가 일어납니다. 이렇게 이벤트가 전파되는 중에 `{ capture: true }` 옵션을 가진 이벤트 리스너를 찾으면, 그 리스너의 콜백 함수가 실행됩니다.

<br>

```javascript
const hint = document.querySelector(".hint");

hint.addEventListener("click", printLog, {
	capture: true,
});

function printLog(event) {
	console.log(event.currentTarget);
}
```

위 코드에서는 `hint` 요소에 이벤트 리스너를 등록하고 `capture` 옵션을 지정했습니다. 이제 `hint` 요소의 하위에 있는 `<a>` 요소를 클릭하면 `html` 요소부터 하위로 이벤트가 전파되다가 `hint` 요소에서 이벤트가 발생했을 때 이벤트 리스너의 콜백함수인 `printLog`가 실행됩니다.

<br>

## 이벤트 전파 막기

이벤트 버블링과 이벤트 캡쳐링은 이벤트가 전파되는 방식들입니다.

<br>

### `Event.stopPropagation()`

이벤트 버블링, 이벤트 캡쳐링과 같은 이벤트 전파를 필요에 따라 막을 수 있습니다. 이벤트 리스너의 콜백함수에서 `stopPropagation()` 웹 API를 사용하면 됩니다. 코드로 보면 아래와 같습니다.

```javascript
function handleEvent(event) {
	event.stopPropagation();
}
```

위 코드를 통해 이벤트 전파를 막을 수 있습니다. 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전파시키지 않습니다. 이벤트 캡쳐링의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않습니다.

<br>

## 이벤트 위임(Event Delegation)

이벤트 캡쳐링과 버블링을 활용하면 이벤트 핸들링 패턴인 이벤트 위임(Event Delegation)을 구현할 수 있습니다. 이벤트 위임은 비슷한 방식으로 여러 요소의 이벤트를 핸들링할 때 사용됩니다. 위의 이벤트 버블링 예제처럼, 반복되는 요소마다 이벤트 리스너를 등록하지 않고, 요소들의 공통 조상이 되는 요소에 이벤트 리스너를 단 하나만 등록하는 방식입니다. 상위 요소에 하나의 리스너를 등록해도 이벤트 전파를 이용해 하위의 여러 요소들의 이벤트를 핸들링할 수 있기 때문입니다.

상위 요소에 등록한 이벤트 리스너의 콜백 함수(이벤트 핸들러)에서 `Event.target`을 통해 실제로 이벤트가 발생한 요소에 접근할 수 있습니다. `Event.target`이 실제로 이벤트가 발생한 요소를 알려주기 때문에, 실제 이벤트가 발생하는 요소가 아닌 상위 요소에 이벤트를 핸들링하라고 위임해놔도 되는거죠.

<br>

---

### References

- [EventTarget | MDN](https://developer.mozilla.org/ko/docs/Web/API/EventTarget)
- [Event | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Event)
- [JavaScript Event Propagation | TutorialRepublic](https://www.tutorialrepublic.com/javascript-tutorial/javascript-event-propagation.php)
- [Event Bubbling and Event Capturing in JavaScript](https://medium.com/@vsvaibhav2016/event-bubbling-and-event-capturing-in-javascript-6ff38bec30e)
- [Bubbling and capturing | javascript.info](https://javascript.info/bubbling-and-capturing)
- [DOM-Level-3-Events | w3.org](https://www.w3.org/TR/DOM-Level-3-Events/)
- [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
