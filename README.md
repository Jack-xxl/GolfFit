# GolfFit

Commercial GolfFit Lite releases live here.

## Store auto-update

1. Install `GolfFit-Lite-Setup.exe` from the latest [Release](https://github.com/Jack-xxl/GolfFit/releases).
2. On the store PC create `C:\ProgramData\GolfFit\config\update.json`:

```json
{
  "checkOnStartup": true,
  "manifestUrl": "https://github.com/Jack-xxl/GolfFit/releases/download/v1.0.6/update-manifest.json"
}
```

3. Later versions upload as new GitHub Releases; the store PC discovers them on next launch (HTTPS + SHA-256 + Authenticode).

Installer binaries are attached to Releases (not stored in git).
