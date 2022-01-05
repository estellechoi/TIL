# 프론트엔드 테스트하기 2: Vue + TypeScript + Jest 단위 테스트 작성하기

<br>

1. Vue 컴포넌트 렌더링 테스트 이해하기

<br>

## 1. Vue 컴포넌트 렌더링 테스트 이해하기

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

test("HelloWorld.vue", () => {});
```

<br>

`mount()` 메소드는 이름 그대로 컴포넌트를 Mount하는데요, 컴포넌트를 렌더링할 뿐만 아니라 반환하는 `VueWrapper` 객체에 유용한 테스트 메소드들이 포함되어있기 때문에 컴포넌트를 Wrap한다고 말합니다. `mount()` 메소드가 반환하는 객체를 흔히 `wrapper`라고 명합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld.vue", () => {
	const wrapper = mount(HelloWorld); // wrapper: VueWrapper<any>
});
```

<br>

### 1-3. DOM 엘리먼트에 접근하기

`wrapper.get()` 메소드를 사용하면 렌더링된 컴포넌트의 DOM 엘리먼트에 접근할 수 있습니다. 아래와 같이 `get("[data-test='hello']")`을 호출하면 해당 Attribute 셀렉터에 맞는 엘리먼트를 찾아 반환합니다. 예를 들어, `<div data-test="hello">...</div>`라고 마크업한 엘리먼트가 있다면 이 엘리먼트를 반환하겠죠.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld.vue", () => {
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

test("HelloWorld.vue", () => {
	const wrapper = mount(HelloWorld);
	const target = wrapper.get("[data-test='hello']");

	expect(target.text()).toBe("Hello World!");
});
```

<br>

### 1-5. 테스트를 통과하는 컴포넌트 작성하기

아직 `HelloWorld.vue` 컴포넌트를 만들지 않았기 때문에, 위에서 작성한 테스트 파일을 실행하면 에러가 발생할겁니다. 이제 테스트 코드에서 확인하려는 명세를 충족하는 컴포넌트를 개발하면 됩니다!

```vue
<!-- HelloWorld.vue-->
<template>
	<div data-test="hello">Hello World!</div>
</template>
```

<br>

---

### References

- [A Crash Course | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html)
- [Test your VueJS + TypeScript application - Vincent Francolin](https://medium.com/codex/test-your-vuejs-typescript-application-b7dc9133e6f)
- [Test your VueJS + TypeScript application; part 2 - Vincent Francolin](https://vince-f.medium.com/test-your-vuejs-typescript-application-part-2-acaa5d8ba327)
