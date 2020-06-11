# Events and the DOM

> [Events and the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events) 외 관련 문서를 개인적인 필요에 따라 재정렬 혹은 부분 번역하였음

<br>

## Registering event listeners

- `EventTarget.addEventListener`

```javascript
myButton.addEventListener("click", greet, false); // Assuming a button element
```

- HTML attribute

```html
<button onclick="alert('Hello world!')"></button>
```

- DOM element properties (older way)

```javascript
// Assuming myButton is a button element
myButton.onclick = (event) => {
	alert("Hello world");
};
```

<br>

## EventTarget.addEventListener

- The [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) method `addEventListener()` sets up a function that will be called whenever the specified event is delivered to the target.

- Common targets are `Element`, `Document`, and `Window`, but the target may be any object that supports events (such as `XMLHttpRequest`).

### Syntax

```
target.addEventListener(type, listener [, options]);
```

- `type` : A case-sensitive string representing the event type.
- `listener` : The object that receives a notification when an event of the specified type occurs. This must be an object implementing the `EventListener` interface, or a JavaScript function.
- `options` : An `options` object specifies characteristics about the event listener. The available options are:

      	- `capture` : 등록된 이벤트 리스너 하위에 있는 타겟에 이벤트를 전송하기 전에 반드시 등록된 리스너에 가장 먼저 이벤트를 전송할지 여부
      	- `once` : 이벤트가 등록된 이후로 리스너를 한 번만 작동시키고 제거할지 여부
      	- `passive` : 이벤트 리스너의 콜백함수에서 [`preventDefault()`](https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault) 메소드를 호출할지 여부, `true` 이면 `preventDefault()` 메소드를 절대 호출하지 않는다. (`preventDefault()`를 이용하여 `scroll` 이벤트를 막지 않겠다.)

> `passive` 속성에 대한 자세한 내용은 [여기](https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md).

