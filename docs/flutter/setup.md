# Flutter setup for Mac OS

<br>

## Flutter SDK Installation

Install Flutter SDK from [Flutter official site](https://flutter.dev/docs/get-started/install).

<br>

1. Click downlaod button that you get `flutter_macos_1.22.5-stable.zip` file.

2. Unzip the file and extract it in the desired directory.

3. Add the downloaded Flutter tool to your path. Just copy and paste the below command.

```zsh
export PATH="$PATH:`pwd`/flutter/bin"
```

<br>

### Add Flutter to a certain desired path permanently (Optional)

The above command works only for the current terminal. To add Flutter to your path permanetly, please modify `.bash_profile` file. Like the following:

```zsh
# Open the file with vim editor.
vim $HOME/.bash_profile
```

<br>

If you got Flutter SDK installed in the file named `temp`, add the below command to `.bash_profile`.

```zsh
# Add the below command as the last line of the file.
export PATH="$HOME/temp/flutter/bin:$PATH"
```

<br>

## Flutter Doctor for additional installation

The following command gets you to see if there are any dependencies you need to install to complete the setup. This command checks your environment and displays a report to the terminal window.

```zsh
flutter doctor
```

<br>

The printed result might be like this, for example:

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

It's saying that you need to install Xcode, Flutter and Dart plugin for Android Studio and some device connection.

<br>

## iOS Setup

### Xcode Installation

To develop iOS app using Flutter, Xcode 9.0^ is necessary. Just follow the above printed guide. Install Xcode via the App Store, and run the below command.

```zsh
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

<br>

And then, agree with the license.

```zsh
sudo xcodebuild -license
```

<br>

To see if Xcode got installed, run `flutter doctor` again.

<br>

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

Yay, it's got installed.

<br>

### iOS simulator

To prepare to run and test your Flutter app on the iOS simulator, check if the Simulator works. You can open the Simulator with the following command.

```zsh
open -a Simulator
```

<br>

### CocoaPods installation (Optional)

To deploy your Flutter app to a physical iOS device, is is necessary to set up physical device deployment in Xcode and an Apple Developer account. If using Flutter plugins, you will also need the third-party CocoaPods dependency manager. To install, follow the guide given by `flutter doctor`.

```zsh
sudo gem install cocoapods
```

> Just skip this step if not depending on Flutter plugins with native iOS code.

<br>

## Android Setup

Install Android Studio via its offical site or homebrew. Run the Android Studio, and go to `Configure > SDK Manager`. Now, install Android SDK, Android SDK Command-line Tools, and Android SDK Build-Tools by checking them all if not checked.

<br>

---

### References

- [macOS install | Flutter](https://flutter.dev/docs/get-started/install/macos)
