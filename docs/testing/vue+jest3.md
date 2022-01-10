# 프론트엔드 테스트하기 3: HTTP 통신 테스트 (feat. Axios, Vuex)

> This doc is WIP ..

<br>

1. Axios Mocking 하기: `jest.mock()`, Mock 데이터 변수명 `mock-` Prefix
2. Promise Resolve 하기: `flushPromises()`
3. Vuex Action 테스트하기

<br>

## 1. Axios Mocking 하기: `jest.mock()`, Mock 데이터 변수명 `mock-` Prefix

### 1-1. `jest.mock()`

[`jest.mock()`](https://jestjs.io/docs/mock-functions#mocking-modules) 전역 메소드를 사용하여 테스트 코드가 실행될 때 실제 API가 아닌 Mock API가 호출되도록 할 수 있습니다. `jest.mock()` 메소드는 JavaScript 모듈을 실제가 아닌 Mock으로 대체하는 메소드인데요, HTTP 클라이언트로 독보적인 [Axios](https://axios-http.com/docs/intro) 사용을 예로 들면, `jest.mock("axios")`를 호출하여 `axios` 모듈을 Mock으로 대체할 수 있습니다. `jest.mock()`의 첫 번째 인자로는 Mock 하려는 모듈의 경로를 넘겨줍니다.

<br>

`jest.mock()`의 두 번째 인자로는 실제 API 대신 호출될 Mock 함수를 반환하는 [Factory 함수](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1)를 넘겨줍니다. 실제 API가 호출되는 환경을 Mimic 하기 위해 함수를 반환하는 함수인 [고차함수(HOF)](https://en.wikipedia.org/wiki/Higher-order_function)를 사용한다는 점에 주목하세요! Mock 함수는 [`jest.fn()`](https://jestjs.io/docs/mock-functions#mock-implementations) 메소드를 사용하여 만들 수 있습니다. `axios` 모듈의 `get`, `post`, `patch`, `delete` 등 각 메소드를 Mock 함수로 대체하면 되는데요, `jest.fn(() => mockUserList)`는 `mockUserList`를 반환하는 Mock 함수를 반환합니다. 아래와 같이 작성하면, 이제 모든 `axios.get()` 호출시 Mock 함수가 대신 호출되면서 `mockUserList`를 응답합니다.

```typescript
// userList.spec.ts
const mockUserList = [
  { id: 1, name: "Yujin" },
  { id: 2, name: "Sohyun" }
]

jest.mock("axios", () => ({
  get: jest.fn(() => mockUserList)
}));
```

<br>

### 1-2. Mock 데이터 변수명 `mock-` Prefix

주의할 점은 `jest.mock()`의 두 번째 인자로 넘긴 Factory 함수에서 `mockUserList` 변수에 접근할 수 없다는 것입니다. `jest.mock()`의 호출이 [호이스팅](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)될 때 `mockUserList` 변수는 아직 선언되지 않았기 때문입니다. [Mock 데이터가 할당된 변수명이 `mock-` Prefix를 포함하면 Jest에서 이 문제를 알아서 처리](https://jestjs.io/docs/es6-class-mocks#calling-jestmock-with-the-module-factory-parameter)해줍니다.

<br>

## 2. Promise Resolve 하기: `flushPromises()`

Vue에서 비동기 DOM 업데이트를 기다릴 때는 `nextTick()` 메소드를 사용했죠. Vue의 가상 DOM 모델에서 실제 DOM 업데이트는 이벤트 루프의 틱 단위로 일어나기 때문입니다. 하지만 Vue-related 하지 않은 비동기 동작을 핸들링할 때는, 대표적으로 `axios`에서 제공하는 메소드들은 `Promise` 기반인데요, `Promise`를 즉시 Resolve 하는 VTU의 [`flushPromises()`](https://next.vue-test-utils.vuejs.org/api/#flushpromises) 메소드를 사용하면 됩니다. `flushPromises()`는 다음 코드를 실행하기 전, 모든 `Promise`를 Resolve 하기 때문에 HTTP 통신과 DOM 업데이트를 포함한 모든 비동기 동작이 완료되었음을 Assert 할 수 있게 합니다.

```typescript
// userList.spec.ts
import { mount, flushPromises } from "@vue/test-utils";
import axios from "axios";
import UserList from "@/views/UserList.vue";

const mockUserList = [
  { id: 1, name: "Yujin" },
  { id: 2, name: "Sohyun" }
]

jest.mock("axios", () => ({
  get: jest.fn(() => mockUserList)
}));

test("UserList - Fetch on Button Click", async () => {
	// Arrange
	const wrapper = mount(UserList);

	await wrapper.get("button").trigger("click");

	expect(axios.get).toHaveBeenCalledTimes(1);
	expect(axios.get).toHaveBeenCalledWith('/api/users');

	await flushPromises();
});
```

<br>

## 3. Vuex Action 테스트하기

### 3-1. 실제 Vuex Store 사용하기

중요! [Vuex](https://next.vuex.vuejs.org/)는 구현 세부사항이기 때문에 Vuex 자체를 테스트하지는 않습니다.

```typescript
import { createStore } from "vuex";

const store = createStore({
  state() {
    return {
      count: 0
    }
  },
  mutations: {
    increment(state: any) {
      state.count += 1
    }
  }
});
```

<br>

---

### References

- [Asynchronous Behavior | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/advanced/async-suspense.html)
- [Making HTTP requests | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/advanced/http-requests.html)
- [Testing Vuex | Vue Test Utils for Vue 3](https://next.vue-test-utils.vuejs.org/guide/advanced/vuex.html)