> `preventDefault()` : 이벤트를 취소할 수 있는 경우, 이벤트의 전파를 막지않고 그 이벤트를 취소한다. 이벤트의 취소가능 여부는 [`event.cancelable`](https://developer.mozilla.org/ko/docs/Web/API/Event/cancelable)를 사용해서 확인할 수 있다. 취소불가능한 이벤트에 대해서 preventDefault를 호출해도 결과는 없다. `preventDefault()`는 DOM을 통한 이벤트의 전파를 막지않는다. 전파를 막을때는 [`event.stopPropagation()`](https://developer.mozilla.org/ko/docs/Web/API/Event/stopPropagation)를 사용한다.

```javascript
target.addEventListener("click", onceHandler, {
	capture: false,
	once: false,
	passive: true,
});
```

<br>

### Improving scrolling performance with passive listeners

> Smooth scrolling performance is essential to a good experience on the web, especially on touch-based devices.

#### Problem

최신 브라우저들은 JavaScript 코드가 실행되는 중에도 부드러운 스크롤 퍼포먼스를 지원하기 시작했다. 하지만 터치스크린 디바이스 사용이 증가하면서 브라우저 스크롤 퍼포먼스에 허점이 생겼다. `touchstart`, `touchmove` 이벤트가 발생한 경우, 유저가 터치 상태로 스크롤을 시도하면 자동으로 `preventDefault()` 메소드가 호출되면서 스크롤 기능이 막혔기 때문이다. 다시 말해, 스크린을 터치하는 순간 어떤 결과가 나오기까지 스크롤 딜레이가 발생하게 되었다.\

> 왜 자동으로 `preventDefault()` 메소드가 호출될까 ?

> [`wheel`](https://developer.mozilla.org/en-US/docs/Web/API/Element/wheel_event) 이벤트 발생시에도 동일 이슈가 있다.

#### Solution: the `passive` option

- By marking a `touch` or `wheel` listener as `passive`, the developer is promising the handler won't call `preventDefault()` to disable scrolling. This frees the browser up to respond to scrolling immediately without waiting for JavaScript, thus ensuring a reliably smooth scrolling experience for the user.

```javascript
addEventListener(
	"touchstart",
	function (event) {
		event.preventDefault(); // does nothing
	},
	Modernizr.passiveeventlisteners ? { passive: true } : false
);
```

- The default value for the `passive` option is mostly `true` in modern browsers. Some browsers(specifically, Chrome and Firefox) have changed the default value of the `passive` option to `true` from `false` for the `touchstart`/`touchmove` event listeners.

- This prevents the `touchstart`/`touchmove` event listener from being called, so it can't block page rendering while the user is scrolling.

<br>

### Safely detecting options support

- In older versions of the DOM specification, the third parameter of `addEventListener()` was a Boolean value indicating whether or not to use `capture`. Over time, it became clear that more options were needed. The third parameter was changed to an object that can contain various properties defining the values of options.

- Because older browsers (as well as some not-too-old browsers) still assume the third parameter is a Boolean, you need to build your code to handle this scenario intelligently. You can do this by using feature detection for each of the options you're interested in.

- For example, if you want to check for the passive option:

```javascript
let isPassiveSupported = false;

try {
	const options = {
		// This function will be called when the browser attempts to access the passive property.
		get passive() {
			isPassiveSupported = true;
			return false;
		},
	};

	window.addEventListener("test", null, options);
	window.removeEventListener("test", null, options);
} catch (err) {
	isPassiveSupported = false;
}

// if 'passive' supported, set the value for 'passive' option within the Object type parameter and if not, just cannot set 'passive' option. The third parameter just represents 'capture' value.
target.addEventListener(
	"mouseup",
	handleMouseUp,
	isPassiveSupported ? { passive: true } : false
);
```

<br>

### Event listener with an arrow function

```javascript
function modifyText(new_text) {
	document.getElementById("t2").firstChild.nodeValue = new_text;
}

const target = document.getElementById("outside");
target.addEventListener(
	"click",
	() => {
		modifyText("four");
	},
	false
);
```

> Please note that while anonymous and arrow functions are similar, they have different `this` bindings. While anonymous (and all traditional JavaScript functions) create their own `this` bindings, arrow functions inherit the `this` binding of the containing function.

<br>

### The value of `this` within the handler

- If attaching a handler function to an element using `addEventListener()`, the value of `this` inside the handler is a reference to the element.

- It is the same as the value of the `currentTarget` property of the event argument that is passed to the handler.

```javascript
target.addEventListener("click", function (event) {
	console.log(this.className); // the className of target
	console.log(event.currentTarget === this); // true
});
```

- However, arrow functions do not have their own `this` context.

```javascript
target.addEventListener("click", (e) => {
	console.log(this.className); // `this` is not target
	console.log(e.currentTarget === this); // false
});
```

<br>

### Specifying `this` using `bind()`

- The `Function.prototype.bind()` method lets you specify the value that should be used as `this` for all calls to a given function.

- This lets you easily bypass problems where it's unclear what `this` will be. Note, however, that you'll need to keep a reference to the listener around so you can remove it later.

```javascript
// assume `this` is a target named element
this.name = "Something Good";
this.onclick = function (event) {
	console.log(this.name); // undefined, as `this` === event.currentTarget
};

element.addEventListener("click", this.onclick, false);
```

```javascript
this.name = "Something Good";
this.onclick = function (event) {
	console.log(this.name); // 'Something Good', as `this` is bound to element
};
element.addEventListener("click", this.onclick.bind(this), false); // Trick
```

#### `Function.prototype.bind()`

- `bind()` 메소드가 호출되면 새로운 함수(바인딩하는 함수)를 생성한다.
- `bind()` 메소드의 첫 인자는 바인딩하는 함수가 대상 함수(target function)의 `this`에 전달할 값이다.
- 바인딩 대상 함수의 `this`는 전달받은 값이 된다.

```javascript
const module = {
	x: 42,
	getX() {
		return this.x;
	},
};

// The function gets invoked with `this` at the global scope
// unboundGetX === undefined
const unboundGetX = module.getX;

// bind `this` to `module`.
// boundGetX === 42
const boundGetX = module.getX.bind(module);
```

> More information regarding `bind()` is [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

<br>

## Accessing Event interfaces

- The `Event` interface is accessible from within the handler function, via the [`Event Object`](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_objects) passed as the first argument.

- Event listeners only take one argument, the `Event Object`, which is automatically passed to the listener, and the return value is ignored.

<br>

## Getting Data Into and Out of an Event Listener Using Objects

> Note: JavaScript Functions are actually objects. (Hence they too can have properties, and will be retained in memory even after they finish executing if assigned to a variable that persists in memory.)

> Note: Objects are stored in variables 'by reference', meaning only the memory location of the actual data is stored in the variable. This means variables that store objects can actually affect other variables that get assigned the same object reference. When two variables reference the same object, changing the data in either variable will affect the other.

- 객체를 이용하면 이벤트 리스너에 어떤 데이터를 전달하고, 이벤트 핸들러가 작업을 끝냈을 때 작업 로직에 의해 변경된 데이터를 돌려 받을 수 있다.

- JavaScript에서 객체를 변수에 할당하면 그 객체는 사라지지 않고 메모리에 계속 존재한다. 변수에 객체의 복사본이 저장되는 것이 아니라, 그 객체가 위치한 메모리 상의 주소를 참조하기 때문이다. 따라서 객체의 속성(object properties)을 사용하면 메모리에 저장된 데이터를 가져오거나 새로운 데이터를 메모리에 저장할 수 있다.

- 예를 들어, 이렇게.

```javascript
const myBtn = document.getElementById("myBtn");
const data = { prop: "Data" };

myBtn.addEventListener("click", function () {
	data.prop = "Data Changed"; // Change the value
});

window.setInterval(function () {
	if (data.prop === "Data Changed") {
		alert("Data reset executed");
		data.prop = "Data";
	}
}, 5000);
```

- `addEventListener()`와 `setInterval()` 메소드의 실행이 끝나더라도, `{ prop: "Data" }` 객체의 값을 변경할 수 있다. 이 객체는 변수 `data`에 의해 참조됨으로써 메모리에 남아있기 때문이다.

<br>

## Cross-browser Issues

### Legacy Internet Explorer and attachEvent

In Internet Explorer versions before IE 9, you have to use `attachEvent()`, rather than the standard `addEventListener()`.

To handle cross-browser issue,

```javascript
if (el.addEventListener) el.addEventListener("click", modifyText, false);
else if (el.attachEvent) el.attachEvent("onclick", modifyText);
```

In the case of `attachEvent()`, the value of `this` will be a reference to the `window` object, instead of the element on which it was fired.

<br>

### Compatibility

`Event.preventDefault()`, and `Event.stopPropagation()` are not supported by Internet Explorer 8.

<br>

## Memory issues : the lack of keeping a STATIC function reference

Assume an array below.

```javascript
const els = document.getElementsByTagName("*");
```

- Case 1 : A new (anonymous) handler function is created with each iteration.

```javascript
for (let i = 0; i < els.length; i++) {
	els[i].addEventListener("click", function (event) {
		/*do something*/
	});
}
```

- Case 2 : A function reference.

```javascript
function processEvent(event) {
	/* do something */
}

for (let i = 0; i < els.length; i++) {
	els[i].addEventListener("click", processEvent, false);
}
```

In Case 2 results in smaller memory consumption. Moreover, in Case 1, not possible to call `removeEventListener()`. Though a function reference is kept, but since it is redefined on each iteration, it is not STATIC.

<br>

---

### Reference

- [Events and the DOM | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events)
- [EventTarget.addEventListener() | MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [DOM Level 3 Events draft](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)
- [Examples of web and XML development using the DOM - Example 5: Event Propagation | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#Example_5:_Event_Propagation)
- [Function.prototype.bind() | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
- [Passive event listeners | Github](https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md)
- [DOM Living Standard](https://dom.spec.whatwg.org/#dom-addeventlisteneroptions-passive)
- [event.preventDefault | MDN](https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault)
- [Multi-touch Web Development](https://www.html5rocks.com/en/mobile/touch/)
