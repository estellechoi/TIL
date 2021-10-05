# 깃 플로우(Git Flow)와 브랜치 네이밍 컨벤션 도입하기

1. 깃 플로우(Git Flow)
2. 5개 브랜치: `master`, `develop`, `feature`, `release`, `hotfix`
3. 브랜치 기록 남기기: `merge --no-ff`
4. 기본 사이클
5. `hotfix` 플로우
6. 브랜치 네이밍 컨벤션

<br>

## 1. 깃 플로우(Git Flow)

깃 플로우(Git Flow)는 여러명의 개발자들이 협업하는 환경에서 생겨나는 깃(Git)의 브랜치(Branch)간 충돌을 최대한 막고 효율적으로 통합하기 위한 전략 중 하나입니다. 2010년 Vincent Driessen이 블로그를 통해 공개한 [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) 글에서 시작되었습니다.

<br>

<img src="./../img/gitflow.png" alt="" />

<br>

## 2. 5개 브랜치: `master`, `develop`, `feature`, `release`, `hotfix`

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

## 3. 브랜치 기록 남기기: `merge --no-ff`

브랜치들을 머지할 때는 `--no-ff` 옵션을 사용하여 브랜치에 대한 기록이 사라지는 것을 방지합니다. 다음은 [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)에서 발췌한 내용입니다.

> The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

<br>

<img src="./../img/gitflow2.png" alt="" width="840" />

<br>

## 4. 기본 사이클

1. `develop` 브랜치에서 기능 개발을 위한 `feature` 브랜치를 생성하고 작업을 진행합니다.

```
git checkout -b feature develop
```

<br>

2. 작업이 완료되면 다시 `develop` 브랜치에 머지시키고, `feature` 브랜치는 제거합니다.

```
git checkout develop
git merge --no-ff feature
git branch -d myfeature
```

<br>

3. 원격 저장소로 `develop` 브랜치를 푸시합니다.

```
git push origin develop
```

<br>

4. 기능 개발과 각 브랜치 머지가 완료되었으면, 이제 배포를 위한 `release` 브랜치를 생성합니다.

```
git checkout -b release-1.0 develop
```

<br>

5. 버전 번호를 일괄 업데이트하기 위한 `bump-version.sh` 파일이 있다면 해당 파일을 수정, 커밋합니다.

```
./bump-version.sh 1.0
git commit -a -m "Bumped version number to 1.0"
```

<br>


6. 배포와 관련된 문서작업, 버그 수정 등을 추가로 진행합니다.

<br>

