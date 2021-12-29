# Vue + Jest 단위테스트 자동화하기

> This doc is not done, under research and using-by-myself currently.

<br>

1. Jest 설치 & 설정하기

<br>

## 1. Jest 설치 & 설정하기

### 1-1. Jest

[Jest](https://jestjs.io/)는 JavaScript 테스트 라이브러리입니다.

<br>

### 1-2. Jest 설치하기

다음과 같이 패키지매니저를 사용해서 설치하거나,

```zsh
yarn add jest --dev
```

<br>

[Vue 테스트 유틸](https://vue-test-utils.vuejs.org/) 라이브러리를 통해 Jest를 사용할 수 있습니다. Vue 테스트 유틸은 Vue에서 공식 지원하는 단위 테스트 라이브러리입니다. [`@vue/cli`](https://cli.vuejs.org/) 3.x 이후 버전을 사용하여 Vue 앱을 새로 생성하는 경우라면, 앱 생성시 단위테스트 툴로 Jest를 선택하여 쉽게 셋업할 수 있습니다.

<br>

기존 프로젝트에 Vue 테스트 유틸을 추가하는 경우라면, 아래와 같이 패키지매니저를 사용하여 필요한 라이브러리들을 직접 추가합니다.

```zsh
yarn add jest @vue/test-utils vue-jest babel-jest --dev
```

<br>

### 1-3. Jest 설정하기: `jest.config.js`

Jest 설정파일은 `jest.config.js`입니다. `@vue/cli`를 사용하여 앱을 생성할 때 `Unit Testing` 도구로 `Jest`를 선택했다면, `jest.config.js` 파일이 자동으로 생성되고 기본값들이 세팅됩니다. `jest.config.js` 대신 `package.json` 파일의 `jest` 필드를 사용할 수도 있습니다.

```javascript
// jest.config.js
// 이 파일은 @vue/cli로 설치 시 기본 세팅됩니다

module.exports = {
	// 주의! preset 지정 후 아래와 같이 각각 다시 설정하는 경우, 새로 설정한 내용으로 적용됩니다
	preset: "@vue/cli-plugin-unit-jest",
	moduleFileExtensions: [
		"js",
		"json",
		// 모든 vue 파일(`*.vue`)을 처리하기 위해 Jest에게 알려줍니다
		"vue",
	],
	transform: {
		// `vue-jest`를 사용하여 모든 vue 파일(`*.vue`)을 처리합니다
		".*\\.(vue)$": "vue-jest",
		// `babel-jest`를 사용하여 모든 js 파일(`*.js`)을 처리합니다
		".*\\.(js)$": "babel-jest",
	},
	moduleNameMapper: {
		// 별칭 @(프로젝트/src) 사용하여 하위 경로의 파일을 맵핑합니다
		"^@/(.*)$": "<rootDir>/src/$1",
	},
	testMatch: [
		// __tests__ 경로 하위에 있는 모든 js/ts/jsx/tsx 파일을 테스트 대상으로 지정합니다
		"**/__tests__/**/*.[jt]s?(x)",
		// 'xxx.spec' 또는 'xxx.test'라는 이름의 모든 js/ts/jsx/tsx 파일을 테스트 대상으로 지정합니다
		"**/?(*.)+(spec|test).[jt]s?(x)",
	],
	// node_modules 경로 하위에 있는 모든 테스트 파일을 대상에서 제외합니다
	testPathIgnorePatterns: ["/node_modules/"],
	collectCoverage: true,
	collectCoverageFrom: ["**/*.{js,vue}", "!**/node_modules/**"],
};
```

<br>

---

### References

- [모던 프론트엔드 테스트 전략 — 1편(Testing Overview) | 콴다 팀블로그](https://blog.mathpresso.com/%EB%AA%A8%EB%8D%98-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A0%84%EB%9E%B5-1%ED%8E%B8-841e87a613b2)
- [테스트 | TOAST UI](https://ui.toast.com/fe-guide/ko_TEST)
- [Unit Testing with Jest | Cracking Vue.js](https://joshua1988.github.io/vue-camp/testing/jest-testing.html#jest-%E1%84%89%E1%85%A9%E1%84%80%E1%85%A2)
- [UnitTest - Martin Fowler](https://martinfowler.com/bliki/UnitTest.html)
- [TestDouble - Martin Fowler](https://martinfowler.com/bliki/TestDouble.html)
- [Mocks Aren't Stubs - Martin Fowler](https://martinfowler.com/articles/mocksArentStubs.html)
- [SelfTestingCode - Martin Fowler](https://martinfowler.com/bliki/SelfTestingCode.html)
- [IntegrationTest - Martin Fowler](https://martinfowler.com/bliki/IntegrationTest.html)
- [BroadStackTest - Martin Fowler](https://martinfowler.com/bliki/BroadStackTest.html)
- [ExtremeProgramming - Martin Fowler](https://martinfowler.com/bliki/ExtremeProgramming.html)
- [TestDrivenDevelopment - Martin Fowler](https://martinfowler.com/bliki/TestDrivenDevelopment.html)
- [Docs | Testing Library](https://testing-library.com/docs/)
