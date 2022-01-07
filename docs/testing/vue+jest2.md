# 프론트엔드 테스트하기 2: DOM 업데이트를 비동기로 테스트하기, AAA 패턴

<br>

1. 렌더링된 컴포넌트의 DOM에 접근해서 테스트하기
2. 구현 코드와 결합된 테스트 코드 피하기: DOM 접근시 Attribute 셀렉터 사용하기
3. Vue의 DOM 업데이트를 비동기로 테스트하기
4. AAA 패턴 (Arrange, Act, Assert)
5. DOM 엘리먼트의 렌더링 Assert 하기: `find().exists()`로 존재하는지 검사, `get().isVisible()`로 보이는지 검사
6. Mount 옵션으로 Vue 컴포넌트에 옵션 부여하기
7. 테스트 코드에서의 이벤트 핸들링: `emitted()`, Form Submit 테스트

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

`mount()` 메소드는 이름 그대로 컴포넌트를 Mount하는데요, 컴포넌트를 렌더링할 뿐만 아니라 [Wrapper API](https://next.vue-test-utils.vuejs.org/api/#wrapper-methods)를 Implement한 `VueWrapper` 객체를 반환합니다.

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

VueWrapper의 [`get()`](https://next.vue-test-utils.vuejs.org/api/#get) 메소드를 사용하면 렌더링된 컴포넌트의 DOM 엘리먼트에 접근할 수 있습니다. 아래와 같이 `get("[data-test='hello']")`을 호출하면 해당 셀렉터에 맞는 엘리먼트를 찾아 `DOMWrapper` 객체를 반환합니다. 예를 들어, `<div data-test="hello">...</div>`라고 마크업한 엘리먼트가 있다면 이 엘리먼트를 감싼 `DOMWrapper`를 반환하고, 해당하는 엘리먼트가 없으면 에러가 발생하여 테스트에 실패합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld", () => {
	const wrapper = mount(HelloWorld);
	const target = wrapper.get("[data-test='hello']"); // target: Omit<DOMWrapper<T>, 'exist'>
});
```

<br>

### 1-4. `expect()`

이제 렌더링된 결과물이 개발자의 예상과 일치하는지 확인하기위해 VTU의 전역 메소드인 `expect()` 메소드를 사용합니다. `expect()` 메소드의 인자로 테스트 대상이 되는 값을 넣고요, 의도한 값을 인자로 받는 [`toBe()`](https://jestjs.io/docs/using-matchers#common-matchers) 메소드를 체이닝합니다.

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

## 2. 구현 코드와 결합된 테스트 코드 피하기: DOM 접근시 Attribute 셀렉터 사용하기

엘리먼트에 접근할 때 Class나 Id가 아닌 Attribute 셀렉터를 사용하는 이유는, Class와 Id는 변하기 때문입니다. 변하기 쉬운 구현 코드와 결합된 테스트 코드는 좋지 않습니다. 이게 무슨 말이냐면, Class 셀렉터를 사용해서 `DOMWrapper`를 얻는 테스트 코드를 작성하면, 나중에 Class 네이밍을 변경하거나 새로운 CSS 라이브러리를 도입하여 리팩토링을 진행하게 될 때 테스트 코드도 같이 변경해줘야하기 때문에 불필요한 관리포인트가 발생하는 것입니다. 다음은 [클린 아키텍처](http://www.yes24.com/Product/Goods/77283734)에서 발췌한 내용입니다.

> 상용 클래스나 메서드 중 하나라도 변경되면 딸려 있는 다수의 테스트가 변경되어야 한다. 결과적으로 테스트는 깨지기 쉬워지고, 이로 인해 상용 코드를 뻣뻣하게 만든다.
> 테스트 API의 역할은 애플리케이션의 구조를 테스트로부터 숨기는 데 있다. 이렇게 만들면 상용 코드를 리팩터링하거나 진화시키더라도 테스트에는 전혀 영향을 주지 않는다. 또한 테스트를 리팩터링하거나 진화시킬 때도 상용 코드에는 전혀 영향을 주지 않는다.

<br>

VTU 공식문서에서는 Class나 Id 대신 `data-test` Attribute 셀릭터를 사용하라고 권장하네요.

> Using data-test selectors is not required, but it can make your tests less brittle. classes and ids tend to change or move around as an application grows - by using data-test, it's clear to other developers which elements are used in tests, and should not be changed. - [Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html#the-first-test-a-todo-is-rendered)

<br>

## 3. Vue의 DOM 업데이트를 비동기로 테스트하기

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

## 4. AAA 패턴 (Arrange, Act, Assert)

위 섹션에서 사용한 예제를 다시 보면, 전형적인 AAA(Arrange, Act, Assert) 패턴의 테스트 코드 입니다.

- Arrange: 시나리오를 세업하는 단계, Vuex 스토어를 생성하거나
- Act: 사용자의 행동을 시뮬레이션
- Assert: 컴포넌트가 Act 후 어떤 상태여야 하는지에 대한 명세

<br>

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

## 5. DOM 엘리먼트의 렌더링 Assert 하기: `find().exists()`로 존재 Asset, `get().isVisible()`로 보임 Asset

### 5-1. `find().exists()`로 존재 Asset

특정한 DOM 엘리먼트가 존재하는지 확인하려면 [`find()`](https://next.vue-test-utils.vuejs.org/api/#find)와 [`exsits()`](https://next.vue-test-utils.vuejs.org/api/#exists) 메소드를 사용합니다. `find()` 메소드가 `DOMWrapper` 객체를 반환했다면 `exists()` 메소드는 `true`를 반환합니다. `boolean` 타입으로 심플하게 검증할 수 있습니다.

```typescript
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - DOM Exists", () => {
	const wrapper = mount(HelloWorld);

	expect(wrapper.find("[data-test='admin']").exists()).toBe(true);
});
```

<br>

`get()` 메소드를 사용하여 엘리먼트의 존재여부를 검증할 수도 있지만 VTU에서 권장하지 않습니다.

> `get()` works on the assumption that elements do exist and throws an error when they do not. It is not recommended to use it for asserting existence. - [Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/conditional-rendering.html#using-find-and-exists)

<br>

### 5-2. `get().isVisible()`로 보임 Asset

DOM 엘리먼트가 실제로 보이는 상태인지 검사하려면 `get()`과 `isVisible()` 메소드를 사용합니다. `isVisible()`로 보이는지 여부를 검사할 때는 이미 DOM 엘리먼트가 존재한다고 가정하기 때문에 `find()`가 아닌 `get()`을 사용합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - DOM is Visible", () => {
	const wrapper = mount(HelloWorld);

	expect(wrapper.get("[data-test='admin']").isVisible()).toBe(true);
});
```

