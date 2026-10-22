# CoveChat release channel

This public repo is the update channel for the CoveChat Android app
(2.0.5+). The app polls **`version.json`** in this repo (raw URL below) and
downloads the APK named there — updates happen inside the app, no links are
ever sent to users.

- Manifest the app reads:
  `https://raw.githubusercontent.com/imran866565-debug/covechat-releases/main/version.json`

## How to launch a new version

1. Build the new APK (in `covechat-android`: `./build.sh`) and note its
   `versionCode` / `versionName` and SHA-256 (`sha256sum build/covechat-final.apk`).
2. Upload the APK into this repo with a versioned name, e.g.
   `covechat-2.0.6.apk`. **Upload the APK first** — the manifest must never
   point at a file that is not here yet.
3. Edit `version.json` (upload it last):
   - `latestVersionCode` / `latestVersionName` — the new build's numbers.
   - `apkUrl` — the raw URL of the file from step 2:
     `https://raw.githubusercontent.com/imran866565-debug/covechat-releases/main/covechat-X.Y.Z.apk`
   - `notes` — one or two short lines shown in the app's update prompt.
   - `sha256` — the new APK's hash (the app verifies the download).
   - `minSupportedVersionCode` — leave at its current value for a normal
     (optional) release.
4. Done. Phones on 2.0.5+ pick it up at next app start (dismissible
   "Update available" prompt + Settings → App Update).

## Strict (forced) update

To force everyone onto a release, set `minSupportedVersionCode` in
`version.json` to the lowest versionCode allowed to keep running (usually
the new `latestVersionCode`). Any phone below it gets a blocking,
non-dismissible "Update your app" screen until it updates. Keep it at `1`
during normal rollouts so nobody is locked out.

## Notes

- Builds ≤ 2.0.4 have no updater inside them; 2.0.5 is the last release
  that has to be delivered as a file/link by hand.
- All APKs are signed with the same project key, so updates install over
  the old app and accounts/chats stay intact.
