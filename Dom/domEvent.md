# Events and the DOM

<br>

## Registering event listeners

### EventTarget.addEventListener

```javascript
myButton.addEventListener("click", greet, false); // Assuming a button element
```

### HTML attribute

```html
<button onclick="alert('Hello world!')"></button>
```

### DOM element properties

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

- capture : 등록된 이벤트 리스너 하위에 있는 타겟에 이벤트를 전송하기 전에 반드시 등록된 리스너에 가장 먼저 이벤트를 전송할지 여부
- once : 이벤트가 등록된 이후로 리스너를 한 번만 작동시키고 제거할지 여부
- passive : 이벤트 리스너의 콜백함수에서 `preventDefault()` 메소드를 호출할지 여부, `true` 이면 `preventDefault()` 메소드를 절대 호출하지 않는다.

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
```

<br>

## Accessing Event interfaces

- The `Event` interface is accessible from within the handler function, via the event object passed as the first argument.

<br>

---

### Reference

- [Events and the DOM | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events)
- [EventTarget.addEventListener() | MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [DOM Level 3 Events draft](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)
- [Examples of web and XML development using the DOM - Example 5: Event Propagation | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#Example_5:_Event_Propagation)