<br>

## 6. Mount 옵션으로 Vue 컴포넌트에 옵션 부여하기

`mount()` 메소드의 두 번째 인자로 [Mount 옵션](https://next.vue-test-utils.vuejs.org/api/#mount)을 줄 수 있습니다. 그 중 [`data` 옵션](https://next.vue-test-utils.vuejs.org/guide/essentials/passing-data.html)은 Vue 컴포넌트에 `data` 옵션을 부여하고 기존의 옵션들과 Merge되며, 중복되는 옵션들을 Overwrite 합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - DOM Exists", () => {
	const wrapper = mount(HelloWorld, {
		data() {
			return {
				isAdmin: true
			}
		}
	});

	expect(wrapper.find("[data-test='admin']").exists()).toBe(true);
});
```

<br>

위의 예제 테스트 코드에서는 컴포넌트를 Mount하면서 `isAdmin: true` 데이터를 부여했는데요, `isAdmin` 값에 따라 특정 엘리먼트가 동적으로 렌더링되는 경우에 유용합니다. 예를 들어 `<div v-if="isAdmin" data-test="admin"></div>`라는 엘리먼트가 있다면, 위의 테스트를 통과하기 위해 `isAdmin` 값이 [Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)여야 합니다.

<br>

## 7. 테스트 코드에서의 이벤트 핸들링: `emitted()`, Form Submit 테스트

### 7-1. `emitted()`

`emitted()` 메소드를 인자 없이 호출하면, 발생한 모든 이벤트의 이름을 Key로 하는 `Record<string, unknown[]>` 객체를 반환합니다. 인자로 이벤트 이름을 주면, 해당 이벤트가 발생한 횟수만큼을 길이로 하는 배열을 반환합니다. 다음은 컴포넌트 내의 버튼을 클릭하면 `increment` 이벤트가 Emit 된다고 가정하는 예제입니다. 아래와 같이 버튼을 두 번 클릭시키면, `increment` 이벤트가 두 번 Emit 되리라고 Assert 할 수 있겠죠. 그럼 `wrapper.emitted("increment")`는 두 번의 Emit된 데이터를 각 요소로 하는 배열을 반환합니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import Incrementor from "@/components/Incrementor.vue";

test("Incrementor - Event Emitted", () => {
	const wrapper = mount(Incrementor);

	wrapper.find("button").trigger("click");
	wrapper.find("button").trigger("click");

	// Assert increment event will be emitted
	const incrementEvent = wrapper.emitted("increment");
	expect(incrementEvent).toHaveLength(2);
});
```

