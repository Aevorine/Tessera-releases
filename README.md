# Tessera — encrypted vault + PC cleanup for Windows

**English** · [简体中文](README.zh-CN.md) · [Français](README.fr.md) · [Español](README.es.md) · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Download](https://img.shields.io/badge/download-latest%20release-brightgreen.svg)](../../releases/latest)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-lightgrey.svg)]()
[![Android](https://img.shields.io/badge/android-companion%20app-green.svg)](../../releases/latest)
[![Languages](https://img.shields.io/badge/languages-6-blue.svg)]()

**Your files, passwords, clipboard and notes behind one unlock — plus a cleanup suite that
keeps the machine underneath them tidy.** Nothing is uploaded, nothing phones home, and
nothing leaves your computer unless you explicitly set it up to.

Free. Offline-first. Windows 10/11 64-bit, with an Android companion for clipboard and files.

## Download

**→ [Get the latest release](../../releases/latest)**

| File | Who it's for |
| --- | --- |
| `Tessera-Setup.exe` | Windows installer — pick a location, gets an uninstall entry. **Most people want this.** |
| `Tessera.exe` | Portable. Runs from anywhere including a USB stick, touches no registry. |
| `Tessera-CrossDevice-<version>.apk` | Android phone or tablet (clipboard, files, sync). |

After installing you never need to come back here — the app finds new versions on its own.

## What you get

### An encrypted vault

- **Files** — encrypt anything into a single `.ivault`. Post-quantum by default (AES-256-GCM
  for the content, the per-file key additionally wrapped by ML-KEM-1024), or classic
  AES-256-GCM / ChaCha20-Poly1305. Filenames and checksums live *inside* the encrypted
  header, so a stolen container reveals neither.
- **Passwords** — full manager, import from a Chrome/Edge/Firefox CSV, and the plaintext CSV
  is securely deleted afterwards.
- **Clipboard history** — auto-classified (URL / email / phone / formula / code / text),
  pinning, search, per-app blacklist, and a global `Ctrl+Shift+V` panel.
- **Notes** — Markdown with images, nested categories, full-text search. Every note returns
  you to where you stopped reading. A 4 MB note shows its first screen in about 40 ms.
- **Five ways to unlock, any one is enough** — password · email code · authenticator (TOTP) ·
  Windows Hello (face / fingerprint / PIN) · one-time recovery key.

### A cleanup suite that tells you the truth

- Junk files, browser caches, privacy traces, driver leftovers, duplicate and large files,
  C: drive slimming, startup manager, right-click menu, popup blocker, and 42 small utilities.
- **Three scan depths** — standard (~15 s), deep, and extreme (all 21 categories).
- **A dry run walks the real code path without touching a byte**, and reports what each drive
  would actually gain.
- **Every category says which kind of empty it found** — genuinely clean, blocked by
  permissions, or never actually ran — and how many locations it checked to get there.
  "Nothing found" and "it never ran" are not allowed to look the same.
- **"Freed" means freed.** Files sent to the Recycle Bin are counted *separately* from space
  actually reclaimed, and the app re-measures the drive afterwards so you can check its
  arithmetic against what Windows reports.
- Five judgements on every item — *Windows needs this · a driver · something you use ·
  optional · an advertisement* — with the reason written out. What Windows needs is locked,
  and the backend refuses it even if asked.
- Registry keys, startup entries and menu handlers are **disabled with a backup**, never
  deleted. If the backup export fails, nothing is changed at all.

### Phone ↔ PC, over your own LAN

Copy on your phone, paste on your PC, and the other way round — text, rich text, code, maths
formulas and images keep their formatting. Send files or whole folders, resumable and
end-to-end encrypted. Pair with a six-digit code, a QR code, a link, or off a nearby-devices
list. Traffic stays on your local network.

### Interface

One design language across every screen. Eight accent colours, light/dark/system, Simple or
Professional depth, and an eye-comfort mode that shifts colour temperature **without** touching
status colours — comfort should not cost you the ability to see what went wrong.

**Six languages** — English, 简体中文, Français, Español, Русский, العربية — switchable
anywhere, including on the login screen.

## Installing: what Windows and Android will ask

**Windows.** Builds are signed with a self-signed certificate, so SmartScreen may warn
("Windows protected your PC"). Click **More info → Run anyway**. That warning is about the
certificate not being bought from a commercial CA — not about the file being altered.

In-app updates use the same signing key: after downloading, the app checks the signature is
*this* key, and **deletes the file rather than installing if it is not**. There is no
"proceed anyway" path.

**Android.** The first in-app update asks you to allow installing unknown apps for Tessera.
Android never lets a sideloaded app install anything silently — that final confirmation is
always the system's, and the app cannot bypass it. Upgrades require a matching signature,
enforced by Android itself, so nobody else can push a build that impersonates this app.

## Verify what you downloaded

GitHub shows a SHA-256 for every asset on the release page. In-app updates compare it
automatically; if you downloaded by hand:

```powershell
Get-FileHash .\Tessera-Setup.exe -Algorithm SHA256
```

```bash
sha256sum Tessera-CrossDevice-*.apk
```

You can also right-click the exe → **Properties → Digital Signatures** to see the signature.

## System requirements

Windows 10 or 11, 64-bit. Several features are bound to Windows APIs (Windows Hello, shell
integration, global file hotkeys). Android companion: Android 8.0 or newer.

## Questions

**Is the source public?** Not at this time. This repository holds release builds only.

**Does it phone home?** No. The only outbound request the app makes on its own is the update
check against this repository, and you can switch that to manual.

**Where does my data live?** In an encrypted database, in a folder you choose during setup.
Updates never touch it.

**I lost my password.** Use the one-time recovery key from setup. Without it, and without any
of the other four unlock methods, the vault cannot be opened — that is what it is for.

## Licence

AGPL-3.0, with a commercial licence also available. Full terms ship with the application and
are visible in its About page.
