# 프론트엔드 테스트하기 2: DOM 업데이트를 비동기로 테스트하기, AAA 패턴

<br>

1. 렌더링된 컴포넌트의 DOM에 접근해서 테스트하기
2. Vue의 DOM 업데이트를 비동기로 테스트하기
3. AAA 패턴 (Arrange, Act, Assert)

<br>

## 1. 렌더링된 컴포넌트의 DOM에 접근해서 테스트하기

### 1-1. 테스트 파일 만들기

Vue 컴포넌트가 무사히 렌더링되는지 확인하는 간단한 테스트 코드를 작성해보겠습니다. 테스트 파일명은 Jest 설정파일인 `jest.config.js`에서 참조하는 Preset이나 직접 명시한 포맷을 따라야합니다. 아무것도 지정하지 않았다면 디폴트 포맷은 다음과 같습니다.

```javascript
// jest.config.js

module.exports = {
	// ..
	testMatch: ["**/tests/unit/**/*.spec.[jt]s?(x)", "**/__tests__/*.[jt]s?(x)"],
};
```

<br>

### 1-2. 컴포넌트 Mount & Wrap

저는 `HelloWorld.vue` 컴포넌트에 대한 테스트를 수행할 `helloworld.spec.ts` 파일을 하나 만들었고요, 다음과 같이 `@vue/test-utils`(VTU)에서 제공하는 `mount()` 메소드와 테스트 대상 컴포넌트인 `HelloWorld.vue`를 Import 했습니다. `test()` 메소드는 VTU에서 Test를 정의하는 전역 메소드로, 두 번째 인자로 테스트 코드가 담긴 콜백 함수를 받습니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld", () => {});
```

<br>

`mount()` 메소드는 이름 그대로 컴포넌트를 Mount하는데요, 컴포넌트를 렌더링할 뿐만 아니라 반환하는 `VueWrapper` 객체에 유용한 테스트 메소드들이 포함되어있기 때문에 컴포넌트를 Wrap한다고 말합니다. `mount()` 메소드가 반환하는 객체를 흔히 `wrapper`라고 명합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld", () => {
	const wrapper = mount(HelloWorld); // wrapper: VueWrapper<any>
});
```

<br>

### 1-3. Attribute 셀렉터로 DOM 엘리먼트에 접근하기

`wrapper.get()` 메소드를 사용하면 렌더링된 컴포넌트의 DOM 엘리먼트에 접근할 수 있습니다. 아래와 같이 `get("[data-test='hello']")`을 호출하면 해당 Attribute 셀렉터에 맞는 엘리먼트를 찾아 반환합니다. 예를 들어, `<div data-test="hello">...</div>`라고 마크업한 엘리먼트가 있다면 이 엘리먼트를 반환하겠죠.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld", () => {
	const wrapper = mount(HelloWorld);
	const target = wrapper.get("[data-test='hello']");
});
```

<br>

엘리먼트에 접근할 때 Class나 Id가 아닌 Attribute 셀렉터를 사용하는 이유는, Class와 Id는 변하기 때문입니다.

> Using data-test selectors is not required, but it can make your tests less brittle. classes and ids tend to change or move around as an application grows - by using data-test, it's clear to other developers which elements are used in tests, and should not be changed. - [Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html#the-first-test-a-todo-is-rendered)

<br>

### 1-4. `expect()`

이제 렌더링된 결과물이 개발자의 예상과 일치하는지 확인하기위해 VTU의 전역 메소드인 `expect()` 메소드를 사용합니다. `expect()` 메소드의 인자로 테스트 대상이 되는 값을 넣고요, 의도한 값을 인자로 받는 `toBe()` 메소드를 체이닝합니다.

- `expect(target.text())`: `target` 엘리먼트의 `innerText` 값을 예상합니다
- `.toBe("Hello World!")`: 예상하는 값은 `"Hello World!"` 입니다

<br>

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld", () => {
	const wrapper = mount(HelloWorld);
	const target = wrapper.get("[data-test='hello']");

	expect(target.text()).toBe("Hello World!");
});
```

<br>

### 1-5. 테스트를 통과하는 컴포넌트 작성하기

