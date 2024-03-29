# macOS에서 Flutter 개발환경 셋업하기

<br>

1. Flutter 준비: Flutter SDK 설치하기, 환경변수에 Flutter 경로 추가하기
2. Flutter Doctor로 개발환경 진단하기
3. iOS 앱 개발을 위한 셋업: Xcode 설치하기, iOS 시뮬레이터 테스트, CocoaPods 설치하기
4. Android 앱 개발을 위한 셋업: Android SDK 설치하기
5. Android Studio/VS Code용 Flutter 플러그인 설치

<br>

## 1. Flutter 준비: Flutter SDK 설치하기, 환경변수에 Flutter 경로 추가하기

### 1-1. Flutter SDK 설치하기

[Flutter 공식 홈페이지](https://flutter.dev/docs/get-started/install)에서 Flutter SDK를 다운로드합니다.

1. `flutter_macos_1.22.5-stable.zip`을 클릭하여 파일을 다운로드

2. 파일 압축을 해제하고, 생성되는 Flutter(`flutter` 디렉토리)를 원하는 경로로 이동

<br>

### 1-2. 환경변수에 Flutter 경로 추가하기

Flutter를 사용하려면 Flutter SDK가 있는 경로를 참조할 수 있도록 쉘에 `PATH` 변수가 필요합니다. `flutter` 디렉토리를 옮긴 경로로 이동한 후, 다음 쉘스크립트를 사용하여 `PATH` 변수를 셋업합니다. 이제 `flutter` 명령어를 사용할 수 있고요, [Update your path | Flutter](https://docs.flutter.dev/get-started/install/macos#update-your-path) 문서에 `PATH` 변수를 추가하는 것에 대한 자세한 설명이 있습니다.

```zsh
export PATH="$PATH:`pwd`/flutter/bin"
```

<br>

다만, 터미널을 종료하면 위에서 셋업한 변수도 사라지므로, 쉘을 실행할 때마다 `PATH` 변수를 세팅해야합니다. 쉘마다 쉘이 시작될 때 실행되는 Initialization 스크립트 파일이 있는데요, 저는 [Z쉘](https://en.wikipedia.org/wiki/Z_shell)을 사용하기 때문에 `.zshrc` 파일에 스크립트를 추가했습니다. `.zshrc`는 Z쉘이 시작될 때마다 실행되는 스크립트 파일입니다.

<br>

`.zshrc` 파일을 열고요,

```zsh
vi ~/.zshrc
```

<br>

저는 Flutter SDK를 `~/dev/`로 옮겼기 때문에 다음과 같이 스크립트를 작성했습니다.

```zsh
# Flutter SDK path
export PATH="$PATH:$HOME/dev/flutter/bin"
```

<br>

이제 터미널을 재시작하고 `PATH` 변수가 잘 세팅되는지 확인합니다.

```zsh
echo $PATH
```

<br>

마지막으로 다음 명령어를 사용하여 `flutter` 명령어가 인식되는지 확인합니다.

```zsh
which flutter
```

<br>

## 2. Flutter Doctor로 개발환경 진단하기

다음 명령어는 Flutter 개발환경 셋업을 완료하기 위해 추가로 설치할 것들은 없는지 진단하고 결과를 출력합니다.

```zsh
flutter doctor
```

<br>

`flutter doctor` 명령어를 실행하면 아래와 같은 진단 결과가 출력됩니다.

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 2.8.1, on macOS 11.3.1 20E241 darwin-arm, locale ko-KR)
[✗] Android toolchain - develop for Android devices
    ✗ Unable to locate Android SDK.
      Install Android Studio from: https://developer.android.com/studio/index.html
      On first launch it will assist you in installing the Android SDK components.
      (or visit https://flutter.dev/docs/get-started/install/macos#android-setup for detailed instructions).
      If the Android SDK has been installed to a custom location, please use
      `flutter config --android-sdk` to update to that location.

[✗] Xcode - develop for iOS and macOS
    ✗ Xcode installation is incomplete; a full installation is necessary for iOS development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
        sudo xcodebuild -runFirstLaunch
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS and macOS platform side's plugin code that responds to your plugin usage on
        the Dart side.
        Without CocoaPods, plugins will not work on iOS or macOS.
        For more info, see https://flutter.dev/platform-plugins
      To install see https://guides.cocoapods.org/using/getting-started.html#installation for instructions.
[✓] Chrome - develop for the web
[!] Android Studio (not installed)
[✓] VS Code (version 1.63.2)
[!] Connected device
    ! No devices available

! Doctor found issues in 3 categories.
```

<br>

Flutter를 사용하여 앱 개발을 하려면 기본적으로 다음 항목들과 테스트용 실제 디바이스 1 개 이상이 필요합니다. 저의 경우 Android SDK와 Xcode 설치가 필요하다고 하네요. Flutter Doctor는 각 디펜던시를 어떻게 설치할 수 있는지에 대한 설명까지 가이드를 주기 때문에 이를 참고하여 설치하시면 됩니다.

#### 개발 디펜던시

- Flutter SDK
- Android SDK : Android 앱 개발에 필요
- Xcode : iOS 앱 개발에 필요

#### 에디터

- [Android Studio](https://developer.android.com/studio)
- [VS Code](https://code.visualstudio.com/)

<br>

## 3. iOS 앱 개발을 위한 셋업: Xcode 설치하기, iOS 시뮬레이터 테스트, CocoaPods 설치하기

### 3-1. Xcode 설치하기

Flutter를 사용해서 iOS 앱을 개발하고 빌드하려면 `Xcode 9.0^`가 설치된 macOS가 필요합니다. Flutter Doctor의 설명에 따라 App Store에서 Xcode를 설치한 후, 아래 명령어를 실행합니다.

```zsh
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

<br>

그 다음, 아래 명령어를 사용하여 Xcode 라이센스에 동의합니다.

```zsh
sudo xcodebuild -license
```

<br>

Xcode 설치가 잘 되었는지 확인하기 위해 다시 `flutter doctor` 명령어를 실행해봅니다. 저의 경우 `Xcode 13.2.1`이 설치된 것을 확인할 수 있네요.

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 2.8.1, on macOS 11.3.1 20E241 darwin-arm, locale ko-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 32.0.0)
[!] Xcode - develop for iOS and macOS (Xcode 13.2.1)
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS and macOS platform side's plugin code that responds to your plugin usage on the Dart
        side.
        Without CocoaPods, plugins will not work on iOS or macOS.
        For more info, see https://flutter.dev/platform-plugins
      To install see https://guides.cocoapods.org/using/getting-started.html#installation for instructions.
[✓] Chrome - develop for the web
[✓] Android Studio (version 2020.3)
[✓] VS Code (version 1.63.2)
[!] Connected device
    ! No devices available

! Doctor found issues in 1 category.
```

<br>

### 3-2. iOS 시뮬레이터 테스트

iOS 앱을 시뮬레이팅하는데 필요한 iOS 시뮬레이터를 테스트합니다. 다음 명령어를 사용하면 됩니다.

```zsh
open -a Simulator
```

<br>

### 3-3. CocoaPods 설치하기

Flutter로 개발한 앱을 iOS 디바이스에 실행하기 위해서는 iOS 앱으로 빌드해야 하는데요, 이때 iOS용 디펜던시 매니저인 [CocoaPods](https://cocoapods.org/)가 필요합니다. [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#installation) 공식문서의 가이드에 따라 설치하시거나, [Homebrew를 사용하여 설치](https://formulae.brew.sh/formula/cocoapods)하시면 됩니다. 저는 Homebrew를 사용했습니다.

```zsh
brew install cocoapods
```

<br>

그리고 Flutter Doctor로 잘 설치됐는지 확인해줍니다.

```zsh
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 2.8.1, on macOS 11.3.1 20E241 darwin-arm, locale ko-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 32.0.0)
[✓] Xcode - develop for iOS and macOS (Xcode 13.2.1)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2020.3)
[✓] VS Code (version 1.63.2)
[✓] Connected device (2 available)

