# 깃 플로우(Git Flow)와 브랜치 네이밍 컨벤션 도입하기

<br>

## 1. 깃 플로우(Git Flow): 5개 브랜치, 기본 사이클

### 1-1. 5개 브랜치: `master`, `develop`, `feature`, `release`, `hotfix`

깃 플로우(Git Flow)는 여러명의 개발자들이 협업하는 환경에서 생겨나는 깃(Git)의 브랜치(Branch)들을 효율적으로 관리하고 통합하기 위한 전략 중 하나입니다. 2010년 Vincent Driessen이 블로그를 통해 공개한 [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) 글에서 시작되었습니다.

<br>

<img src="./../img/gitflow.png" alt="" />

<br>

깃 플로우에서는 총 5개 브랜치를 사용하고요, 그 중 `master`, `develop` 브랜치는 정규 브랜치이므로 새로 생성하거나 없애지 않고 하나의 브랜치를 계속 사용합니다. 반면, `feature`, `release`, `hotfix` 브랜치는 정규 브랜치에서 분기하는 브랜치들입니다. 각 브랜치의 작업이 끝나고 정규 브랜치로 머지(`merge`)가 완료되면 제거해도 되는 브랜치들이죠. `feature`, `release` 브랜치는 `develop` 브랜치에서 분기되고요, `hotfix` 브랜치는 `master` 브랜치에서 직접 분기하는 임시 브랜치로, 이미 배포된 버전의 버그를 빠르게 수정하기 위한 브랜치입니다. 

<br>

정규 브랜치

- `master`: 최종 배포되는 브랜치, `release` 브랜치가 머지되면 배포 버전 태그를 붙인 후 배포
- `develop`: `hotfix`를 제외한 모든 변경이 시작되는 브랜치 

<br>

임시 브랜치

- `feature`: 기능을 개발하는 브랜치, `develop` 브랜치에서 기능별로 분기
- `release`: `feature` 브랜치들이 모두 머지된 후 배포 관련 문서작업, 버그 수정 등이 이루어지는 브랜치
- `hotfix`: 배포된 버전의 버그 수정을 위해 `master`에서 직접 분기하고 머지되는 브랜치

<br>

### 1-2. 기본 사이클

깃 플로우의 사이클을 대략적으로 정리하면 다음과 같습니다.

1. `develop` 브랜치에서 기능 개발을 위한 `feature` 브랜치를 생성하고 작업을 진행합니다.
2. 작업이 완료되면 다시 `develop` 브랜치에 머지(`merge`)시킵니다.
3. 기능 개발을 담당하는 `feature` 브랜치들이 모두 머지되었으면, 배포를 위한 `release` 브랜치를 생성합니다.
4. `release` 브랜치에서 배포와 관련된 문서작업, 버그 수정 등을 진행합니다.
5. `release` 브랜치 작업이 완료되면 `master`, `develop` 브랜치에 각각 머지합니다.
6. `master` 브랜치에 마지막으로 머지된 커밋(`commit`)에 태그(`git tag`)를 생성하여 원격 저장소로 푸시(`push`)합니다.

<br>

브랜치들을 머지할 때는 `--no-ff` 옵션을 사용하여 브랜치에 대한 기록이 사라지는 것을 방지합니다. 다음은 [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)에서 발췌한 내용입니다.

> The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

<img src="./../img/gitflow2.png" alt="" />

<br>


깃 태그 생성시에는 [Semantic Versioning](https://semver.org/)을 사용합니다.

<br>

## 2. 규칙


<br>

---

### References
