# Android source packaging and CI

## Source checkpoint

The initial Android project is archived under `source/partyqueue.tar.xz.b64.part01`–`part04` (four ordered Base64 text chunks). This archive was produced from the November 3, 2026 PartyQueue Android Overlay proof-of-concept and contains the Android Studio project's Java, Gradle, manifest, assets, and static guardrail test.

To restore the project on macOS/Linux:

```bash
cat source/partyqueue.tar.xz.b64.part* | tr -d '\\r\\n' | base64 --decode > /tmp/partyqueue-source.tar.xz
printf '%s  %s\\n' '5cd74a6c22f84d8c2803805c8924e759168c666d48b3424cae149db19ac73b0f' /tmp/partyqueue-source.tar.xz | sha256sum -c
mkdir -p restored-project
tar -xJf /tmp/partyqueue-source.tar.xz -C restored-project
```

The GitHub Actions workflow performs the same checksum verification before building. For future changes, prefer committing **ordinary readable source files** and changing the workflow to compile those directly; the chunked source archive is a bootstrap of the existing POC, not the desired long-term repo layout.

## Build results

Go to **Actions → Build PartyQueue Android APK**. The workflow runs automatically on main-branch changes to the source/archive/workflow and can also be started manually with **Run workflow**. A successful run publishes `PartyQueue-v0.26.11.03.1-debug-apk` under Artifacts. Click, download, unzip, and install `app-debug.apk` on your phone.

The debug build is not a Play Store release, and clicking the YouTube Music Play Next overlay helper is experimental and requires explicit Android permissions and user action. No proof of device compatibility is implied by successful compilation.