아직 `HelloWorld.vue` 컴포넌트를 만들지 않았기 때문에, 위에서 작성한 테스트 파일을 실행하면 에러가 발생할겁니다. 이제 테스트 코드에서 확인하려는 명세를 충족하는 컴포넌트를 개발하면 됩니다! 정말 심플하게 만들면 다음과 같이 작성할 수 있고, 일단 이 컴포넌트는 테스트를 통과하게 됩니다.

```vue
<!-- HelloWorld.vue-->
<template>
	<div data-test="hello">Hello World!</div>
</template>
```

<br>

## 2. Vue의 DOM 업데이트를 비동기로 테스트하기

만약 `HelloWorld` 컴포넌트에서 Form Input에 이름을 입력하고 제출하면, 제출한 이름이 나타나는 명세를 테스트한다고 가정해보겠습니다. 이 경우에는 Input에 값을 입력할 때와 제출할 때 각각 DOM 업데이트가 발생하기 때문에 VTU 메소드들을 비동기로 호출해야합니다. 아래와 같이 `await` 키로 명시하지 않으면 VTU 메소드들은 기본적으로 동기적으로 실행됩니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - Form Submit", async () => {
	const wrapper = mount(HelloWorld);

	// Input에 이름을 입력
	await wrapper.get("[data-test='input-name']").setValue("Yujin Choi");
	// Form submit
	await wrapper.get("[data-test='form']").trigger("submit");

	expect(wrapper.findAll('[data-test="new-name"]')).toHaveLength(1);
});
```

<br>

DOM 업데이트가 발생하는 테스트를 비동기로 실행해야하는 이유는 Vue에서의 DOM 업데이트가 비동기 방식으로 일어나기 때문입니다. Vue는 실제 DOM의 복사본과 같은 [가상 DOM](https://v3.vuejs.org/guide/optimizations.html#virtual-dom)을 사용하는데요, 컴포넌트에서 발생하는 변경을 실제 DOM에 바로 반영하지 않습니다. Vue는 변경을 감지하면 [큐](https://v3.vuejs.org/guide/change-detection.html#async-update-queue)를 열어 동일한 [이벤트 루프](https://github.com/estellechoi/TIL/blob/master/docs/javascript/howJavascriptWorks.md#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84event-loop) 틱(Tick)에서 발생하는 모든 데이터 변경을 버퍼에 모으고 중복을 제거합니다. 그 다음, 새로운 틱이 시작되면 큐에 쌓인 데이터를 모두 비우고 이전 DOM과 달라진 부분에 대해서만 실제 DOM을 조작합니다. 다음은 [Vue 공식문서](https://v3.vuejs.org/guide/change-detection.html#async-update-queue)에서 발췌한 설명입니다.

> Vue performs DOM updates asynchronously. Whenever a data change is observed, it will open a queue and buffer all the data changes that happen in the same event loop. If the same watcher is triggered multiple times, it will be pushed into the queue only once. This buffered de-duplication is important in avoiding unnecessary calculations and DOM manipulations. Then, in the next event loop "tick", Vue flushes the queue and performs the actual (already de-duped) work. Internally Vue tries native Promise.then, MutationObserver, and setImmediate for the asynchronous queuing and falls back to setTimeout(fn, 0).

<br>

## 3. AAA 패턴 (Arrange, Act, Assert)

위 섹션에서 사용한 예제는 전형적인 AAA(Arrange, Act, Assert) 패턴의 테스트 코드 입니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - Form Submit", async () => {
	// Arrange
	const wrapper = mount(HelloWorld);

	// Act
	await wrapper.get("[data-test='input-name']").setValue("Yujin Choi");
	await wrapper.get("[data-test='form']").trigger("submit");

	// Assert
	expect(wrapper.findAll('[data-test="new-name"]')).toHaveLength(1);
});
```

<br>

- Arrange: 시나리오를 세업하는 단계, Vuex 스토어를 생성하거나
- Act: 사용자의 행동을 시뮬레이션
- Assert: 컴포넌트가 Act 후 어떤 상태여야 하는지에 대한 명세

<br>

---

### References

- [A Crash Course | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html)
- [Conditional Rendering | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/conditional-rendering.html)
- [Test your VueJS + TypeScript application - Vincent Francolin](https://medium.com/codex/test-your-vuejs-typescript-application-b7dc9133e6f)
- [Test your VueJS + TypeScript application; part 2 - Vincent Francolin](https://vince-f.medium.com/test-your-vuejs-typescript-application-part-2-acaa5d8ba327)
