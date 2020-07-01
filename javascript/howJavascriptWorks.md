# How Javascript works ?

> This markdown page is just a combination of several recommended articles. The only goal of this page is to help people dig into how JavaScript actually works on this page rather than wandering around from hrer to there.

<br>

## JavaScript Engine

A JavaScript engine is a program or an interpreter which executes JavaScript code. A popular example of a JavaScript Engine is Google’s V8 engine. The V8 engine is used inside Chrome and also for Node.js runtime.

> For more examples of JavaScript Engine, see [here](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e).

<br>

![JS Engine](./../img/jsEngine.png "The simplified view of what the engine looks like")

The Engine consists of two main components:

- Memory Heap : where the memory allocation happens
- Call Stack : where your stack frames are as your code executes

<br>

> How V8 engine works? there is my another markdown page, see [here](https://github.com/estellechoi/TIL/blob/master/javascript/v8.md). It is written in Korean.

<br>

## JavaScript Runtime

For JavaScript runtime, we have the engine but there is actually a lot more. We have those things called Web APIs which are provided by browsers, like the DOM, AJAX, setTimeout and many more. And then, we have the so popular event loop and the callback queue.

![JS Runtime](./../img/jsRuntime.png)

<br>

## The Call Stack

JavaScript is a single-threaded programming language, which means it has a single Call Stack. Therefore it can do one thing at a time. <strong>The Call Stack is a data structure, which records basically where in the program we are.</strong>

If we step into a function, we put it on the top of the stack. If we return from a function, we pop off the top of the stack. That’s all the stack can do.

<br>

For example,

```javascript
function multiply(x, y) {
	return x * y;
}

function printSquare(x) {
	var s = multiply(x, x);
	console.log(s);
}

printSquare(5);
```

When the engine starts executing this code, the Call Stack would be empty. Afterwards, the steps will be the following:

![Call Stack Steps](./../img/callStackSteps.png)

<br>

Each entry in the Call Stack is called a Stack Frame. And this is how stack traces are being constructed when an exception is being thrown — it is basically the state of the Call Stack when the exception happened.

For example with the below code,

```javascript
function foo() {
	throw new Error("SessionStack will help you resolve crashes :)");
}

function bar() {
	foo();
}

function start() {
	bar();
}

start();
```

If this is executed in Chrome, the following stack trace will be produced like`At foo` as you see.

![Stack Traces](./../img/stackTraces.png)

<br>

### Blowing the stack

This happens when you reach the maximum Call Stack size. And that could happen quite easily, especially if you’re using recursion without testing your code very extensively.

> 번역 : 스택이 쌓이다가 한도 사이즈를 초과하면 "스택 날려버리기"가 발생합니다. 이는 꽤 쉽게 발생할 수 있는데요, 재귀 코드를 광범위하게 테스트하지 않았을 때 특히 그렇습니다.

> 재귀란, 자신을 정의할 때 자기 자신을 재참조하는 방법입니다.

<br>

For example with this recursive code,

```javascript
function foo() {
	foo(); // recursion
}

foo();
```

Call stack gets looking something like this:

![Recursion](./../img/recursionStack.png)

At some point the number of function calls in the Call Stack exceeds the actual size of the Call Stack, and the browser decides to take action, by throwing an error like this:

![Stack blowing](./../img/stackBlowing.png)

<br>

As you see, running on a single thread is quite limiting.

<br>

## The Lack of concurrency

The problem is that while the Call Stack has functions to execute, the browser can’t actually do anything else — it’s getting blocked. This means that the browser can’t render, it can’t run any other code, it’s just stuck.

So, how can we execute heavy code without blocking the UI? The solution is <strong>asynchronous callbacks</strong>.

<br>

## Asynchronous callbacks

The most common block unit is the function. only one of blocks is going to execute now, and the rest will execute later.

The most developers new to JavaScript seem to have problems, understanding that <b>later</b> doesn’t necessarily happen strictly and immediately after <b>now</b>. In other words, tasks that cannot complete now are going to complete <strong>asynchronously</strong>, which means you won’t have the above-mentioned blocking behavior.

<br>

For example,

```javascript
// ajax(..) is some arbitrary Ajax function given by a library
const response = ajax("https://example.com/api");

// `response` won't have any value.
console.log(response);
```

You’re probably aware that standard `Ajax` requests don’t complete synchronously, which means that at the time of code execution the `ajax(..)` function does not yet have any value to return back. Nothing is to be assigned to `const response` variable.

A simple way of “waiting” for an asynchronous function to return its result is to use a function called callback. See the below.

```javascript
ajax("https://example.com/api", (response) => {
	console.log(response);
});
```

> Note: Though you can actually make synchronous Ajax requests, never, ever do that. If you make a synchronous Ajax request, the UI of your JavaScript app will be blocked — the user won’t be able to click, enter data, navigate, or scroll.

<br>

You can have any chunk of code execute asynchronously. This can be done with the `setTimeout(callback, milliseconds)` function.

```javascript
function first() {
	console.log("first");
}

function second() {
	console.log("second");
}

function third() {
	console.log("third");
}

first();
setTimeout(second, 1000);
third();
```

The output will be:

```
first
third
second
```

<br>

## Event Loop

Despite allowing async JavaScript code (like the `setTimeout`), until ES6, JavaScript itself has actually never had any direct notion of asynchrony built into it.

So, who tells the JS Engine to execute multiple chunks of your program? The event loop does, which is a built-in mechanism in all environments where JS Engine runs inside. The event loop handles the execution of multiple chunks of your program over time, each time invoking the JS Engine.

> In reality, the JS Engine doesn’t run in isolation — it runs inside a hosting environment, which is the typical web browser or Node.js. Actually, nowadays, JavaScript gets embedded into all kinds of devices, from robots to light bulbs. Every single device represents a different type of hosting environment for the JS Engine.

This means that the JS Engine is just an on-demand execution environment for any JS code. It’s the surrounding environment that schedules the events(the JS code executions).

<br>

For example,

When your JavaScript program makes an `Ajax` request to fetch some data, you set up the "after response" code in a callback function. And the JS Engine tells the hosting environment:

“Hey, I’m going to suspend execution for now, but whenever you finish with that network request, and you have some data, please call this function back.”

The browser is then set up to listen for the response from the network. An when it has something to return to you, it will schedule the callback function to be executed by inserting it into the event loop.

<br>

## JavaScript Working Process

![Javascript working process](./../img/javascript.png)

<br>

### References

- [How JavaScript works: an overview of the engine, the runtime, and the call stack](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)
- [How JavaScript works: Event loop and the rise of Async programming + 5 ways to better coding with async/await](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
