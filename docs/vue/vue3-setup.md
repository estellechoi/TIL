# Vue3 + TypeScript 프로젝트 셋업하기

<br>

## 1. `@vue/cli` 버전 업데이트

기존에 전역 설치된 `@vue/cli`가 있다면, 아래 명령어를 사용하여 최신 버전으로 업데이트합니다.

```
# npm
npm update -g @vue/cli

# yarn
yarn global upgrade --latest @vue/cli
```

<br>

버전 업데이트가 정상적으로 실행되었는지 확인하기 위해 아래 명령어를 사용하여 버전을 확인합니다.

```
vue --version
```

<br>

만약 버전 업데이트가 되지 않았다면 기존에 설치된 `@vue/cli`를 삭제한 후 다시 설치하는 방법이 있습니다. 아래와 같이 패키지매니저 명령어를 사용하여 삭제하시거나,

```
# npm
npm uninstall -g @vue/cli

# yarn
yarn global remove @vue/cli
```

<br>

위 방법도 정상 작동하지 않는다면, 전역 설치된 `@vue/cli`가 위치한 아래의 경로로 이동합니다. (MacOS 기준)

```
# npm
cd /usr/local/lib/node_modules

# yarn
cd ~/.config/yarn/global/node_modules
```

<br>

그다음 `ls` 명령어를 사용하여 `@vue` 폴더가 조회되는지 확인하신 후, 아래 명령어를 사용하여 기존에 설치되어있던 `@vue` 라이브러리 폴더를 삭제합니다.

```
rm -rf @vue
```

<br>

그리고 재설치.

```
#npm
npm install -g @vue/cli

# yarn
yarn global add @vue/cli
```

<br>

## 2. Vue3 프로젝트 생성

이제 `@vue/cli`를 사용하여 Vue 프로젝트를 생성합니다.

```
vue create project-name
```

<br>

프로젝트 생성 시작. 아래는 제가 `@vue/cli`의 `create` 프로세스를 따르면서 선택한 옵션들입니다.

<img src="./../img/vue3-setup-1.png"  />

<br>

아래 단계에서 `TypeScript`를 포함하고요,

<img src="./../img/vue3-setup-2.png"  />

<br>

아래 질문에 `3.x`를 선택하여 Vue3 프로젝트를 생성해주세요.

<img src="./../img/vue3-setup-3.png"  />
<img src="./../img/vue3-setup-4.png"  />
<img src="./../img/vue3-setup-5.png"  />
<img src="./../img/vue3-setup-6.png"  />
<img src="./../img/vue3-setup-7.png"  />

<br>

다음 단계에서 [`ESLint`](https://eslint.org/)를 선택합니다. [`TSLint`](https://www.npmjs.com/package/tslint)는 Deprecation을 공식 선언했습니다.

<img src="./../img/vue3-setup-8.png"  />
<img src="./../img/vue3-setup-9.png"  />
<img src="./../img/vue3-setup-10.png"  />

<br>

## 3. `package.json`

프로젝트 생성이 완료되면 다음 명령어를 사용하여 프로젝트 루트 경로로 이동합니다.

```
cd project-name
```

<br>

그 다음 프로젝트 루트 경로에 있는 `package.json` 파일을 확인해보면, `@vue/cli`를 사용하여 프로젝트 생성시 선택했던 옵션들에 따라 기본 셋업이 되어있습니다.

```json
{
	"name": "project-name",
	"version": "0.1.0",
	"private": true,
	"scripts": {
		"serve": "vue-cli-service serve",
		"build": "vue-cli-service build",
		"lint": "vue-cli-service lint"
	},
	"dependencies": {
		"core-js": "^3.6.5",
		"register-service-worker": "^1.7.1",
		"vue": "^3.0.0",
		"vue-router": "^4.0.0-0",
		"vuex": "^4.0.0-0"
	},
	"devDependencies": {
		"@typescript-eslint/eslint-plugin": "^4.18.0",
		"@typescript-eslint/parser": "^4.18.0",
		"@vue/cli-plugin-babel": "~4.5.0",
		"@vue/cli-plugin-eslint": "~4.5.0",
		"@vue/cli-plugin-pwa": "~4.5.0",
		"@vue/cli-plugin-router": "~4.5.0",
		"@vue/cli-plugin-typescript": "~4.5.0",
		"@vue/cli-plugin-vuex": "~4.5.0",
		"@vue/cli-service": "~4.5.0",
		"@vue/compiler-sfc": "^3.0.0",
		"@vue/eslint-config-prettier": "^6.0.0",
		"@vue/eslint-config-typescript": "^7.0.0",
		"eslint": "^6.7.2",
		"eslint-plugin-prettier": "^3.3.1",
		"eslint-plugin-vue": "^7.0.0",
		"node-sass": "^4.12.0",
		"prettier": "^2.2.1",
		"sass-loader": "^8.0.2",
		"typescript": "~4.1.5"
	}
}
```

<br>

## 4. `shims-vue.d.ts`

`src/shims-vue.d.ts` 파일은 TypeScript가 Vue 문법을 어떻게 해석해야하는지 명시하는 파일입니다. 예를 들어, `.ts` 파일 내에서 `.vue` 포맷의 파일을 임포트하려는 경우, 이 파일의 타입을 어떻게 해석해야하는지 정의합니다.

```typescript
/* eslint-disable */
declare module "*.vue" {
	import type { DefineComponent } from "vue";
	const component: DefineComponent<{}, {}, any>;
	export default component;
}
```

<br>

---

### References

- [Installation | Vue.js](https://v3.vuejs.org/guide/installation.html#release-notes)
- [Intro to the TSConfig Reference | TypeScript](https://www.typescriptlang.org/tsconfig)
