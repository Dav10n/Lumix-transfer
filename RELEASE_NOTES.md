# Luma 快传 1.0.1

Community test build for Panasonic LUMIX S5M2.

## Highlights

- Faster startup: the newest 150 camera items are shown first.
- Better S5 video detection for MP4/MOV files with numeric titles.
- Separate card 1 / card 2 browsing for S5M2 dual-slot setups.
- Cached thumbnail shown immediately, followed by the camera large preview.
- Bounded “load 150 more” and explicit full-card scan.

## Safety boundary

This is an unofficial local-network interoperability tool. It does not send
capture, recording, live-view, delete, format, or firmware commands. Playback-mode
and explicit current-card selection are the only camera state changes exposed by
the app.

The file server exposes one current card at a time. The app does not send commands
that modify Allocation Recording. Users should verify their camera slot settings
after switching the card being browsed.

## Installation

- Android 10 or newer
- Sideload `LumaTransfer-S5M2-1.0.1.apk`
- Close LUMIX Lab / LUMIX Sync before connecting
- Approve the app on the camera screen

## Verification

- 36 protocol tests passed
- 5 core tests passed
- Release Lint Vital passed
- APK v2 signature verified

SHA-256:

```text
43b6df96d2027b5a58734596f46cc8601ad71928e51ffe14f2317edc3d2bf348
```

This APK uses a local debug certificate and is not a Google Play production build.

## Known limitations

- Unofficial, undocumented camera API
- More S5M2 firmware and phone combinations need real-device reports
- Complete scans of large cards can still be slow