7. `release` 브랜치 작업이 완료되면 `master`로 머지하고, 버전 태그(`git tag`)를 생성합니다. 태그명은 [Semantic Versioning](https://semver.org/)을 사용합니다.

```
git checkout master
git merge --no-ff release-1.0
git tag -a 1.0
```

<br>

8. `release` 브랜치를 `develop` 브랜치에 똑같이 머지합니다.

```
git checkout develop
git merge --no-ff release-1.0
```

<br>

9. `release` 브랜치를 삭제합니다.

```
git branch -d release-1.0햣
```

<br>

10. `develop` 브랜치와 `master` 브랜치를 푸시(`push`)합니다.

```
git push origin develop
git checkout master
git push
```

<br>

## 5. `hotfix` 플로우

### 5-1. 브랜치 핸들링 규칙

`hotfix` 브랜치는 `master` 브랜치에서 바로 분기하고 다시 머지시킵니다. 이미 배포된 버전에 문제가 생겼기 때문에 빠르게 대응하고, 다음 버전을 준비하고있는 `develop` 브랜치와 독립적으로 작업하기 위함이죠. 다음은 `hotfix` 플로우의 규칙들입니다.

- `master` 브랜치에서 분기
- `master`, `develop` 브랜치로 머지
- 네이밍 컨벤션 : `hotfix-*`

<br>

<img src="./../img/gitflow3.png" alt="" width="600" />

<br>

### 5-2. 사이클

1. 현재 출시된 버전이 `1.0` 버전이라고 가정하면, 버그를 수정하고 배포하는 버전은 [Semantic Versioning](https://semver.org/)에 따라 `1.0.1`이 되므로, `hotfix-1.0.1` 브랜치를 생성합니다.

```
git checkout -b hotfix-1.0.1 master
```

<br>

2. `hofix` 브랜치에서는 `release` 브랜치를 통하지 않기 때문에 `hotfix` 브랜치 내에서 배포 관련 작업을 해줍니다.

```
./bump-version.sh 1.0.1
git commit -a -m "Bumped version number to 1.0.1"
```

<br>

3. 버그 수정 내용들을 커밋합니다. 하나 또는 여러 개의 커밋으로 나누어 진행할 수 있습니다.

```
git commit -m "Bug fixed in landing page"
```

<br>

4. 수정된 버전을 배포하기 위해 `master` 브랜치로 머지시킵니다.

```
git checkout master
git merge --no-ff hotfix-1.0.1
```

<br>

5. 새 버전명으로 태그를 생성합니다.

```
git tag -a 1.0.1
```

<br>

6. `develop` 브랜치로 똑같이 머지시킵니다.

```
git checkout develop
git merge --no-ff hotfix-1.0.1
```

<br>

7. `hotfix` 브랜치를 삭제합니다.

```
git branch -d hotfix-1.0.1
```

<br>

8. `develop` 브랜치, `master` 브랜치를 푸시합니다.

```
git push origin develop
git checkout master
git push
```

<br>

## 6. 브랜치 네이밍 컨벤션

### 6-1. `master`, `develop` 브랜치

`master`, `develop` 브랜치는 해당 이름을 그대로 사용하는 것이 일반적입니다.

<br>

### 6-2. `feature` 브랜치

`master`, `develop`, `release-*`, `hotfix-*`는 사용할 수 없습니다. 팀과 함께 컨벤션을 만들어 사용하면 되고요, 아래의 추천 목록에서 선택하거나 변형해서 네이밍합니다.

- `feature/{feature-description}`
- `feature/{issue-number}-{feature-description}`
- `feature/{author}{issue-number}-{feature-description}`

<br>

예를 들어 아래와 같이 사용할 수 있겠습니다.

```
git checkout -b feature/yujin-1001-app-tutorial-update
```

<br>

### 6-3. `release`, 

배포 관련 브랜치인 `release` 브랜치는 `release-{next-version}` 형식을 추천합니다. `{next-version}`은 이번에 출시할 버전명입니다. 예를 들어 아래와 같이 사용할 수 있겠습니다.

```
git checkout -b release-1.0
```

<br>

### 6-4. `hotfix` 브랜치

`hotfix` 브랜치는 아래의 추천 목록에서 선택하거나 변형해서 네이밍합니다.

- `hotfix-{next-version}`
- `hotfix-{next-version}/{hotfix-description}`
- `hotfix-{next-version}/{issue-number}-{hotfix-description}`
- `hotfix-{next-version}/{author}-{issue-number}-{hotfix-description}`

<br>

예를 들어 아래와 같이 사용할 수 있겠습니다.

```
git checkout -b hotfix-1.0.1/yujin-1001-login-btn-typo
```

<br>

---

### References

- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
- [Git Branching Naming Convention: Best Practices](https://codingsight.com/git-branching-naming-convention-best-practices/)
- [GitFlow? 들어도 봤고, 쓰고도 있는데... | 강남언니 공식 블로그](https://blog.gangnamunni.com/post/understanding_git_flow/)
- [하루에 1000번 배포하는 조직 되기 | 뱅크샐러드 블로그](https://blog.banksalad.com/tech/become-an-organization-that-deploys-1000-times-a-day/?gclid=CjwKCAjwzOqKBhAWEiwArQGwaLas_It3JTTOTPuC9pp3gVTZqV_efdm4a0QbeGYnVDhvphXXQCW0RBoC5BIQAvD_BwE)
