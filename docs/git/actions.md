# GitHub Actions로 프론트엔드 CI/CD 구축하기

<br>

1. GitHub Actions
2. Workflow 등록하기: `yml`
3. Runner
4. Workflow 구성요소: Events, Jobs, Steps, Actions
5. Workflow 파일 작성하기

<br>

## 1. GitHub Actions

GitHub Actions는 Workflow 자동화 도구입니다. 테스트, 빌드, 배포뿐만 아니라 원하는 어떤 작업이던 Workflow에 포함시켜서 자동화할 수 있습니다. 한 번에 같이 실행시킬 일들을 모아 하나의 Job으로 구성해놓고요, `push`, `pull` 등 특정 이벤트가 발생했을 때 Job을 실행합니다. 예를 들어, 누군가 특정 브랜치에 PR(Pull request)을 생성하는 이벤트가 발생하면, 테스트 스크립트가 자동으로 실행되게 할 수 있습니다.

<br>

<img src="./../img/actions.png" width="212" />

<br>

## 2. Workflow 등록하기: `yml`

실행할 Workflow는 `yml`(Yaml) 포맷 파일로 작성하여 레포지토리의 `/.github/workflows/` 경로에 두면 됩니다. 이 파일 작성을 완료하고 GitHub 레포지토리에 `push`하면 완료입니다. 이후부터 파일에 설정한대로 Workflow가 작동합니다. 또는 GitHub 레포지토리의 ~~Actions~~ 탭으로 이동, Workflow 셋업 버튼을 클릭하여 해당 파일을 쉽게 생성할 수 있습니다. 다음은 Workflow 셋업 버튼을 클릭했을 때 제공되는 샘플 파일입니다.

```yml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.

```

<br>

## 3. Runner

Runner는 [Job의 실행 환경](https://github.com/actions/runner)이 설치된 서버입니다. GitHub에서 호스팅하는 Runner를 사용할 수 있고요, 직접 Runner를 호스팅해도 됩니다. GitHub에서 호스팅하는 Runner는 가상머신의 형태로 제공하고요, Ubuntu Linux, Microsoft Windows, macOS 환경을 지원합니다. [About GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners) 문서에서 더 자세한 설명과 OS별 하드웨어 사양, 각 환경을 사용하기 위한 `yml` 설정값 등을 확인할 수 있습니다. 예를 들어, macOS Big Sur 11 환경을 사용하려면 문서에 따라 `yml` 파일의 해당 항목에 `macos-11`이라고 지정하면 됩니다.

```yml
# main.yml
jobs:
  build:
    runs-on: macos-11
```

<br>

## 4. Workflow 구성요소: Jobs, Steps, Actions

### 4-1. Jobs

이벤트가 발생했을 때 여러 개의 Job들이 실행되도록 Workflow를 구성할 수 있습니다. 각 Job은 지정한 Runner 위에서 실행됩니다. 기본적으로 Job들은 순차가 아닌 동시에 실행됩니다. 하지만 특정 Job이 성공했을 때만 다른 Job이 실행되도록 순차 지정할 수도 있습니다.

<br>

<img src="./../img/actions2.png" width="352" />

<br>

[Reusing workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)에 따르면, Workflow에서 다른 Workflow를 참조하도록 해서 마치 [SPA](https://en.wikipedia.org/wiki/Single-page_application)의 컴포넌트처럼 Workflow를 재사용할 수 있습니다.

<br>

### 4-2. Steps

Step은 Job 내에서 개별 업무들을 말합니다. Step이라는 이름처럼 지정한 순서대로 단계적으로 실행됩니다. Step은 하나의 [Action](./#actions)이 될 수도 있고, Shell 커맨드가 될 수도 있습니다.

<br>

### 4-3. Actions

Action은 Workflow를 이루는 가장 작은 Work 단위입니다. Action을 직접 만들거나, [GitHub 커뮤니티에서 제공하는 Action](https://github.com/marketplace?type=actions)들을 사용할 수 있습니다.

<br>

## 5. Workflow 파일 작성하기

### 5-1. 최상위 레벨 항목: `name`, `on`, `jobs`

Workflow 파일의 가장 상위 레벨 항목들은 다음과 같습니다. 모든 항목과 하위 항목에 대한 파일 작성 문법은 [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#onpushpull_requestpaths) 문서에서 확인할 수 있습니다.

- `name`: GitHub Actions 탭에 표시되는 Workflow의 이름입니다. Optional 항목입니다.
- `on`: Workflow를 자동으로 실행할 이벤트를 지정합니다. 이벤트 종류는 [Events that trigger workflows](https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows)
- `jobs`: 이 항목에 실행할 모든 Job들을 지정하면 됩니다.

<br>

```yml
name: CI
on: [push]
jobs:
  # job들을 여기에 작성합니다
```

<br>

### 5-2. Job 구성 항목: `runs-on`, `steps`

- `runs-on`: Job을 실행할 Runner를 지정합니다.
- `steps`: Job 내에서 실행될 Step들을 순서대로 지정합니다.

```yml
jobs:
  build: # job 이름
    runs-on: macos-11 # runner
    steps:
      # step들을 여기에 작성합니다
```

<br>

### 5-3. Step 구성 항목: `uses`, `run`

- `- uses`: 사용할 Action을 지정합니다. 커뮤니티 Action들은 이름에 `actions/` Prefix를 사용합니다.
- `- run`: Runner에서 실행할 커맨드를 지정합니다.

<br>

가령 아래와 같이 작성하면, 총 3 단계의 `steps`가 구성되는 겁니다.

```yml
steps:
  - uses: actions/checkout@v2 # step 1
  - uses: actions/setup-node@v2 # step 2
    with:
      node-version: '14'
  - run: npm install -g yarn # step 3
```

1. `actions/checkout@v2`를 사용해서 이 레파지토리에 체크아웃, Runner에 다운로드
2. `actions/setup-node@v2`를 사용해서 Runner에 `14` 버전의 `node` 설치 
3. `node`와 함께 설치될 `npm` 커맨드를 실행한다는 뜻입니다.

<br>

<br>

---

### References

- [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
- [About GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
- [[Github Action] Github Action 맛보고, AWS S3에 Vue 자동으로 배포하기 | 빈이의 개발 블로그](https://bin-e.tistory.com/44)
