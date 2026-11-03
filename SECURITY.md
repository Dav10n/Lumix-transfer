# Security Policy

## Supported versions

Security and interoperability fixes are accepted for the latest `1.0.x` release.

## Reporting a vulnerability

Please use GitHub's private vulnerability reporting / Security Advisory feature
instead of opening a public issue for:

- arbitrary file write or path traversal
- requests escaping the private camera network
- accidental camera-side deletion or formatting
- credential or token disclosure
- APK signing or dependency compromise

Do not publish camera session identifiers, Wi-Fi passwords, personal file names,
or complete diagnostic logs in a public issue.

## Threat model

Luma 快传 is a local-network Android client for an undocumented Panasonic LUMIX
HTTP/UPnP interface. It assumes:

- the phone is connected directly to a trusted camera Wi-Fi network;
- the camera is at a private or link-local IP address;
- the user explicitly starts connection, browsing, card switching, and transfer;
- camera media is untrusted input and must be parsed defensively.

## Android permissions

The app requests network, Wi-Fi discovery/connection, notifications, foreground
data-transfer service, and wake-lock permissions. It does not request contacts,
SMS, microphone, camera, broad external-storage, package installation, or root
access.

## Camera commands

The app may send:

- read-only status and capability requests;
- UPnP `Browse` requests;
- camera access approval requests;
- file and thumbnail `GET` requests to the camera file server;
- `camcmd playmode` when file browsing needs playback mode;
- `setsetting current_sd=sd1|sd2` only after the user chooses a card.

It does not intentionally send capture, recording, live-view, delete, format,
firmware-update, register-write, or arbitrary setting commands.

Card switching and playback-mode changes are guarded by the UI and camera-status
checks. The app refuses card switching while a transfer is active, recording,
card writing, or interval shooting is detected. Status checks cannot eliminate
every race in an undocumented protocol.

## Residual risk

This is not Panasonic-certified software. Camera firmware may behave differently
across models and versions. A rare mode-transition or session race may leave the
camera's wireless control service temporarily unresponsive. If that happens,
stop interacting with the camera, power it off, and restart it. Do not repeatedly
send connection or mode-switch commands.

## Release signing

Community test APKs may be signed with a local debug certificate. Do not present
them as production or Play Store builds. For a long-lived public release, use a
dedicated release key that is backed up securely and never commit it to Git.
