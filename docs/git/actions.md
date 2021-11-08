# GitHub Actions로 프론트엔드 CI/CD 구축하기

<br>

1. GitHub Actions
2. 설정하기: `/.github/workflows/main.yml`
3. Runner

<br>

## 1. GitHub Actions

### 1-1. GitHub Actions란

GitHub Actions는 Workflow 자동화 도구입니다. 테스트, 빌드, 배포뿐만 아니라 원하는 어떤 작업이던 Workflow에 포함시켜서 자동화할 수 있습니다. 한 번에 같이 실행시킬 일들을 모아 하나의 Job으로 구성하고, Job의 트리거 시점은 `push`, `pull` 등 특정 이벤트를 사용합니다. 예를 들어, 누군가 특정 브랜치에 PR(Pull request)를 생성하면, 테스트 스크립트가 자동으로 실행되게 할 수 있습니다.

<br>

<img src="./../img/actions.png" width="212" />

<br>


<br>

이벤트가 발생했을 때 여러 개의 Job들이 실행되도록 Workflow를 구성할 수 있고요, 각 Job은 지정한 Runner 위에서 실행됩니다. [Reusing workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)에 따르면, Workflow에서 다른 Workflow를 참조하도록 해서 마치 [SPA](https://en.wikipedia.org/wiki/Single-page_application)의 컴포넌트처럼 Workflow를 재사용할 수 있습니다.

<br>

<img src="./../img/actions2.png" width="352" />

<br>

## 2. 설정하기: `/.github/workflows/main.yml`

실행할 Workflow는 `yml` 파일로 작성하여 레포지토리의 `/.github/workflows/` 경로에 두면 됩니다. GitHub 레포지토리에서 Actions 탭으로 이동하고, Workflow 셋업 버튼을 클릭하여 해당 파일을 생성할 수 있습니다. 다음은 Workflow 셋업 버튼을 클릭했을 때 제공되는 샘플 파일입니다.

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

Runner는 [Job의 실행 환경을 제공하는 GitHub Actions Runner](https://github.com/actions/runner)가 설치된 서버입니다. GitHub에서 호스팅하는 Runner를 사용할 수 있고요, 직접 Runner를 호스팅해도 됩니다. GitHub에서 호스팅하는 Runner는 가상머신의 형태로 제공하고요, Ubuntu Linux, Microsoft Windows, macOS 환경을 지원합니다. [About GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners) 문서에서 더 자세한 설명과 OS별 하드웨어 사양, 각 사양을 사용 설정하기 위한 설정값을 확인할 수 있습니다. 예를 들어, macOS Big Sur 11 환경을 사용하려면 문서에 따라 `macos-11`이라고 지정하면 됩니다.

```yml
jobs:
  build:
    runs-on: macos-11
```

<br>

## 4. Events, Jobs, Steps, Actions

<br>

---

### References

- [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
- [About GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
- [[Github Action] Github Action 맛보고, AWS S3에 Vue 자동으로 배포하기 | 빈이의 개발 블로그](https://bin-e.tistory.com/44)
