# exhaustism-releases

Public **update feed** for **Exhaustism** (by Dōzo). This repository contains only the
Sparkle appcast and release notes — **no application source code lives here**.

- **Feed URL** (set as `SUFeedURL` in the app's Info.plist):
  `https://raw.githubusercontent.com/ianfreeze/exhaustism-releases/main/appcast.xml`
- Release binaries (a signed `.zip` for Sparkle to download) are attached to this repo's
  **GitHub Releases**, and referenced by `<enclosure url=...>` in `appcast.xml`.

## Publishing a new version (after Sparkle is wired into the app)

1. Build + ad-hoc sign the app, then zip it (zip, not dmg, for Sparkle updates):
   `ditto -c -k --keepParent Exhaustism.app Exhaustism-x.y.z.zip`
2. Sign it for Sparkle: `./bin/sign_update Exhaustism-x.y.z.zip`
   → prints `sparkle:edSignature="..."` and `length="..."`.
3. Create a GitHub **Release** `vX.Y.Z` in this repo and upload the `.zip` as an asset.
4. Add an `<item>` to `appcast.xml` (increment `sparkle:version` = the app's
   CFBundleVersion / build number), pasting the `edSignature`, `length`, and the
   release-asset download URL. Commit + push.
5. Installed apps auto-check on launch (and via **Check for Updates…**) and offer it.

## Keys

The EdDSA **public** key is embedded in the app (`SUPublicEDKey` in Info.plist). The
**private** key lives only in the macOS **Keychain** on the release machine — it is never
committed here or anywhere in the repo.

## Note on the current alpha

0.1.0-alpha shipped as an ad-hoc-signed, non-notarized DMG **without** Sparkle embedded, so
it cannot self-update. The first build that *can* consume this feed is the next one (with
Sparkle wired in). See the app repo's `_STAGED_sound_suite_jul10/INTEGRATION_README.md`.
