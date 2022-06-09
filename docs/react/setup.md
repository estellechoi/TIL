# Create React App으로 React 16.x + TypeScript 프로젝트 셋업하기

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

## 2. React 16.xx + TypeScript 앱 생성하기

### 2-1. 앱 생성시 `--template` 옵션 사용하기

저는 `yarn`을 사용했는데, [공식문서](https://create-react-app.dev/docs/adding-typescript)에 다른 패키지매니저를 사용하는 방법이 자세히 나와있기 때문에 따로 정리하지는 않겠습니다. 아래처럼 `--template` 옵션을 사용해서 TypeScript 셋업이 되어있는 앱을 생성합니다. 이렇게 하면 `yarn add typescript @types/node @types/react @types/react-dom @types/jest` 커맨드를 사용하여 TypeScript와 패키지들의 Type Declaration를 별도로 설치하는 것과 같습니다.

```zsh
yarn create react-app project-name --template typescript
```

<br>

### 2-2. React 버전 낮추기

최신 버전의 `create-react-app`을 사용해서 앱을 생성하면 최신 버전의 `react` 패키지로 React 앱이 셋업됩니다. 만약 이전 버전의 React를 사용하려면 다음과 같이 몇 개 파일을 수정해주어야 하는데요, 저의 경우 `18.x` 버전의 React 앱 셋업을 `16.x` 버전으로 Downgrade 했기 때문에 제 상황을 기준으로 정리해보았습니다.

<br>

`package.json` 파일에서 다음 두 패키지의 버전을 변경합니다.

변경 전

```json
"dependencies": {
    ...
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "react": "^18.1.0",
    "react-dom": "^18.1.0",
    ...
  },
```

변경 후

```json
"dependencies": {
    ...
    "@types/react": "^16.0.0",
    "@types/react-dom": "^16.0.0",
    "react": "^16.13.0",
    "react-dom": "^16.13.0",
    ...
  },
```

<br>

그리고 아직 React 16.x 버전에는 존재하지 않는 메소드들을 수정해줍니다. `index.tsx` 파일에서 다음 부분을 수정하면 될겁니다.

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

---

### References

- [React v16.0 | React](https://ko.reactjs.org/blog/2017/09/26/react-v16.0.html)
- [Create a New React App | React](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)
- [Adding TypeScript | Create React App](https://create-react-app.dev/docs/adding-typescript)
- [How to use create-react-app with an older React version? | StackOverflow](https://stackoverflow.com/questions/46566830/how-to-use-create-react-app-with-an-older-react-version)
