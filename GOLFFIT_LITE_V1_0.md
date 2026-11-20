# GolfFit Lite V1.0.6

Minimal secure Windows installable application. Does **not** include cloud billing, membership, or venue ops.

## Build (dev host)

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File commercial-runtime\scripts\build-lite-v1.ps1
```

Output:

- `%LOCALAPPDATA%\GolfFitBuild\dist\GolfFit-Lite-Setup.exe`
- Desktop copy: `GolfFit-Lite-Setup.exe`
- Publish bundle: `%LOCALAPPDATA%\GolfFitBuild\dist\publish\` (`update-manifest.json`, `SHA256SUMS.txt`, `.cer`)

Optional:

- `$env:GOLFFIT_LITE_FORCE_FRONTEND=1` rebuild Next.js tree
- `$env:GOLFFIT_UPDATE_SETUP_URL` HTTPS URL written into the publish manifest
- `$env:GOLFFIT_CODESIGN_THUMBPRINT` use an existing code-signing cert instead of the self-signed field cert

## Install (customer PC)

1. Copy only `GolfFit-Lite-Setup.exe` to the target Windows 10/11 x64 PC.
2. Run as Administrator; accept VC++ redistributable if prompted.
3. Desktop shortcut **GolfFit Lite** → double-click.
4. UI: `http://127.0.0.1:13000` (launcher opens browser). API: `127.0.0.1:18000`.
5. Default local accounts are provisioned on first boot (see C1.3.4 docs). Change passwords.

## Secure remote update

On startup the launcher checks for a newer setup:

1. HTTPS `manifestUrl` from `C:\ProgramData\GolfFit\config\update.json` (preferred) or the packaged `assets\config\update.json`
2. Manifest must include `version`, HTTPS `setupUrl`, `sha256`, and preferably `sizeBytes` + `publisherThumbprint`
3. Download is atomic (`.partial` → final) and rejected on size/hash mismatch
4. Authenticode must match the pinned GolfFit Lite Updates thumbprint
5. Failed packages are quarantined as `*.bad`

Host `update-manifest.json` + `GolfFit-Lite-Setup.exe` on HTTPS. Point each PC’s `manifestUrl` at that JSON. For the self-signed field cert, import `GolfFit-Lite-Updates.cer` once per PC (or rely on the thumbprint pin already baked into the launcher).

## Data

- App: `C:\Program Files\GolfFit`
- Customer data: `C:\ProgramData\GolfFit` (kept on uninstall)
- Updates cache: `C:\ProgramData\GolfFit\updates`

## Camera

See `assets\CAMERA_NOTES.txt` and `assets\HIKROBOT_MVS_CAPTURE.md` inside the install. Golf simulator software is never modified.

## Baked field patches

- Hikrobot MVS capture + binding
- MediaPipe DLL bootstrap after camera PATH changes
- Learn voice / narration + autoplay allowlist
- FE UI / login field fixes
- Secure remote update channel (HTTPS + SHA-256 + Authenticode)
