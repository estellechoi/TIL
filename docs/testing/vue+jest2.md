# 프론트엔드 테스트하기 2: Vue 컴포넌트 테스트 Basics, 구현 세부사항을 테스트하지 말 것

<br>

1. 렌더링된 컴포넌트의 DOM에 접근해서 테스트하기
2. 구현 세부사항을 테스트하지 말 것: Class나 Id 사용 피하기, 사용자 행동 Mimic 하기
3. Vue의 DOM 업데이트를 비동기로 테스트하기
4. AAA 패턴 (Arrange, Act, Assert)
5. DOM 엘리먼트 렌더링 테스트: `find().exists()`, `get().isVisible()`
6. 테스트 코드에서 Vue 컴포넌트에 데이터 전달하기: Data, Props, Slots
7. 이벤트 핸들링 테스트: `emitted()`, Form Submit 테스트

<br>

## 1. 렌더링된 컴포넌트의 DOM에 접근해서 테스트하기

### 1-1. 테스트 파일 만들기

Vue 컴포넌트가 무사히 렌더링되는지 확인하는 간단한 테스트 코드를 작성해보겠습니다. 테스트 파일의 이름과 경로는 Jest 설정파일에 명시한 포맷을 따라야합니다. `jest.config.js` 파일에서 Preset을 사용한다면 해당 Preset의 `testMatch` 필드를 확인해보시고요, 또는 직접 해당 필드에 명시한 포맷에 따라 파일명을 정하면 됩니다. 아무것도 지정하지 않았다면 디폴트 포맷은 다음과 같고, 이 경우 테스트 파일의 이름은 `sample.spec.ts` 이런식이 되겠죠.

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

### 1-3. `get()` 메소드로 DOM 엘리먼트에 접근하기

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

## 2. 구현 세부사항을 테스트하지 말 것: Class나 Id 사용 피하기, 사용자 행동 Mimic 하기

### 2-1. Class나 Id 사용 피하기

