# PartyQueue v0.26.11.05.2 — Auto DJ experimental test plan

This version is an *experimental* unattended-host milestone. It is **not** yet verified against the current YouTube Music interface on a physical Android device.

## New in this patch

- Auto DJ ON/OFF and a host-configurable **1–5 positive-vote threshold** (default 2).
- Guest votes automatically promote qualifying requests to APPROVED; positive votes must exceed negative votes.
- A serial foreground queue worker searches an exact song title in YouTube Music, opens its available item menu, and attempts to activate **Play next**.
- Only the YouTube Music package is visible to the Accessibility service; taps are restricted to exact accessible controls, not screen coordinates.
- Bounded search timeouts, ambiguity refusal, FAILED status with a retry button, and ACTION_SENT status (not proof a track entered Up Next).
- Compact overlay collapses by default. Android is requested to keep the display on during Auto DJ.
- Room changes invalidate earlier callbacks; stopping a room disables Auto DJ. No Google credentials, Firebase, GitHub hosting, or guest app required at runtime.

## First two-phone test

1. Update PartyQueue to the new APK, then on the host enable PartyQueue's overlay and Accessibility settings.
2. Connect both phones to the same Wi-Fi, start a new room, and scan the host QR code from the guest phone.
3. On the host set approval threshold to **1** for this two-phone test, start the overlay, and turn **Auto DJ ON**. Leave YouTube Music visible and the phone unlocked.
4. From the guest phone submit a clearly identified song, preferably `Du hast — Rammstein`, then cast one upvote.
5. Without touching the host, look for REQUESTED -> APPROVED -> SEARCHING -> ACTION_SENT (or FAILED with a specific error) on the dashboard.
6. Check the actual YouTube Music **Up next** list. ACTION_SENT is not verification.
7. Repeat with two other unambiguous tracks. Confirm no taps are required on the host *if* the current YouTube Music UI exposes compatible controls.
8. Toggle Auto DJ OFF and confirm new guest votes do not trigger playback automation. Verify no additional actions after stopping the room.

## Known constraints

- YouTube Music provides no official public queue-control API. UI automation is brittle and depends on the exact version, accessibility labels, visible app, unlocked display, and service terms.
- Search is initiated within YouTube Music by the Accessibility adapter; matching deliberately refuses multiple exact matches instead of guessing wrong versions.
- Title matching works best with clear song titles, optionally followed by ` — Artist`. The code does not yet resolve video IDs from guest links, so requests may fail even when a URL is supplied.
- Each Play next call is an *unverified* action; the app cannot yet independently confirm queue membership or guarantee ordering when several tracks qualify.
- Room state is in memory and does not survive the Android process being killed.
- Host app must run on the same LAN as guests; Android battery/background policies may interrupt the LAN server, especially while screen locked.

## Acceptance criteria for a future FULL AUTO release

A five-track end-to-end test on a real phone: guests submit/vote, tracks are chosen unambiguously, YouTube Music acknowledges queue insertion, audio advances in intended order, no host interactions are necessary after setup, and failures are handled without freezing the party. This version has **not** passed that acceptance test.
