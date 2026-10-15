# Moru Plugins

[English](README.md) | 한국어

[Moru](https://github.com/taehwandev/Moru-releases)의 플러그인 마켓 목록과 배포 파일을 두는 곳이에요. 플러그인 소스는
여기에 없어요. Moru 왼쪽 레일의 **플러그인** 화면이 이 저장소의 `index.json`을 읽어 목록을 보여 주고, **설치**를
누르면 플러그인 압축 파일을 내려받아 풀어요.

## 플러그인 만들기

플러그인은 맨 위에 `moru-plugin.json`이 있는 폴더 하나예요.

```json
{
    "schema": 1,
    "id": "dev.example.my-plugin",
    "name": "My Plugin",
    "version": "26.42.0",
    "author": "you",
    "description": { "en": "One line about what it does.", "ko": "무엇을 하는지 한 줄" },
    "readme": { "en": "README.md", "ko": "README.ko.md" },
    "contributes": {
        "deviceLive": {
            "platform": "android",
            "command": ["bin/my-plugin", "watch", "--project", "${project}", "--serial", "${device}"],
            "protocol": "moru-live-events/1"
        }
    }
}
```

- **언어**: 사람이 읽는 글(`description`, `readme`, 배포 `notes`)은 ISO 639-1 언어 코드별로 적어요. Moru는 사용자의
  언어, 없으면 영어, 그것도 없으면 있는 아무 언어를 보여 줘요. 번역할 때마다 `"ja"`, `"de"` 같은 코드를 추가하면 되고,
  영어는 꼭 넣어 주세요.
- **버전**: Moru와 같은 `YY.WW.N` 규칙이에요(연도 두 자리, ISO 주차, 그 주의 릴리스 번호).
- **`contributes`**: 플러그인이 Moru의 어디에 붙는지예요. 지금 있는 자리는 `deviceLive`(Devices 도구의 Live 스위치)예요.
  `command`는 플러그인 폴더 기준 경로이고, `${project}`는 Task의 폴더, `${device}`는 기기 serial(Android) 또는
  UDID(iOS)로 바뀌어요. 프로토콜 `moru-live-events/1`은 표준 출력에 한 줄에 하나씩 JSON 이벤트(`preparing`, `ready`,
  `applying`, `applied`, `unchanged`, `needs_rebuild`, `failed`, `stopped`)를 쓰고, 표준 입력으로 `rebuild`와 `stop`을 받아요.
- 만들면서 써 보려면 Moru 플러그인 화면의 **내 폴더 추가**로 폴더를 바로 연결하세요. 내려받기 없이 동작해요.

## 마켓에 올리기

1. 플러그인 폴더를 zip으로 묶어 공개적으로 내려받을 수 있는 곳에 올려요(예: 자기 저장소의 GitHub Release).
2. 이 저장소에 `plugins/<이름>/`(소개 글 `README.md`와 `README.ko.md` 같은 번역, `preview.png`)를 추가하고,
   `index.json`에 zip 주소와 SHA-256을 담은 항목을 하나 추가하는 PR을 보내요.
3. 병합되면 모든 Moru의 마켓에 나타나요.

## 공식 플러그인

| 플러그인 | 버전 | 하는 일 |
|---|---|---|
| [Android Live](plugins/live-android/README.ko.md) | [26.42.1](https://github.com/taehwandev/moru-plugins/releases/tag/live-android-26.42.1) | 저장하면 실행 중인 Android 앱에 약 1초 만에 반영 (재시작 없음) |
| [iOS Live](plugins/live-ios/README.ko.md) | [26.42.0](https://github.com/taehwandev/moru-plugins/releases/tag/live-ios-26.42.0) | 저장하면 iOS 시뮬레이터의 SwiftUI 앱에 약 1초 만에 반영 (재시작 없음) |