위 섹션에서 엘리먼트에 접근할 때 Class나 Id가 아닌 Attribute 셀렉터를 사용한 이유는, Class와 Id는 변하기 때문입니다. 다시 말해 Class와 Id가 구현 세부사항이기 때문인데요, 테스트에 관한 유명한 말이 있죠. [Do not test implementation details](https://next.vue-test-utils.vuejs.org/guide/essentials/easy-to-test.html#do-not-test-implementation-details): 구현 세부사항을 테스트하지 말 것. 구현 세부사항은 변하기 쉽기 때문에 구현 세부사항과 결합된 테스트 코드는 좋지 않다고 봅니다. 만약 Class 셀렉터를 사용해서 `DOMWrapper`를 얻는 테스트 코드를 작성하면, 나중에 Class 네이밍을 변경하거나 새로운 CSS 라이브러리를 도입하여 리팩토링을 진행하게 될 때 테스트 코드도 같이 변경해줘야하기 때문에 불필요한 관리포인트가 발생하겠죠. 테스트 코드에서는 아래와 같이 Class 셀렉터를 사용해서 DOM에 접근하지 맙시다!

```typescript
const wrapper = mount(Counter)
const paragraph = wrapper.find('.paragraph');
```

<br>

[VTU 공식문서]((https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html#the-first-test-a-todo-is-rendered))에서는 Class나 Id 대신 `data-test` Attribute 셀릭터를 사용하라고 권장하네요.

> Using data-test selectors is not required, but it can make your tests less brittle. classes and ids tend to change or move around as an application grows - by using data-test, it's clear to other developers which elements are used in tests, and should not be changed.

<br>

### 2-2. 사용자 행동 Mimic 하기

다음과 같이 `setData()`를 사용하여 데이터를 강제로 변경하는 코드 역시 구현 세부사항을 포함하기 때문에 좋지 않습니다.

```typescript
await wrapper.setData({ count: 2 })
```

<br>

위의 테스트 코드는 이렇게 사용자의 행동을 Mimic 하는 식으로 리팩토링할 수 있습니다.

```typescript
const button = wrapper.find('button');
await button.trigger('click');
await button.trigger('click');
```

<br>

다음은 [클린 아키텍처](http://www.yes24.com/Product/Goods/77283734)에서 발췌한 단락인데요, 테스트 코드가 애플리케이션의 구현 사항과 결합되어있으면 리팩토링을 해나감에 따라 테스트 코드가 언제든 깨질 수 있다는 내용입니다.

> 상용 클래스나 메서드 중 하나라도 변경되면 딸려 있는 다수의 테스트가 변경되어야 한다. 결과적으로 테스트는 깨지기 쉬워지고, 이로 인해 상용 코드를 뻣뻣하게 만든다.
> 테스트 API의 역할은 애플리케이션의 구조를 테스트로부터 숨기는 데 있다. 이렇게 만들면 상용 코드를 리팩터링하거나 진화시키더라도 테스트에는 전혀 영향을 주지 않는다. 또한 테스트를 리팩터링하거나 진화시킬 때도 상용 코드에는 전혀 영향을 주지 않는다.

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

Vue에서는 다음 이벤트 루프 틱이 실행되어 DOM이 업데이트될 때까지 기다리는 [`nextTick()`](https://v3.vuejs.org/api/global-api.html#nexttick) 메소드를 제공하는데요, 그래서 위의 테스트는 아래와 같이 작성할 수도 있습니다. VTU에서 DOM을 업데이트하는 `trigger()`, `setValue()` 같은 메소드들이 `nextTick`을 반환하기 때문에 위의 테스트 코드는 축약형을 사용한 것이죠. [VTU 공식문서](https://next.vue-test-utils.vuejs.org/guide/advanced/async-suspense.html#a-simple-example-updating-with-trigger)를 참고하시면 좋을 것 같습니다!

```typescript
import { nextTick } from "vue"
// ..

wrapper.get("[data-test='input-name']").setValue("Yujin Choi");
await nextTick();
```

<br>

## 4. AAA 패턴 (Arrange, Act, Assert)

위 섹션에서 사용한 예제를 다시 보면, 전형적인 AAA(Arrange, Act, Assert) 패턴의 테스트 코드 입니다.

- Arrange: 시나리오를 셋업하는 단계, Vuex 스토어를 생성하는 것도 Arrange에 해당
- Act: 사용자 행동을 시뮬레이션, 값을 입력하거나 버튼을 클릭하는 행동을 Mimic
- Assert: 컴포넌트가 Act 후 어떤 상태여야 하는지에 대한 명세를 단언

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

## 5. DOM 엘리먼트 렌더링 테스트: `find().exists()`, `get().isVisible()`

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

`get()` 메소드를 사용하여 엘리먼트의 존재여부를 검증할 수도 있지만 VTU에서 권장하지 않습니다. `get()` 메소드는 어떤 DOM 엘리먼트가 정적으로 렌더링되기 때문에 항상 존재한다고 믿을 때 사용합니다.

> `get()` works on the assumption that elements do exist and throws an error when they do not. It is not recommended to use it for asserting existence. - [Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/conditional-rendering.html#using-find-and-exists)

<br>

### 5-2. `get().isVisible()`로 보임 Asset

DOM 엘리먼트가 존재는 하나, 실제로 Visible 상태인지 검사하려면 `get()`과 [`isVisible()`](https://next.vue-test-utils.vuejs.org/api/#isvisible) 메소드를 사용합니다. `isVisible()`로 보이는지 여부를 검사할 때는 이미 DOM 엘리먼트가 존재한다고 가정하기 때문에 `find()`가 아닌 `get()`을 사용합니다.

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

## 6. 테스트 코드에서 Vue 컴포넌트에 데이터 전달하기: Data, Props, Slots

### 6-1. Data 전달하기

`mount()` 메소드의 두 번째 인자로 [Mount 옵션](https://next.vue-test-utils.vuejs.org/api/#mount)을 넘겨서 테스트하려는 Vue 컴포넌트에 데이터를 전달할 수 있습니다. `data`, `props` 같은 [Vue 컴포넌트 옵션](https://v3.vuejs.org/guide/instance.html#component-instance-properties)을 통제할 수 있죠. Mount 옵션은 컴포넌트가 이미 갖고있는 옵션들과 Merge되며, 중복되는 옵션 Key들을 Overwrite 합니다. 아래의 예제 테스트 코드에서는 컴포넌트를 Mount하면서 `data` 옵션을 사용하여 `isAdmin: true` 데이터를 넘겼는데요, `isAdmin` 값에 따라 특정 엘리먼트가 동적으로 렌더링되는 경우 이를 테스트할 때 유용합니다. 예를 들어 `<div v-if="isAdmin" data-test="admin">`라는 엘리먼트가 있다고 가정해보겠습니다. 위의 테스트를 통과하려면 `isAdmin` 값이 `true`이므로 해당 엘리먼트가 렌더링되어야겠죠. 반대로 `isAdmin`이 `false`일 때는 렌더링되면 안됩니다!

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - Dynamic Rendering", () => {
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

하지만 Mount 옵션은 `data`와 `props`의 초기값을 세팅할 때만 유용합니다. 만약 컴포넌트가 Mount 된 후 변경된 데이터를 나중에 넘겨야할 때는 [`setData()`](https://next.vue-test-utils.vuejs.org/api/#setdata) 메소드를 사용합니다.

<br>

### 6-2. Props 전달하기

`props` 데이터도 마찬가지로 Mount 옵션으로 넘기면 됩니다. Mount 이후 변경된 Prop은 [`setProps()`](https://next.vue-test-utils.vuejs.org/api/#setprops) 메소드를 사용하여 전달합니다. 어떤 컴포넌트가 `show`라는 `props`을 갖고있으며, `show`의 초기값이 `true`라고 가정해보겠습니다. 그리고 `show === true` 일 때만 `Hello World`라는 텍스트를 보여준다고 해볼게요. 이 명세에 대한 테스트 코드는 다음과 같이 작성할 수 있습니다. 필요한 경우 같은 방식으로 를 사용하면 됩니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - Dynamic Rendering", async () => {
	const wrapper = mount(HelloWorld);
	expect(wrapper.html()).toContain("Hello World");

	await wrapper.setProps({ show: false });

	expect(wrapper.html()).not.toContain("Hello World");
});
```

<br>

### 6-3. Slot 사용하기

Vue에서는 [Slot](https://v3.vuejs.org/guide/component-slots.html)을 사용하여 컴포넌트의 특정 부분에 HTML을 전달하는데요, 컴포넌트를 테스트할 때는 VTU의 Mount 옵션 중 [`slots`](https://next.vue-test-utils.vuejs.org/guide/advanced/slots.html#slots) 옵션을 사용하면 됩니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import HelloWorld from "@/components/HelloWorld.vue";

test("HelloWorld - Slot", () => {
	const wrapper = mount(HelloWorld, {
		slots: {
			default: "Main Content"
		}
	});

	expect(wrapper.html()).toContain("Main Content");
});
```

<br>

[Named Slot](https://v3.vuejs.org/guide/component-slots.html#named-slots)을 가진 컴포넌트는 다음과 같이 테스트하고요, 값으로는 Vue의 렌더링 함수인 `h`를 사용하거나 Vue의 SFC를 그대로 지정할 수도 있습니다.

```typescript
// helloworld.spec.ts
import { mount } from "@vue/test-utils";
import { h } from "vue";
import HelloWorld from "@/components/HelloWorld.vue";
import Header from "@/views/Header.vue";

test("HelloWorld - Named Slots", () => {
	const wrapper = mount(HelloWorld, {
		slots: {
			header: Header,
			main: h("div", "Main Content"),
			sidebar: { template: "<div>Sidebar</div>" },
			footer: "<div>Footer</div>",
		}
	});

	expect(wrapper.html()).toContain("Main Content");
});
```

<br>

## 7. 이벤트 핸들링 테스트: `emitted()`, Form Submit 테스트

### 7-1. `emitted()`

`emitted()` 메소드를 인자 없이 호출하면, 발생한 모든 이벤트의 이름을 Key로 하는 `Record<string, unknown[]>` 객체를 반환합니다. 인자로 이벤트 이름을 주면, 해당 이벤트가 발생한 횟수만큼을 길이로 하는 배열을 반환합니다. 다음은 컴포넌트 내의 버튼을 클릭하면 `increment` 이벤트가 Emit 된다고 가정하는 예제입니다. 아래와 같이 버튼을 두 번 클릭시키면, `increment` 이벤트가 두 번 Emit 되리라고 Assert 할 수 있겠죠. 그럼 `wrapper.emitted("increment")`는 두 번의 Emit된 데이터를 각 요소로 하는 배열을 반환합니다.

```typescript
// incrementor.spec.ts
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

만약 `increment` 이벤트가 Emit될 때 실제로 어떤 값이 Increment 되는지, 그 값이 이벤트와 함께 Emit 되는지 테스트하려면 다음과 같이 테스트 코드를 추가할 수 있습니다. 각 이벤트와 함께 Emit된 값이 배열에 담긴 것에 유의하세요. 이 예제에서 `wrapper.emitted("increment")`는 `[[1], [2]]`를 반환합니다.

```typescript
expect(incrementEvent[0]).toEqual([1]);
expect(incrementEvent[1]).toEqual([2]);
```

<br>

이제 위의 테스트를 통과하는 기능은 다음과 같이 작성할 수 있겠습니다. [Composition API]()를 사용한다면, `this.$emit()`이 아닌 `context.emit()`을 호출할텐데요, 두 경우 모두 VTU의 `emitted()` 메소드를 사용하여 테스트할 수 있습니다.

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

Form Submit 테스트를 간단하게 하는 방법은 `submit` 대신 `click` 이벤트를 Trigger 하는 것입니다. [HTML 명세](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#form-submission-algorithm)에 따르면, `document` 객체에 연결되지 않은 Form은 Submit될 수 없기 때문입니다. `submit` 이벤트를 사용하여 테스트하고 싶다면 [`attachTo`](https://next.vue-test-utils.vuejs.org/api/#attachto)를 사용해서 `document` 객체와 Form 엘리먼트를 연결해야합니다.

```typescript
// form.spec.ts
import { mount } from "@vue/test-utils";
import Form from "@/components/Form.vue";

document.body.innerHTML = `<div id="app"></div>`;

test("Form - Submit", async () => {
	// Arrange
	const wrapper = mount(Form, {
		attachTo: document.getElementById("app")
	});
	const email = "abc@gmail.com";

	// Act
	await wrapper.find('input[type=email]').setValue(email);
	await wrapper.find("form").trigger("submit");

	// Assert
	const submitEvent = wrapper.emitted("submit");
	expect(submitEvent[0][0]).toStrictEqual({
		email
	});
});
```

<br>

Submit 버튼 외에 Form 엘리먼트에 대한 이벤트 핸들링 테스트를 더 알아보시려면 [Vue Test Utils 공식문서의 Interacting with Vue Component inputs](https://next.vue-test-utils.vuejs.org/guide/essentials/forms.html#interacting-with-vue-component-inputs) 섹션을 참고하세요.

<br>

---

### References

- [A Crash Course | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/a-crash-course.html)
- [Conditional Rendering | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/conditional-rendering.html)
- [Testing Emitted Events | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/event-handling.html)
- [Testing Forms | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/forms.html)
- [Passing Data to Components | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/passing-data.html)
- [Write components that are easy to test |  Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/essentials/easy-to-test.html)
- [Slots | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/advanced/slots.html)
- [Test your VueJS + TypeScript application - Vincent Francolin](https://medium.com/codex/test-your-vuejs-typescript-application-b7dc9133e6f)
- [Test your VueJS + TypeScript application; part 2 - Vincent Francolin](https://vince-f.medium.com/test-your-vuejs-typescript-application-part-2-acaa5d8ba327)
