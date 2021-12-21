# macOS에서 Flutter 개발환경 셋업하기

<br>

1. Flutter 준비: Flutter SDK 설치하기, 환경변수에 Flutter 경로 추가하기
2. Flutter Doctor로 개발환경 진단하기
3. iOS 앱 개발을 위한 셋업: Xcode 설치하기, iOS 시뮬레이터 테스트, CocoaPods 설치하기
4. Android 앱 개발을 위한 셋업: Android SDK 설치하기
5. 에디터(Android Studio/Visual Studio Code) 플러그인 설치

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

다음 명령어는 Flutter 개발환경 셋업을 완료하기 위해 추가로 설치할 것들은 없는지 진단 결과를 출력합니다.

```zsh
flutter doctor
```

<br>

명령어를 실행하면 아래와 같은 진단 결과가 출력되는데요, 저의 경우 Xcode 설치가 필요하고, Android Studio용 Flutter/Dart 플러그인 설치, 테스트용 기기 연결이 필요하다고 하네요.

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.7 19H2 darwin-x64, locale
    ko-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[✗] Xcode - develop for iOS and macOS
    ✗ Xcode installation is incomplete; a full installation is necessary for iOS
      development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
        sudo xcodebuild -runFirstLaunch
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS and macOS platform side's plugin
        code that responds to your plugin usage on the Dart side.
        Without CocoaPods, plugins will not work on iOS or macOS.
        For more info, see https://flutter.dev/platform-plugins
      To install:
        sudo gem install cocoapods
[!] Android Studio (version 4.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] Connected device
    ! No devices available

! Doctor found issues in 3 categories.
```

<br>

## 3. iOS 앱 개발을 위한 셋업: Xcode 설치하기, iOS 시뮬레이터 테스트, CocoaPods 설치하기

### 3-1. Xcode 설치하기

Flutter를 사용해서 iOS 앱을 개발하고 빌드하려면 `Xcode 9.0^`가 설치된 macOS가 필요합니다. Flutter Doctor의 설명에 따라 App Store에서 Xcode를 설치한 후 다음 명령어를 실행합니다.

```zsh
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

<br>

그 다음, 아래 명령어를 사용하여 Xcode 라이센스에 동의합니다.

```zsh
sudo xcodebuild -license
```

<br>

Xcode 설치가 잘 되었는지 확인하기 위해 다시 `flutter doctor` 명령어를 실행해봅니다. 저의 경우 `Xcode 12.3`이 설치된 것을 확인할 수 있네요.

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.7 19H2 darwin-x64, locale
    ko-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[!] Xcode - develop for iOS and macOS (Xcode 12.3)
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS and macOS platform side's plugin
        code that responds to your plugin usage on the Dart side.
        Without CocoaPods, plugins will not work on iOS or macOS.
        For more info, see https://flutter.dev/platform-plugins
      To install:
        sudo gem install cocoapods
[!] Android Studio (version 4.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.

[!] Connected device
    ! No devices available

! Doctor found issues in 3 categories.
```

<br>

### 3-2. iOS 시뮬레이터 테스트

iOS 앱을 시뮬레이팅하는데 필요한 iOS 시뮬레이터를 테스트합니다. 다음 명령어를 사용하면 됩니다.

```zsh
open -a Simulator
```

<br>

### 3-3. CocoaPods 설치하기 (선택)

Flutter로 개발한 앱을 배포하고 실제 iOS 디바이스에서 테스트하려면, Xcode에 디바이스를 등록해야하고 Apple Developer 계정도 필요합니다. 이때 Flutter 플러그인을 사용한다면 Xcode용 써드파티 디펜던시 매니저인 [CocoaPods](https://cocoapods.org/)가 필요한데요, Flutter Doctor의 가이드에 따라 미리 설치해줍니다.

```zsh
sudo gem install cocoapods
```

<br>

## 4. Android 앱 개발을 위한 셋업: Android SDK 설치하기

[Android Studio](https://developer.android.com/studio)를 공식 홈페이지나 [Homebrew](https://brew.sh/index_ko)를 사용해서 설치합니다. Android Studio를 열고 `Configure` → `SDK Manager`로 이동하면 안드로이드 앱 개발과 빌드에 필요한 Android SDK와 CLI, 빌드 툴을 설치할 수 있습니다. 이제 다음 명령어를 사용하여 Android SDK 라이센스에 동의합니다.

```zsh
flutter doctor --android-licenses
```

<br>

## 5. 에디터(Android Studio/Visual Studio Code) 플러그인 설치

### 5-1. Visual Studio Code

<br>

### 5-2. Android Studio용 Flutter & Dart 플러그인 설치하기

만약 Android Studio에서 Flutter를 사용하여 개발하려면 별도로 플러그인을 설치해야 하는데요, `Configure` → `Plugins`로 이동한 후 `Flutter`를 검색하여 설치합니다. `Dart` 플러그인은 `Flutter` 플러그인에 내장되어 있으므로 별도로 설치하지 않아도 됩니다.

<br>

---

### References

- [macOS install | Flutter](https://flutter.dev/docs/get-started/install/macos)
- [Building a web application with Flutter | Flutter](https://flutter.dev/docs/get-started/web)
- [가상 기기 만들기 및 관리하기 | Android Studio](https://developer.android.com/studio/run/managing-avds)
- [앱 버전 지정 | Android Studio](https://developer.android.com/studio/publish/versioning)
- [Flutter's build modes | Flutter](https://flutter.dev/docs/testing/build-modes)
