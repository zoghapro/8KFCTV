# 8K FCTV

An IPTV player for Android phones, tablets and Android TV / Fire TV.
It plays **your own** playlists — you supply either Xtream Codes credentials
or an M3U URL. The app ships with no content and no servers of its own.

## What works

| Area | Detail |
|---|---|
| Sources | Xtream Codes login, or any M3U / M3U8 URL |
| Multiple playlists | Save several, switch between them, delete with their data |
| Live TV | Category sidebar, search, favorites, now/next EPG |
| Movies | Poster grid, detail page with plot and cast, resume playback |
| Series | Seasons, episodes, per-episode resume |
| Player | HLS + DASH + progressive, channel zapping, aspect toggle, resume |
| Parental lock | 4-digit PIN, lockable categories |
| TV remote | Full d-pad focus handling, channel up/down, seek, play/pause |

## Building

No Android Studio needed — pushing to `main` builds the APK in GitHub Actions
and uploads it as an artifact. Locally:

```bash
./gradlew :app:assembleDebug
# app/build/outputs/apk/debug/app-debug.apk
```

Requires JDK 17 and the Android SDK (compileSdk 36).

### If a dependency fails to resolve

`app/build.gradle` pins the versions at the top:

```gradle
ext.media3Version = '1.4.1'
ext.glideVersion  = '4.16.0'
```

Bump `media3Version` if Gradle reports it cannot find the artifact.

## Layout of the code

```
data/     Store (prefs), Repo (cache), XtreamClient, M3uParser
model/    Playlist, MediaEntry, Category, Episode, SeriesDetail, EpgProgram
ui/       Splash → Playlists → Home → Catalog → Detail → Player, plus Settings
util/     Http, Task (background work), Fmt
```

`Repo` caches each section in memory, so switching categories doesn't re-hit
the network. `Store` is the only thing that touches disk.

## Remote control keys

| Key | Live TV | Movies & series |
|---|---|---|
| Up / Down | Previous / next channel | — |
| Left / Right | — | Back 10s / forward 30s |
| OK | Show channel banner | Show controls |
| Menu / Info | Cycle aspect ratio | Cycle aspect ratio |

## Not built yet

- Full XMLTV EPG grid (only Xtream's now/next is wired up)
- Catch-up / archive playback
- Subtitle and audio track picker
- Recording

## Note on content

This is a player, like VLC. What you point it at is your responsibility —
use playlists you have the right to watch.