<br>

만약 `increment` 이벤트가 Emit될 때 실제로 어떤 값이 Increment 되는지, 그 값이 이벤트와 함께 Emit 되는지 테스트하려면 다음과 같이 테스트 코드를 추가할 수 있습니다.

```typescript
expect(incrementEvent[0]).toEqual([1]);
expect(incrementEvent[1]).toEqual([2]);
```

<br>

각 이벤트와 함께 Emit된 값이 배열에 담긴 것에 유의하세요. 이 예제에서 `wrapper.emitted("increment")`는 `[[1], [2]]`를 반환합니다.

<br>

이제 위의 명세 테스트를 통과하는 기능은 다음과 같이 작성할 수 있겠습니다. [Composition API]()를 사용한다면, `this.$emit()`이 아닌 `context.emit()`을 호출할텐데요, 두 경우 모두 VTU의 `emitted()` 메소드를 사용하여 테스트할 수 있습니다.

```vue
<!-- Incrementor.vue-->
<template>
	<button type="button" @click="handleClick">Increment</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "HelloWorld",
  data() {
	return {
		count: 0
	}
  },
  methods: {
	  handleClick() {
		  this.count += 1;
		  this.$emit("increment", this.count);
	  }
  }
});
</script>
```

<br>

### 7-2. Form Submit 테스트

Form Submit 테스트를 간단하게 하는 방법은 `submit` 대신 `click` 이벤트를 Trigge 하는 것입니다. [HTML 명세](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#form-submission-algorithm)에 따르면, `document` 객체에 연결되지 않은 Form은 Submit될 수 없기 때문입니다. `submit` 이벤트를 사용하여 테스트하고 싶다면 [`attachTo`](https://next.vue-test-utils.vuejs.org/api/#attachto)를 사용해서 `document` 객체와 Form 엘리먼트를 연결해야합니다.

<br>

이 외 Form 엘리먼트에 대한 이벤트 핸들링 테스트를 더 알아보시려면 [Vue Test Utils 공식문서의 Interacting with Vue Component inputs](https://next.vue-test-utils.vuejs.org/guide/essentials/forms.html#interacting-with-vue-component-inputs) 섹션을 참고하세요.

<br>

---

### References

- [A Crash Course | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html)
- [Conditional Rendering | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/conditional-rendering.html)
- [Test your VueJS + TypeScript application - Vincent Francolin](https://medium.com/codex/test-your-vuejs-typescript-application-b7dc9133e6f)
- [Test your VueJS + TypeScript application; part 2 - Vincent Francolin](https://vince-f.medium.com/test-your-vuejs-typescript-application-part-2-acaa5d8ba327)
