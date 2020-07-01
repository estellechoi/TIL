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

### Asynchronous execution

You can have any chunk of code execute asynchronously. For example, this can be done with the `setTimeout(callback, milliseconds)` function.

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
setTimeout(second, 1000); // asynchronous execution
third();
```

The output will be:

```
first
third
second
```

<br>

### Why callbacks are needed?

Let's see the below example.

```javascript
// ajax(..) is some arbitrary Ajax function given by a library
const response = ajax("https://example.com/api");

// `response` won't have any value.
console.log(response);
```

You’re probably aware that standard `Ajax` requests don’t complete synchronously, which means that at the time of code execution the `ajax(..)` function does not yet have any value to return back. Nothing is to be assigned to `const response` variable.

<br>

A simple way of “waiting” for an asynchronous function to return its result is to use a function called callback. See the below.

```javascript
ajax("https://example.com/api", (response) => {
	console.log(response);
});
```

> Note: Though you can actually make synchronous Ajax requests, never, ever do that. If you make a synchronous Ajax request, the UI of your JavaScript app will be blocked — the user won’t be able to click, enter data, navigate, or scroll.

<br>

## Event Loop

### Who tells the JS Engine to execute asynchronously(multiply)?

Despite allowing async JavaScript code (like the `setTimeout`), until ES6, JavaScript itself has actually never had any direct notion of asynchrony built into it.

So, who tells the JS Engine to execute multiple chunks of your program? The event loop does, which is a built-in mechanism in all environments where JS Engine runs inside. The event loop handles the execution of multiple chunks of your program over time, each time invoking the JS Engine.

> In reality, the JS Engine doesn’t run in isolation — it runs inside a hosting environment, which is the typical web browser or Node.js. Actually, nowadays, JavaScript gets embedded into all kinds of devices, from robots to light bulbs. Every single device represents a different type of hosting environment for the JS Engine. This means that the JS Engine is just an on-demand execution environment for any JS code. It’s the surrounding environment that schedules the events(the JS code executions).

<br>

When your JavaScript program makes an `Ajax` request to fetch some data, you set up the "after response" code in a callback function. And the JS Engine tells the hosting environment:

“Hey, I’m going to suspend execution for now, but whenever you finish with that network request, and you have some data, please call this function back.”

The browser is then set up to listen for the response from the network. An when it has something to return to you, it will schedule the callback function to be executed by inserting it into the event loop.

<br>

### What is the event loop after all?

![Event Loop](./../img/eventLoop.png)

The Event Loop has one simple job:

- to monitor the Call Stack and the Callback Queue. If the Call Stack is empty, it will take the first event from the Callback queue and will push it to the Call Stack. And the Event Loop keeps doing this repeatedly.

Such an iteration is called a "tick" in the Event Loop. Each event is just a function callback.

<br>

### Event Loop Working Process

Let’s execute this code and see what happens.

```javascript
console.log("Hi");

setTimeout(function cb1() {
	console.log("cb1");
}, 5000);

