# React 17.x + TypeScript + Redux + Tailwind CSS 프로젝트 셋업하기

<br>

1. `create-react-app` 최신 버전 확인
2. React 17.x + TypeScript + Redux 앱 생성하기
3. ESLint React 플러그인 셋업
4. Tailwind CSS 셋업

<br>

## 1. `create-react-app` 최신 버전 확인

기존에 전역 설치된 [`create-react-app`](https://github.com/facebook/create-react-app)이 있다면 삭제하고 최신 버전을 사용하라고 [공식문서](https://create-react-app.dev/docs/getting-started#quick-start)에서 권장하고 있습니다.

```zsh
# or npm uninstall -g create-react-app
yarn global remove create-react-app
```

<br>

참고로 `create-react-app` CLI의 전역 설치는 더이상 지원되지 않기 때문에, 새 React 앱을 생성할 때 `npx` 또는 `yarn`의 `yarn create` 커맨드를 사용해서 최신 버전의 `create-react-app`을 바로 사용할 수 있습니다. 이 부분은 다음 섹션에 정리했습니다.

> Global installs of create-react-app are no longer supported. - [Create React App](https://create-react-app.dev/docs/adding-typescript#installation)

<br>

## 2. React 17.x + TypeScript + Redux 앱 생성하기

### 2-1. 앱 생성시 `--template` 옵션 사용하기

저는 `yarn`을 사용했는데, [공식문서](https://create-react-app.dev/docs/adding-typescript)에 다른 패키지매니저를 사용하는 방법이 자세히 나와있기 때문에 따로 정리하지는 않겠습니다. 아래처럼 `--template` 옵션을 사용해서 TypeScript 셋업이 되어있는 앱을 생성합니다. 이렇게 하면 `yarn add typescript @types/node @types/react @types/react-dom @types/jest` 커맨드를 사용하여 TypeScript와 패키지들의 Type Declaration를 별도로 설치하는 것과 같습니다.

```zsh
yarn create react-app project-name --template typescript
```

<br>

만약 State 관리를 위해 Redux를 사용하실거라면, 다음과 같이 `--template` 옵션 값으로 `redux-typescript`를 지정하면 됩니다. 이렇게 하면 [Redux Toolkit](https://redux-toolkit.js.org/)과 [React Redux](https://react-redux.js.org/) 패키지가 함께 설치됩니다.

```zsh
yarn create react-app project-name --template redux-typescript
```

<br>

### 2-2. React 버전 낮추기

최신 버전의 `create-react-app`을 사용해서 앱을 생성하면 최신 버전의 `react` 패키지로 React 앱이 셋업됩니다. 만약 이전 버전의 React를 사용하려면 다음과 같이 몇 개 파일을 수정해주어야 하는데요, 저의 경우 `18.x` 버전의 React 앱 셋업을 `17.x` 버전으로 Downgrade 했기 때문에 제 상황을 기준으로 정리해보았습니다.

<br>

`package.json` 파일에서 다음 패키지들의 버전을 변경합니다.

- `@testing-library/react`: 테스트 러너
- `@types/**`: Types Declaration
- `react`: React
- `react-dom`: React-DOM 상호작용을 위한 패키지

<br>

변경 전

```json
"dependencies": {
  "@testing-library/react": "^13.0.0",
  "@types/react": "^18.0.0",
  "@types/react-dom": "^18.0.0",
  "react": "^18.1.0",
  "react-dom": "^18.1.0",
},
```

변경 후

```json
"dependencies": {
  "@testing-library/react": "^12.0.0",
  "@testing-library/react-hooks": "^7.0.2",
  "@types/react": "^17.0.2",
  "@types/react-dom": "^17.0.1",
  "react": "^17.0.1",
  "react-dom": "^17.0.1",
},
```

<br>

그리고 아직 React 17.x 버전에는 존재하지 않는 메소드들을 수정해줍니다. `index.tsx` 파일에서 다음 부분을 수정하면 될겁니다.

<br>

변경 전

```tsx
import ReactDOM from 'react-dom/client';
// ..

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

변경 후

```tsx
import ReactDOM from 'react-dom';
// ..

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root') as HTMLElement
);
```

<br>

이제 `node_modules` 폴더를 삭제해버리시고요, 패키지 셋업을 다시 하면 됩니다.

```zsh
yarn install
```

<br>

## 3. ESLint React 플러그인 셋업

`create-react-app`을 사용하여 React 앱 Preset을 가져오셨다면, 아마 다음과 같은 ESLint 패키지들이 추가되어있을 겁니다.

```json
"devDependencies": {
  "@typescript-eslint/eslint-plugin": "^5.27.1",
  "@typescript-eslint/parser": "^5.27.1",
  "eslint": "^8.17.0",
  "eslint-config-prettier": "^8.5.0",
  "eslint-plugin-better-styled-components": "^1.1.2",
  "eslint-plugin-prettier": "^4.0.0",
  "eslint-plugin-react": "^7.30.0",
  "eslint-plugin-react-hooks": "^4.5.0",
  "eslint-plugin-simple-import-sort": "^7.0.0",
  "eslint-plugin-unused-imports": "^2.0.0",  
}
```

<br>

그 다음 ESLint 설정 파일을 생성하신 후 ESLint 플러그인들을 추가해주면 됩니다. 다음은 React 17.x + TypeScript 조합을 사용중인 [Uniswap/interface](https://github.com/Uniswap/interface/blob/main/.eslintrc.json) 레포지토리의 ESLint 설정 파일인데, 참고하기 좋아서 가져왔습니다. 다른 부분은 `create-react-app`의 Preset을 거의 그대로 사용하므로, `extends`와 `rules` 필드를 주로 보시면 되겠습니다.

#### 패키지 업데이트 이슈

- `prettier/@typescript-eslint` 플러그인은 8.0.0 버전부터 `prettier` 패키지에 기본 기능으로 통합되었습니다. 만약 저처럼 8.x 이상 버전을 사용하신다면 아래의 ESLint 설정을 참고하시되, `extends` 필드에서 `prettier/@typescript-eslint`는 제거하셔야 합니다.

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "ignorePatterns": [
    "src/types/v3",
    "src/abis/types",
    "src/locales/**/*.js",
    "src/locales/**/en-US.po",
    "src/state/data/generated.ts",
    "node_modules",
    "coverage",
    "build",
    "dist",
    ".DS_Store",
    ".env.local",
    ".env.development.local",
    ".env.test.local",
    ".env.production.local",
    ".idea/",
    ".vscode/",
    "package-lock.json",
    "yarn.lock"
  ],
  "extends": [
    "react-app",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended"
  ],
  "plugins": ["simple-import-sort", "unused-imports"],
  "rules": {
    "unused-imports/no-unused-imports": "error",
    "simple-import-sort/imports": "error",
    "simple-import-sort/exports": "error",
    "@typescript-eslint/explicit-function-return-type": "off",
    "prettier/prettier": "error",
    "@typescript-eslint/no-explicit-any": "off",
    "@typescript-eslint/ban-ts-comment": "off",
    "@typescript-eslint/ban-ts-ignore": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "react/react-in-jsx-scope": "off",
    "object-shorthand": ["error", "always"],
    "no-restricted-imports": [
      "error",
      {
        "paths": [
          {
            "name": "ethers",
            "message": "Please import from '@ethersproject/module' directly to support tree-shaking."
          },
          {
            "name": "styled-components",
            "message": "Please import from styled-components/macro."
          },
          {
            "name": "@lingui/macro",
            "importNames": ["t"],
            "message": "Please use <Trans> instead of t."
          }
        ],
        "patterns": [
          {
            "group": ["**/dist"],
            "message": "Do not import from dist/ - this is an implementation detail, and breaks tree-shaking."
          },
          {
            "group": ["!styled-components/macro"]
          }
        ]
      }
    ]
  }
}
```

<br>

## 4. Tailwind CSS 셋업

Tailwind CSS 공식문서 [Install Tailwind CSS with Create React App](https://tailwindcss.com/docs/guides/create-react-app)을 참고했습니다. 필요한 디펜던시는 다음과 같습니다.

- `tailwindcss`: Tailwind CSS 코어 패키지
- `postcss`: Tailwind CSS를 Pure CSS로 변환해주는 Preprocessor
- `autoprefixer`: Post CSS 플러그인으로 사용될 Vendor Prefixer

```zsh
yarn add -D tailwindcss postcss autoprefixer
```

<br>

`tailwindcss` 초기화 커맨드를 실행하면 Tailwind CSS 설정파일 `tailwind.config.js`와 Post CSS 설정파일 `postcss.config.js`가 자동으로 생성됩니다.

```zsh
npx tailwindcss init -p
```

<br>

그 다음, `tailwind.config.js` 파일에서 Tailwind를 사용할 파일들의 경로를 지정해주면 됩니다.

```javascript
// tailwind.config.js

module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

<br>

마지막으로 `index.css` 파일 상단에 아래와 같이 `@tailwind` 디렉티브를 사용해서 Tailwind CSS를 포함시키면 됩니다.

```css
/* index.css*/

@tailwind base;
@tailwind components;
@tailwind utilities;
```

<br>

---

### References

- [React v17.0 | React](https://reactjs.org/blog/2020/10/20/react-v17.html)
- [Create a New React App | React](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)
- [Adding TypeScript | Create React App](https://create-react-app.dev/docs/adding-typescript)
- [How to use create-react-app with an older React version? | StackOverflow](https://stackoverflow.com/questions/46566830/how-to-use-create-react-app-with-an-older-react-version)
- [Install Tailwind CSS with Create React App](https://tailwindcss.com/docs/guides/create-react-app)
- [Getting Started | Redux Toolkit](https://redux-toolkit.js.org/introduction/getting-started#using-create-react-app)
