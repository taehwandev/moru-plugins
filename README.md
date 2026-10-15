# Moru Plugins

[Moru](https://github.com/taehwandev/Moru-releases)의 플러그인 마켓 목록과 배포 파일을 두는 곳이에요. 플러그인
소스는 여기에 없어요. Moru 왼쪽의 **플러그인** 화면이 이 저장소의 `index.json`을 읽어 목록을 보여 주고, `설치`를
누르면 각 플러그인의 압축 파일을 내려받아 풀어요.

This is the list and the downloads of Moru's plugin marketplace; plugin sources live elsewhere. Moru's Plugins area
reads `index.json` here and installs a plugin by downloading and unpacking its zip.

## 플러그인 만들기 / Writing a plugin

플러그인은 폴더 하나예요. 맨 위에 `moru-plugin.json`이 있어야 해요.

```json
{
    "schema": 1,
    "id": "dev.example.my-plugin",
    "name": "My Plugin",
    "version": "26.42.0",
    "author": "you",
    "description": "One line about what it does.",
    "description_ko": "무엇을 하는지 한 줄",
    "readme": "README.md",
    "contributes": {
        "deviceLive": {
            "platform": "android",
            "command": ["bin/my-plugin", "watch", "--project", "${project}", "--serial", "${device}"],
            "protocol": "moru-live-events/1"
        }
    }
}
```

- `version`은 Moru와 같은 `YY.WW.N` 규칙이에요(연도 두 자리, ISO 주차, 그 주의 릴리스 번호).
- `contributes`는 플러그인이 Moru의 어디에 붙는지예요. 지금 있는 자리는 `deviceLive`(Devices 도구의 Live 스위치)예요.
  `command`는 플러그인 폴더 기준 경로이고, `${project}`는 Task의 폴더, `${device}`는 기기 serial(Android) 또는
  UDID(iOS)로 바뀌어요. 프로토콜 `moru-live-events/1`은 표준 출력에 한 줄에 하나씩 JSON 이벤트(`preparing`,
  `ready`, `applying`, `applied`, `unchanged`, `needs_rebuild`, `failed`, `stopped`)를 쓰고, 표준 입력으로
  `rebuild`와 `stop`을 받아요.
- 만들면서 써 보려면 Moru 플러그인 화면의 **내 폴더 추가**로 폴더를 바로 연결하세요. 내려받기 없이 동작해요.

## 마켓에 올리기 / Publishing

1. 플러그인 폴더를 zip으로 묶어 공개적으로 내려받을 수 있는 곳에 올려요(예: 자기 저장소의 GitHub Release).
2. 이 저장소에 `plugins/<이름>/README.md`(마켓 상세 화면에 보이는 소개 글)와 `preview.png`를 추가하고,
   `index.json`에 항목을 하나 추가하는 PR을 보내요. `sha256`은 zip의 SHA-256이에요.
3. 병합되면 모든 Moru의 마켓에 나타나요.

Zip the plugin folder, host it at a public URL, then open a PR adding `plugins/<name>/` (README and preview) and
an entry in `index.json` with the zip's URL and SHA-256.

## 공식 플러그인 / Official plugins

| 이름 | 버전 | 하는 일 |
|---|---|---|
| [Android Live](plugins/live-android/README.md) | [26.42.0](https://github.com/taehwandev/moru-plugins/releases/tag/live-android-26.42.0) | 저장하면 실행 중인 Android 앱에 약 1초 만에 반영 (재시작 없음) |
