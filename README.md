# PartyQueue — Android local party host

A local Android party-room application with guest requests/voting and a host-controlled experimental YouTube Music overlay helper.

## APK builds

GitHub Actions builds a **debug APK** on pushes to `main` and from **Actions → Build PartyQueue Android APK → Run workflow**. Successful builds publish a directly downloadable **APK under [GitHub Releases](https://github.com/dolbinplays/Party-Playlist/releases)**. On Android, open the release page, tap the `.apk` under **Assets**, and install. This avoids expiring chat sandbox links and does not require unzipping the older build artifact. GitHub Actions also retains a ZIP under each run's Artifacts section for 30 days.

This APK is a **development/test build**, not a signed production release. Future source changes should trigger automated APK generation; no local Android Studio build required.

## Notes

- Play Next is currently an assisted, user-initiated proof of concept only. YouTube Music is a separate service/app and no autonomous queue control or verified playback is implemented.
- LAN room server and guest interface require guests on the same network; no hosted backend.
- Repository source is based on `PartyQueue_Android_Overlay_POC_v0.26.11.03_SOURCE.zip`.
- See `SOURCE_NOTES.md` for the archived source format used in the first CI deployment.
