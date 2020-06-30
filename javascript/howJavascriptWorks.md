# How Javascript works ?

> This markdown page is just a combination of several recommended articles. The only goal of this page is to help people dig into how JavaScript actually works on this spot rather thant wandering around.

<br>

## JavaScript Engine

A JavaScript engine is a program or an interpreter which executes JavaScript code.

A popular example of a JavaScript Engine is Google’s V8 engine. The V8 engine is used inside Chrome and also for Node.js runtime.

> For more examples of JavaScript Engine, see [here](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e).

<br>

![JS Engine](./../img/jsEngine.png "The simplified view of what the engine looks like")

The Engine consists of two main components:

- Memory Heap : where the memory allocation happens
- Call Stack : where your stack frames are as your code executes

<br>

## JavaScript Runtime

For JavaScript runtime, we have the Engine but there is actually a lot more. We have those things called Web APIs which are provided by browsers, like the DOM, AJAX, setTimeout and many more.

And then, we have the so popular event loop and the callback queue.

![JS Runtime](./../img/jsRuntime.png)

<br>

## The Call Stack

JavaScript is a single-threaded programming language, which means it has a single Call Stack. Therefore it can do one thing at a time.

<strong>The Call Stack is a data structure, which records basically where in the program we are.</strong>

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

Each entry in the Call Stack is called a Stack Frame.

And this is how stack traces are being constructed when an exception is being thrown — it is basically the state of the Call Stack when the exception happened.

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

For example,

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

Running on a single thread is quite limiting.

<br>

## Concurrency

The problem is that while the Call Stack has functions to execute, the browser can’t actually do anything else — it’s getting blocked. This means that the browser can’t render, it can’t run any other code, it’s just stuck.

So, how can we execute heavy code without blocking the UI? Well, the solution is <strong>asynchronous callbacks</strong>.

<br>

##

## JavaScript Working Process

![Javascript working process](./../img/javascript.png)

<br>

### References

- [How JavaScript works: an overview of the engine, the runtime, and the call stack](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)
- [How JavaScript works: inside the V8 engine + 5 tips on how to write optimized code](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e)