console.log("Bye");
```

#### 1. The browser console is clear, and the Call Stack is empty.

![Stop 1](./../img/eventLoop1.png)

<br>

#### 2. `console.log('Hi')` is added to the Call Stack.

![Stop 2](./../img/eventLoop2.png)

<br>

#### 3. `console.log('Hi')` is executed.

![Stop 3](./../img/eventLoop3.png)

<br>

#### 3. `console.log('Hi')` is executed.

![Stop 3](./../img/eventLoop3.png)

<br>

#### 4. `console.log('Hi')` is removed from the Call Stack.

![Stop 4](./../img/eventLoop4.png)

<br>

#### 5. `setTimeout(function cb1() { ... })` is added to the Call Stack.

![Stop 5](./../img/eventLoop5.png)

<br>

#### 6. `setTimeout(function cb1() { ... })` is executed.

The browser creates a timer as part of the Web APIs. It is going to handle the countdown for you.

![Stop 6](./../img/eventLoop6.png)

<br>

#### 7. `The setTimeout(function cb1() { ... })` itself is complete and removed from the Call Stack.

The timer keeps hadling the countdown.

![Stop 7](./../img/eventLoop7.png)

<br>

#### 8. `console.log('Bye')` is added to the Call Stack.

![Stop 8](./../img/eventLoop8.png)

<br>

#### 9. `console.log('Bye')` is executed.

![Stop 9](./../img/eventLoop9.png)

<br>

#### 10. `console.log('Bye')` is removed from the Call Stack.

![Stop 10](./../img/eventLoop10.png)

<br>

#### 11. After at least 5000 ms, the timer completes and it pushes the `cb1` callback to the Callback Queue.

![Stop 11](./../img/eventLoop11.png)

<br>

#### 12. The Event Loop takes `cb1` from the Callback Queue and pushes it to the Call Stack.

![Stop 12](./../img/eventLoop12.png)

<br>

#### 13. `cb1` is executed and adds `console.log('cb1')` to the Call Stack.

![Stop 13](./../img/eventLoop13.png)

<br>

#### 14. `console.log('cb1')` is executed.

![Stop 14](./../img/eventLoop14.png)

<br>

#### 15. `console.log('cb1')` is removed from the Call Stack.

![Stop 15](./../img/eventLoop15.png)

<br>

#### 16. `cb1` is removed from the Call Stack.

![Stop 16](./../img/eventLoop16.png)

<br>

> It’s interesting to note that ES6 specifies how the event loop should work. It means that technically the event loop is within the scope of the JS engine’s responsibilities, no longer playing just a hosting environment role. One main reason for this change is the introduction of Promises in ES6 because the latter requires access to a direct, fine-grained control over scheduling operations on the event loop queue.

<br>

## How `setTimeout(callback, 0)` works

It’s important to note that `setTimeout()` doesn’t automatically put your callback on the event loop queue. It sets up a timer. When the timer expires, the environment places your callback into the event loop, so that some future tick will pick it up and execute it.

<br>

For example,

```javascript
setTimeout(myCallback, 1000);
```

This code doesn’t mean that `myCallback` function will be executed in 1,000 ms but rather that, in 1,000 ms `myCallback` will be added to the queue. The queue, however, might have other events that have been added earlier — your callback will have to wait. calling `setTimeout(callback, 0)` just puts off the callback until the Call Stack is clear.

<br>

```javascript
console.log("Hi");

setTimeout(function () {
	console.log("callback");
}, 0);

console.log("Bye");
```

Although the wait time is set to 0 ms, the output is:

```
Hi
Bye
callback
```

<br>

## What are Jobs in ES6 ?

A new concept called the "Job Queue" was introduced in ES6. It’s a layer on top of the Event Loop queue. Imagine it like this: the Job Queue is a queue that’s attached to the end of every tick in the Event Loop queue.

During a tick of the event loop, async actions will not cause a whole new event to be added to the event loop queue, but will instead add a <b>Job</b> to the end of the current tick’s Job queue. This means that you can add another functionality to be executed later, and you can rest assured that it will be executed right after, before anything else.

> A Job can also cause more Jobs to be added to the end of the same queue. In theory, it’s possible for a Job loop (a Job that keeps adding other Jobs, etc.) to spin indefinitely.

> It is mostly for asynchronous behavior of Promises.

<br>

## Callback hell

```javascript
listen("click", function (e) {
	setTimeout(function () {
		ajax("https://api.example.com/endpoint", function (text) {
			if (text == "hello") {
				doSomething();
			} else if (text == "world") {
				doSomethingElse();
			}
		});
	}, 500);
});
```

We’ve got a chain of three functions nested together, each one representing a step in an asynchronous series. What a callback hell.

First, we’re waiting for the “click” event, then we’re waiting for the timer to fire, then we’re waiting for the Ajax response to come back, at which point it might get all repeated again.

<br>

## Promises

...

<br>

## JavaScript Working Process

![Javascript working process](./../img/javascript.png)

<br>

### References

- [How JavaScript works: an overview of the engine, the runtime, and the call stack](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)
- [How JavaScript works: Event loop and the rise of Async programming + 5 ways to better coding with async/await](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
