# PartyQueue — Unattended Host Mode (Required Product Behavior)

Status: design requirement, not yet implemented or verified
Updated: 2026-11-05

## Definition of success

Once the host launches a local room and explicitly enables **Auto DJ**, PartyQueue must accept guest submissions and votes, decide which requests qualify, enqueue songs, and continue music playback **without per-song taps or approvals on the host device**.

An assisted-only Play Next button does **not** satisfy the requirement.

## Existing verified foundation (v0.26.11.03.1)

- Android host can create a local Wi-Fi room and QR code.
- A second phone can join and submit a song; requests appear on the host.
- Overlay and Accessibility helper are enabled, and YouTube Music can remain open.
- There is **no evidence yet** that assisted Play Next actually modifies YouTube Music's queue.
- Existing code requires manual host approval, song search, menu opening, and Play Next action.

## Autonomous pipeline

1. Guest selects a specific recording (prefer canonical YouTube Music video link/ID, not a free-text title alone), submits it, and votes.
2. Deterministic host-configured rules (e.g. distinct guest vote threshold, duplicate checks, rate limiting, cooldowns) determine whether a request qualifies.
3. Local queue controller grants one job at a time, records state transitions, enforces timeout, and never replays a job on accidental retries.
4. Playback adapter looks up or opens the exact selected song.
5. Adapter requests **Play Next**, reads back/validates queue result where supported, and only then records confirmed success. Never equate an accessibility click with confirmed queue insertion.
6. On ambiguity, unavailable track, or UI mismatch: skip/retry within a bounded limit and log the failure; keep the party running.
7. Host can stop auto mode immediately, change rules, reject requests, and view errors; none of these actions is required for ordinary songs.

## Playback adapters and constraints

### YouTube Music Accessibility adapter (experimental)
- No unsupported claims of reliable playback automation.
- YouTube Music has no supported public API for controlling the app's Up Next queue.
- Automated interactions with YouTube Music can be fragile or restricted by YouTube's Terms. Evaluate service authorization before deploying an autonomous UI bot.
- Android UI automation generally requires the device unlocked, YouTube Music visible, and Accessibility permission. Expect failures on UI changes; do not bypass OS security or permissions.
- Confine any interaction to the exact YouTube Music package and a narrow, static, user-configured rule set. No password capture, arbitrary navigation, or taps in unrelated apps.
- Prototype exact track identification and visible menu selection on a real phone before treating as unattended.

### Official embedded player (robust fallback)
- Local guest server and vote engine remain unchanged.
- A host-facing YouTube IFrame player can advance the queued video IDs under PartyQueue control after the host starts playback, subject to YouTube player rules and mobile autoplay/background restrictions.
- This is not the same as driving the YouTube Music Android app, but is more supportable as an automatic jukebox.

## First development milestone
Build and test the local queue/state-machine and auto-approval settings independently of playback, then validate one-song search/queue insertion and queue confirmation on device. Do not label any version "fully unattended" until a sustained multi-song, no-touch test passes.

## Release preference
Build via GitHub Actions and publish downloadable Android APK assets to GitHub Releases. Avoid temporary ChatGPT file links and don't ask the user to compile APKs.
