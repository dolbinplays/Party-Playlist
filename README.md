# PartyQueue — Android local party host

A local Android party-room application with guest requests/voting and an experimental **Auto DJ** mode designed to process voted-in songs without host taps.

## APK builds

GitHub Actions builds a **debug APK** on pushes to `main` and from **Actions → Build PartyQueue Android APK → Run workflow**. Successful builds publish a directly downloadable **APK under [GitHub Releases](https://github.com/dolbinplays/Party-Playlist/releases)**. On Android, open the release page, tap the `.apk` under **Assets**, and install. This avoids expiring chat sandbox links and does not require unzipping the older build artifact. GitHub Actions also retains a ZIP under each run's Artifacts section for 30 days.

**Latest tested compilation:** [v0.26.11.05.2 Auto DJ experiment](https://github.com/dolbinplays/Party-Playlist/releases/tag/v0.26.11.05.2-debug). See [Auto DJ test plan](AUTO_DJ_TEST_PLAN_v0.26.11.05.2.md). This APK is a **development/test build**, not a signed production release. This CI currently uses runner-generated debug signing keys, so Android may require uninstalling the previous debug APK before installing an update. Future source changes should trigger automated APK generation; no local Android Studio build required.

## Notes

- Auto DJ can automatically approve guest-voted requests, attempt an exact-title YouTube Music search, and send Play next using a narrowly scoped Accessibility service. This is experimental and **unverified on a real phone**; an accessibility action is not proof of actual queue insertion or correct playback order.
- LAN room server and guest interface require guests on the same network; no hosted backend.
- Repository source is based on `PartyQueue_Android_Overlay_POC_v0.26.11.03_SOURCE.zip`.
- See `SOURCE_NOTES.md` for the archived source format used in the first CI deployment.
