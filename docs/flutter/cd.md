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

`What would you like to use fastlane for?` 질문이 나타나면 해당하는 번호를 입력한 후 계속합니다. TestFlight 배포의 경우 2번, App Store 배포의 경우 3번을 선택하면 됩니다.

<img src="./../img/fastlane.png" alt="fastlane" width="600" />

<br>
<br>

그 다음, [App Store Connect](https://appstoreconnect.apple.com/) 로그인시 사용하는 Apple 계정 이메일과 비밀번호를 입력하고 계속합니다.

<br>

<img src="./../img/fastlane2.png" alt="fastlane" width="600" />

<br>
<br>

셋업이 완료되면 `ios/` 경로에 다음 4개 파일이 생성되고요, 4개 파일은 `.gitignore`에 추가하지 않고 원격 디렉토리를 통해 공유될 수 있도록 합니다.

- `fastlane/Appfile`
- `fastlane/Fastfile`
- `Gemfile`
- `Gemfile.lock`

<br>

## 3. Fastlane 설정하기

### 1) `Appfile`

`Appfile` 파일은 앱 배포시 App Store Connect에 자동으로 접속하여 배포를 진행하기 위한 Apple 계정 정보와 앱 ID를 포함해야합니다. 파일 내용은 아래와 같이 구성되어 있고요, 각 메소드의 인자 값이 자동으로 세팅되어있습니다.

```
app_identifier("your.app.identifier") # The bundle identifier of your app
apple_id("apple-id") # Your Apple email address

itc_team_id("team-id") # App Store Connect Team ID
team_id("portal-team-id") # Developer Portal Team ID
```

<br>

위의 인자 값들은 환경변수를 사용하여 관리할 수 있고요, `fastlane`에는 `.env` 파일을 통해 환경변수를 사용할 수 있도록 `dotenv` 라이브러리가 기본으로 포함되어 있습니다. (`Gemfile.lock` 파일에서 확인할 수 있습니다) `fastlane/` 경로에 `.env` 파일을 생성하고 아래와 같이 환경변수를 세팅합니다.

```
APP_IDENTIFIER=your.app.identifier
APPLE_ID=apple-id
ITC_TEAM_ID=team-id
TEAM_ID=portal-team-id
```

<br>

그 다음 `Appfile`을 아래와 같이 수정하면 끝입니다.

```
app_identifier(ENV['APP_IDENTIFIER']) # The bundle identifier of your app
apple_id(ENV['APPLE_ID']) # Your Apple email address

itc_team_id(ENV['ITC_TEAM_ID']) # App Store Connect Team ID
team_id(ENV['TEAM_ID']) # Developer Portal Team ID

```

<br>

### 2) `Fastfile`

`Fastfile`은 실제 배포를 진행하기 위해 커맨드 명령들을 설정하는 파일입니다. 파일의 기본 내용에 필요한 내용을 추가합니다.

```
default_platform(:ios)

platform :ios do
  desc "Push a new beta build to TestFlight"
  lane :beta do
    get_certificates
    get_provisioning_profile
    cocoapods(use_bundle_exec: false)
    increment_build_number(xcodeproj: "Runner.xcodeproj")
    build_app(workspace: "Runner.xcworkspace", scheme: "Runner")
    upload_to_testflight
    version = get_version_number
    send_slack({ "version": version })
  end

  lane :send_slack do |options|
    slack(
      message: "iOS 앱이 TestFlight에 성공적으로 업로드 되었습니다.",
      channel: "#deploy",
      slack_url: ENV["SLACK_WEBHOOK_URL"],
      payload: {
        "Version": options[:version]
      }
    )
  end
end
```

<br>

아래와 같이 `beta` 명령어를 실행하면,

```
fastlane beta
```

<br>

아래의 명령어들이 실행되면서 TestFlight 배포가 진행되고, Slack에 메시지를 전송하도록 설정되어 있습니다. 진행 과정에서 앱 암호(`app-specific password`) 입력이 필요합니다. 앱 암호는 Apple 계정 암호와는 다른 것이고요, 앱 암호가 없다면 [Apple 계정관리](https://appleid.apple.com/account/manage)에서 생성 후 계속 진행합니다.

```
# 인증서, 프로비저닝 프로파일을 가져오기
get_certificates
get_provisioning_profile

# pod install
cocoapods(use_bundle_exec: false)

# build 번호 변경
increment_build_number(xcodeproj: "Runner.xcodeproj")

# 앱 빌드
build_app(workspace: "Runner.xcworkspace", scheme: "Runner")

# TestFlight에 업로드
upload_to_testflight

# 현재 버전을 info.plist에서 읽어오고, Slack에 메시지 전송
version = get_version_number
send_slack({"version": version })

lane :send_slack do |options|
slack(
    message: "iOS 앱이 TestFlight에 성공적으로 업로드 되었습니다.",
    channel: "#deploy",
    slack_url: ENV["SLACK_WEBHOOK_URL"],
    payload: {
    "Version": options[:version]
    }
)
end
```

<br>

#### `slack()` 메소드

Slack으로 메시지를 보내는 메소드 설정을 커스텀하려면 `fastlane` 공식문서의 [slack](https://docs.fastlane.tools/actions/slack/) 섹션을 참고하시고요, `slack_url` 값은 [Slack Incoming Webhooks](https://api.slack.com/messaging/webhooks) URL을 생성한 후 지정합니다.

<br>

## 4. `.gitignore` 세팅하기

`fastlane beta` 혹은 `fastlane release`를 실행하고나면 `ios/` 경로에 여러 파일들이 자동으로 생성되는데요, 원격 레파지토리에 올리지 않을 파일들은 `.gitignore` 파일에 추가하는 것을 잊지마세요. 저는 아래 항목들을 추가했습니다.

```
# fastlane outputs
AppStore_com.*
*.cer
Runner.app.*.zip
Runner.ipa
```

<br>

---

### References

- [Getting started with fastlane for iOS](https://docs.fastlane.tools/getting-started/ios/setup/)
