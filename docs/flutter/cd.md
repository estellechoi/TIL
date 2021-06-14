# Fastlane을 사용하여 Flutter iOS 앱 배포 자동화하기

<br>

## 1. 셋업

### 1) Xcode CLT 설치하기

아래 명령어를 사용하여 Xcode CLT(Command Line Tool)를 설치합니다. 이미 설치되어있다면 오류 메시지가 나타납니다.

```
xcode-select --install
```

<br>

### 2) Homebrew를 사용하여 Fastlane 설치하기

아래와 같이 `brew` 명령어를 사용하여 `fastlane`을 설치합니다. 자세한 내용은 [Homebrew Formulae](https://formulae.brew.sh/formula/fastlane)를 참고하세요.

```
brew install fastlane
```

<br>

## 2. Fastlane 셋업

이제 프로젝트의 루트 경로에서 다음 명령어를 사용하여 Fastlane 셋업을 진행합니다. Flutter 프로젝트는 `ios/` 경로에서 실행합니다.

```
fastlane init
```

<br>

<img src="./../img/fastlane.png" alt="fastlane" width="600" />

<br>
<br>

그 다음, [App Store Connect](https://appstoreconnect.apple.com/) 로그인시 사용하는 Apple 계정 이메일과 비밀번호를 입력하고 계속합니다.

<br>

<img src="./../img/fastlane.png" alt="fastlane" width="600" />

<br>

---

### References

- [Getting started with fastlane for iOS](https://docs.fastlane.tools/getting-started/ios/setup/)
