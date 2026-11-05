# PartyQueue — Android local party host

A local Android party-room application with guest requests/voting and a host-controlled experimental YouTube Music overlay helper.

## APK builds

GitHub Actions builds a **debug APK** on pushes to `main` and from **Actions → Build Android APK → Run workflow**. After a successful run, open the run's **Artifacts** section and download `PartyQueue-debug-apk`. Unzip the downloaded artifact to get `app-debug.apk`; install it on your Android phone.

This APK is a **development/test build**, not a signed production release. Future source changes should trigger automated APK generation; no local Android Studio build required.

## Notes

- Play Next is currently an assisted, user-initiated proof of concept only. YouTube Music is a separate service/app and no autonomous queue control or verified playback is implemented.
- LAN room server and guest interface require guests on the same network; no hosted backend.
- Repository source is based on `PartyQueue_Android_Overlay_POC_v0.26.11.03_SOURCE.zip`.
- See `SOURCE_NOTES.md` for the archived source format used in the first CI deployment.