• No issues found!
```

<br>

## 4. Android 앱 개발을 위한 셋업: Android SDK 설치하기

먼저 Android Studio를 [공식 홈페이지](https://developer.android.com/studio)나 [Homebrew](https://brew.sh/index_ko)를 사용해서 설치합니다. 저는 [Homebrew를 사용](https://formulae.brew.sh/cask/android-studio)했습니다.

```zsh
brew install --cask android-studio
```

<br>

이제 Android Studio를 열고 `Preferences...` → `Appearance & Behavior` → `System Settings` → `Android SDK`로 이동한 후, `SDK` 탭을 클릭하면 안드로이드 앱 개발과 빌드에 필요한 Android SDK와 CLI, 빌드 툴 등을 설치할 수 있습니다. 아래와 같이 설치할 항목들을 체크하신 후 `Apply` 버튼을 클릭하여 진행하면 됩니다.

<img src="./../img/android-sdk-install.png" width="700" />

<br>

그리고 다음 명령어를 사용하여 Android SDK 라이센스에 동의합니다.

```zsh
flutter doctor --android-licenses
```

<br>

## 5. Android Studio/VS Code용 Flutter 플러그인 설치

[Flutter 공식문서](https://docs.flutter.dev/get-started/editor?tab=vscode)의 가이드에 따라 선택한 에디터에서 Flutter를 사용하기 위해 `Flutter` 플러그인을 설치하고 개발을 시작하면 됩니다. `Dart` 플러그인은 `Flutter` 플러그인에 내장되어 있으므로 별도로 설치하지 않아도 됩니다.

<br>

만약 Android Studio에서 Flutter를 사용하여 개발하려면 별도로 플러그인을 설치해야 하는데요, `Preferences...` →→→ `Plugins`로 이동한 후 `Flutter`를 검색하여 설치합니다. VS Code용 플러그인과 마찬가지로 `Dart` 플러그인은 `Flutter` 플러그인에 포함되어있습니다.

<br>

---

### References

- [macOS install | Flutter](https://flutter.dev/docs/get-started/install/macos)
- [Building a web application with Flutter | Flutter](https://flutter.dev/docs/get-started/web)
- [가상 기기 만들기 및 관리하기 | Android Studio](https://developer.android.com/studio/run/managing-avds)
- [앱 버전 지정 | Android Studio](https://developer.android.com/studio/publish/versioning)
- [Flutter's build modes | Flutter](https://flutter.dev/docs/testing/build-modes)
