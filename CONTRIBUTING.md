# Contributing

Thanks for helping improve Luma 快传.

## Development requirements

- JDK 17
- Android SDK 35
- Gradle 8.9 or the included wrapper

Run before opening a pull request:

```bash
./gradlew :protocol:test :core:testDebugUnitTest :app:assembleDebug
```

## Camera-safety rules

- File transfer operations are read-only.
- Do not add capture, recording, live-view, delete, format, firmware, or arbitrary
  setting commands without an explicit product decision and a visible warning.
- Any camera state-changing command must be user-triggered, narrowly allowlisted,
  status-guarded, documented, and covered by tests.
- Never add repeated mode-switch attempts to work around `err_busy`.
- Do not send commands while a transfer, recording, card write, or interval run
  is detected.
- Preserve the upstream Apache-2.0 attribution and NOTICE.

## Pull requests

Include:

- the user-facing behavior being changed;
- security implications;
- tests added or updated;
- camera model and firmware for device-specific claims;
- screenshots for visible UI changes when possible.

Do not commit:

- `local.properties`
- Android SDK or Gradle caches
- APK/AAB files
- keystores or signing passwords
- personal camera names, SSIDs, tokens, or unredacted diagnostic logs

## Real-device reports

State what was actually tested. Use "code complete", "emulator tested", or
"real device tested" accurately. A command returning `ok` is not proof that a
file transfer completed.
