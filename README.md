# Moru Plugins

English | [한국어](README.ko.md)

The list and the downloads of [Moru](https://github.com/taehwandev/Moru-releases)'s plugin marketplace. Plugin
sources are not here. Moru's **Plugins** area on the left rail reads `index.json` from this repository, and
**Install** downloads a plugin's zip and unpacks it.

## Writing a plugin

A plugin is a folder with a `moru-plugin.json` at its root:

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

- **Languages**: text a person reads (`description`, `readme`, release `notes`) is keyed by ISO 639-1 language code.
  Moru shows the user's language, else English, else whatever the plugin has, so add `"ja"`, `"de"`, ... whenever
  you translate. English is expected.
- **Version**: Moru's `YY.WW.N` rule (two-digit year, ISO week, release number within that week).
- **`contributes`**: where the plugin plugs into Moru. Available now: `deviceLive`, the Live switch of the Devices
  tool. `command` is relative to the plugin folder; `${project}` becomes the Task's folder and `${device}` the
  device's serial (Android) or UDID (iOS). Protocol `moru-live-events/1` prints one JSON event per line on stdout
  (`preparing`, `ready`, `applying`, `applied`, `unchanged`, `needs_rebuild`, `failed`, `stopped`) and reads `rebuild`
  and `stop` on stdin.
- To try a plugin while writing it, use **Add Local Folder** in Moru's Plugins area: it links the folder, no download.

## Publishing

1. Zip the plugin folder and host the zip at a public address (for example a GitHub Release of your repository).
2. Open a pull request to this repository adding `plugins/<name>/` (its introduction as `README.md` plus translations
   such as `README.ko.md`, and a `preview.png`) and an entry in `index.json` with the zip's address and SHA-256.
3. Once merged, it shows up in every Moru's marketplace.

## Official plugins

| Plugin | Version | What it does |
|---|---|---|
| [Android Live (Alpha)](plugins/live-android/README.md) | [26.42.7](https://github.com/taehwandev/moru-plugins/releases/tag/live-android-26.42.7) | Saves reach the running Android app in about a second, without a restart |
| [iOS Live (Alpha)](plugins/live-ios/README.md) | [26.42.8](https://github.com/taehwandev/moru-plugins/releases/tag/live-ios-26.42.8) | Saves reach the SwiftUI app in the iOS Simulator in about a second, without a restart |
