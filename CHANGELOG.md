# Changelog

## 1.0.1 - 2026-11-03

- Improve S5-family video detection using `upnp:class`, extension, and
  `PANASONIC.COM_PN=CAM_AVC_*`.
- Correct S5 object-ID to card filename mapping such as `11067148` to
  `P1067148.MP4`.
- Preserve identical card filenames when more than one storage slot is returned.
- Add explicit card 1 / card 2 browsing with guarded `current_sd` switching.
- Load the newest 150 entries first instead of scanning the complete card.
- Add bounded "load 150 more" and explicit full-scan actions.
- Automatically continue searching older files when the video filter finds no
  videos in the recent window.
- Show cached thumbnails immediately and then load the camera large preview
  instead of downloading the original photo first.
- Add tests for S5 video parsing, card-slot identity, card switching, and recent
  window browsing.

## 1.0.0 - 2026-11-01

- Initial S5M2-focused Android transfer build.
- Camera Wi-Fi binding, access approval, UPnP browsing, and file transfer.
- Resumable downloads, retries, structure validation, and SHA-256 recording.
- Foreground transfer service and transfer queue.
