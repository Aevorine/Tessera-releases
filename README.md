# Tessera

**English** · [简体中文](README.zh-CN.md) · [Français](README.fr.md) · [Español](README.es.md) · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Licence: AGPL-3.0](https://img.shields.io/badge/licence-AGPL--3.0-blue.svg)](LICENSE) [![Commercial licence available](https://img.shields.io/badge/commercial%20licence-available-brightgreen.svg)](LICENSE-COMMERCIAL.md) [![Platform: Windows](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-lightgrey.svg)]()

**Tessera** — a privacy-first Windows toolkit built on one idea: **your data stays on your
machine.** A multi-factor encrypted vault holds your files, passwords, clipboard history and
notes behind a single unlock; a full set of cleanup tools keeps the machine underneath them
tidy — junk, privacy traces, startup entries, duplicate files, the right-click menu, and a
toolbox of the small utilities Windows buries several clicks deep.

Nothing is uploaded, nothing phones home, and nothing leaves your machine unless you
explicitly configure it to.

> Status: `0.1.18`, under active development. Windows 10/11 x64 only — several features
> (Windows Hello, shell integration, global file hotkeys) are bound to Windows APIs.

---

## Contents

- [What's inside](#whats-inside)
- [Getting started](#getting-started)
- [First run: the setup wizard](#first-run-the-setup-wizard)
- [Setting up email-code login](#setting-up-email-code-login)
- [Setting up the authenticator app](#setting-up-the-authenticator-app)
- [Setting up biometric unlock](#setting-up-biometric-unlock)
- [Recovery key — read this one](#recovery-key--read-this-one)
- [Everyday use](#everyday-use)
- [Cleaning up the computer](#cleaning-up-the-computer)
- [Cross-device sync](#cross-device-sync)
- [Security model](#security-model)
- [Where your data lives](#where-your-data-lives)
- [Development](#development)
- [Building a release](#building-a-release)
- [License](#license)

---

## What's inside

| Feature | What it does |
|---|---|
| **File encryption** | Encrypt/decrypt any file to a single `.ivault` file. Three profiles: **post-quantum** (default — AES-256-GCM for the content, with the per-file key additionally wrapped by ML-KEM-1024, so recovering it means breaking both constructions), classic AES-256-GCM, or ChaCha20-Poly1305. The profile applies identically to multi-selected files and to a whole folder. Batch-select many files at once. Filenames and checksums live in an *encrypted* header, so a stolen `.ivault` reveals neither. Decryption has its own folder entry alongside the file one, and opening an encrypted folder shows what is inside it without decrypting the container first - the listing reads only the few hundred KB the archive index sits in, so a 10 GB folder opens as fast as a small one, and extraction writes each file exactly once instead of staging the whole tree twice. |
| **Secure delete + remnant scan** | Optionally overwrite-then-delete the original after encrypting, and scan for plaintext leftovers Office/WPS backups and same-name copies elsewhere on disk left behind. Shredding a folder walks the whole tree: read-only files - a folder's own `desktop.ini`, anything restored from an archive - are cleared and overwritten rather than aborting the sweep, and whatever is genuinely stuck, such as a file still open in another program, is named in the error. A source that survived is never reported as a completed deletion. |
| **Password manager** | Full CRUD, masked by default, import from a Chrome/Edge/Firefox password CSV (the plaintext CSV is securely deleted after import), export back to Chrome/Edge format. Recovery-code import can shred the file it read from, ticked by default: that file holds recovery codes in plain text, and leaving it in the downloads folder after importing means the import protected nothing. |
| **Clipboard history** | Encrypted history with auto-classification (URL / email / phone / **formula** / code / text), pinning, search, virtual scrolling, per-app blacklist, recycle bin, tray icon, and a global `Ctrl+Shift+V` panel. Click a thumbnail to open the full image (fullscreen or floating window, your choice) with zoom, pan and click-to-copy. Export everything / pinned / hidden / just what you ticked, with multi-select and select-all; import several files at once and securely delete the originals afterwards. An import never leaves you holding a second copy of something already there: a matching entry is merged into rather than duplicated, and its pinned and hidden marks are kept. Duplicates already in the vault get a cleanup that lists what it found and merges only once you confirm, with whatever it folds away going to the recycle bin. |
| **Notes** | Markdown notes with images, categories, and full-text search, stored in an encrypted database. Categories sort by name, note count, last used — or an order you arrange yourself. Four counters sit above the list — Total, Active, Hidden, Trashed — and each one opens what it counts: click Hidden to see those notes, click Trashed to open the recycle bin, click again to come back. The two that would put hidden notes on screen ask for identity first, using the same unlock as everywhere else, and a hidden note that has been deleted stays behind that check in the recycle bin too — the bin says how many entries it is holding back so a short list never looks like lost data. Every note remembers where you stopped reading or editing and returns you there when you reopen it, including in notes large enough to be paged. A large note reaches the screen in milliseconds and stays responsive while you type: the document is rendered block by block on a background thread, so the first screen does not wait for the rest — a 4 MB note shows its opening in about 40 ms rather than a second and a half. Notes move into a hidden category in one step from the list, several or all at once, and a folder import can land straight in one. Categories nest as deeply as you like, in the hidden area exactly as in the visible one. |
| **AI assistant** | Chat against any OpenAI-compatible endpoint (DeepSeek by default). The model list is fetched live from the provider and shown with each model's release date, newest first, so you can see what you are choosing; models the app has never heard of still appear and stay selectable, and retired ones are labelled rather than failing silently. Picking one applies it to every AI feature in the app. Importable "skills" using the same `SKILL.md` format as Claude Code. Conversation history is stored encrypted. |
| **File manager** | High-speed copy / cut / paste / delete that interoperates with Explorer's own clipboard, with a live progress card. Global `Ctrl+V` / `Delete` / `Shift+Delete` hooks. |
| **PC cleanup** | Five panels that keep the machine itself in order. One checkup scans junk files, browser caches, privacy traces, plugins, startup entries, popup sources and stale registry references at once, and fills in as each part finishes rather than making you watch a still screen. Disk space covers system-drive slimming, large files, space by folder, duplicate files (compared by size, then by a sample of each file, then in full) and drive optimisation that runs TRIM on an SSD instead of defragmenting it. Startup and menus lists what runs at sign-in — registry, startup folder, scheduled tasks and services — plus every entry in your right-click menu, and a popup blocker that both stops the things producing popups from starting and closes the ones that get through. Installed software lists everything on the machine, and a toolbox holds forty-two small utilities, searchable and pinnable. Before a cleanup runs, a dry run walks the real code path without touching a byte and reports what each drive would gain and how much could not be taken back. Browser data is itemised per browser and per profile — cookies, history, form data, site storage — with password stores and bookmarks never listed and impossible to remove from here. Driver packages superseded by newer versions are usually the largest dead weight on a system drive, and Windows' own disk cleanup does not touch them. Empty folders, shortcuts pointing nowhere and zero-byte files are gathered from the desktop, documents and downloads. Installed software is ranked by size *and* by how long it has sat untouched, because four gigabytes only becomes a reason to uninstall once you know you have not opened it in a year. The cleanup action sits at the top of the page, not the bottom: one dial that reads “scan” before a run and turns into “clean, 6.4 GB” after it, plus a bar that follows you down a long list. Every category says which kind of empty it found — genuinely nothing to clean, blocked by permissions, or never actually ran — and how many locations it checked to get there. Panels open on the previous result while a fresh scan runs behind them. You can add rules pointing at your own folders — never at a drive root, a system folder or the vault's own data — and let a scheduled pass look without deleting. |
| **Knowing what is safe to remove** | Every item carries one of five judgements — Windows needs this, a driver, something you use, optional, an advertisement — with the reason written out in words. What Windows needs is locked and cannot be selected, and the backend refuses it even if asked. Anything unrecognised is called optional, never junk: missing a cache file costs megabytes, removing a driver costs an afternoon. |
| **Nothing removed you cannot get back** | Caches and temporary files are deleted outright — that is what frees the space. Your own files go to the Recycle Bin. Registry keys, startup entries and context-menu handlers are *disabled with a backup*, never deleted, so each one goes back with one click. A registry backup is exported before any change, and if the export fails nothing is changed at all. |
| **Network speed test** | Run and chart connection tests, with history. Throughput is read the way public speed tests read it: the connection is sampled in quarter-second slices and the figure quoted is the 90th percentile of the steady state, so TCP's slow-start ramp and one-off dips do not drag the number below what the line actually delivers. Latency is a TCP round trip rather than a full HTTPS request, which keeps the server's own processing time out of it, and jitter is the average change between consecutive samples - the quantity that decides whether a call or a game holds up. Each result carries a stability figure saying how far the fast and slow moments of the test were apart, which is what tells you whether a different number next time means anything. |
| **Signed auto-update** | The release carries **two files: an installer and a portable exe**, one per flavour. Trust comes from the Authenticode signature embedded inside it, checked against a publisher name and public-key fingerprint baked into the build — so a free self-signed certificate is enough; a paid CA certificate only buys away the SmartScreen prompt. An unsigned, tampered, or differently-keyed download is deleted, and a build with no configured signer refuses every update — never "allow if unconfigured". A release carrying two builds of the *same* flavour is rejected rather than guessed at. Install is transactional: the signature is re-checked after the app exits, the old exe is kept, and a failed or silently-broken new version is rolled back automatically. Your vault, credentials and settings live outside the exe and are never touched by an update. |
| **Cross-device sync** | Copy on your phone, paste on your PC — and the other way round. Text, rich text, code, maths formulas and physics symbols keep their formatting; images travel too. Send any file or folder to one device or several at once, with resume-after-interruption and end-to-end encryption. Several ways to pair — a six-digit code, a QR code, a copyable pairing link you can send over any chat app, picking the device off a nearby list, or typing an address in by hand — about ten seconds either way. The nearby list keeps refreshing while it is open, so a device that comes online a moment later still shows up. Once a pair completes, both screens put their codes away and the code stops being answerable. When the nearby list comes up empty, an active LAN scan finds devices that multicast discovery cannot reach, and a connection check names the step that is failing. Sharing from another Android app goes through the system share sheet: long-press a photo or file, pick Tessera, then choose which paired devices get it. Files are sent to the devices you pick — one or several, chosen explicitly before each transfer — while the clipboard always reaches every paired device. Once paired, a device that reappears on the network reconnects on its own in about a second, with no button to press — and pairing is a one-time act: paired devices re-find each other on a different network or after an IP change, without relying on multicast discovery, which many routers drop. |
| **Interface & appearance** | One design language across every screen — the same page header, section cards and status colours everywhere, so nothing looks bolted on. Eight accent colours, two visual styles (Enhanced Native / Oriental Tech Luxe), system/light/dark, and a Simple or Professional mode that hides or shows the diagnostic depth. The Android companion uses the same palette and the same section layout. |
| **6 languages** | English, 简体中文, Français, Español, Русский, العربية — switchable anywhere, including on the login screen. |

Five ways to unlock, **any one of which is sufficient**: password · email code ·
authenticator app (TOTP) · Windows Hello · one-time recovery key.

Windows Hello here means whichever method you have enrolled — **face, fingerprint or PIN**. The
prompt is the system's own, so a PC with a Hello camera and no fingerprint reader unlocks by face.

The login screen **opens on Windows Hello and prompts for it straight away** — no click needed. On a
machine where Hello was never enrolled for this vault it drops to the password field silently, without
an error, so nothing is lost by the attempt.

---

## Getting started

### Option A — download a release build

Two builds on the [Releases](../../releases) page. Both are the same application; pick
whichever suits how you work.

| | `Tessera.Setup.exe` | `Tessera.exe` |
|---|---|---|
| | **Installer** | **Portable** |
| Install location | you choose it | wherever you put the file |
| Start menu / desktop shortcut | yes | no |
| Shows up in *Apps & features* | yes | no |
| Administrator rights | not needed (installs per-user) | not needed |
| Runs from a USB stick | no | yes |
| Auto-update can roll back a bad version | no | yes |

Neither writes anything outside your own user profile. **Uninstalling does not delete
your vault** — see [Where your data lives](#where-your-data-lives).

> On first run Windows will show *"Windows protected your PC"*. That is SmartScreen
> reacting to a self-signed certificate, not a virus warning. Click **More info → Run
> anyway**. It only happens the first time; in-app updates never trigger it.

Both builds update themselves, each staying on its own kind.

### Option B — run from source

```bash
git clone <this repo>
cd Tessera

# Python side — every runtime dependency is declared in pyproject
pip install -e .

# UI side (node_modules is not committed)
cd modules/file_vault/ui && npm install && cd ../../..

# Launch
python scripts/run.py run file_vault
```

That last command runs `npm run dev` inside `ui/`, which starts Vite plus an Electron
window. The Electron main process spawns `backend_server.py` as a long-lived Python
child and talks to it over NDJSON on stdin/stdout; closing the window shuts it down.

Optional extra — fingerprint unlock needs the Windows Hello bindings:

```bash
pip install winrt-Windows.Security.Credentials.UI winrt-Windows.Foundation
```

---

## First run: the setup wizard

On first launch the wizard walks you through, in order:

1. **Set a password** — this wraps your identity master key.
2. **Bind an authenticator app** — scan a QR code, confirm one code.
3. **Bind fingerprint unlock** — *skipped automatically* if this PC has no Windows Hello.
4. **Configure email** — optional here, addable later in Settings. See below.
5. **Generate a recovery key** — shown once. Write it down.

After that, every launch opens the login screen; pass any one factor and you're in.

---

## Setting up email-code login

This is the "email me a 6-digit code" unlock path. It works by sending mail **from your
own mailbox, through your own SMTP account** — there is no Tessera server in the middle,
which is why you have to supply SMTP credentials.

### Step 1 — get an app-specific password from your mail provider

You almost certainly cannot use your normal login password. Providers require a
dedicated app password (Gmail) or authorization code (QQ, 163) for third-party clients:

| Provider | Where to get the credential |
|---|---|
| **Gmail** | Turn on 2-Step Verification, then Google Account → Security → App passwords. You get a 16-character code; the spaces Google displays are for readability and are **not** part of the password. Google retired "less secure app access" in 2022, so an app password (or OAuth) is the only way. |
| **QQ Mail** | Settings → Account → enable the POP3/IMAP/SMTP service, then generate an 授权码 (authorization code). Use that, not your QQ password. |
| **163 Mail** | Settings → POP3/SMTP/IMAP → enable the service and set an 授权码. Same idea. |
| **Outlook.com / Microsoft 365** | **Not supported.** Microsoft disabled Basic Authentication for SMTP AUTH in 2026; these accounts now require OAuth 2.0 (XOAUTH2), which this app does not implement. Use a different mailbox for the code, or rely on the other four unlock methods. |

### Step 2 — fill in the settings

In the setup wizard's *Email and outgoing mail setup* step, or later via
**Settings → Update email and outgoing mail**:

| Field | Meaning | Typical value |
|---|---|---|
| Recovery email address | Where the code gets sent (can be the same mailbox) | `you@example.com` |
| SMTP server | Your provider's outgoing host | `smtp.gmail.com` · `smtp.qq.com` · `smtp.163.com` |
| SMTP port | `465` for SSL, `587` for STARTTLS | `465` or `587` |
| Sending account | The full address the mail is sent from | `you@gmail.com` |
| App-specific password | The credential from Step 1 — *not* your login password | 16-character app password / 授权码 |
| Use SSL | Check it for port **465**; leave unchecked for **587** (STARTTLS) | — |

> In Settings, leaving the password field blank keeps the currently stored one.

### Step 3 — send a test email

Click **Send test email** and confirm it actually arrives. If it doesn't, the error is
shown verbatim — the usual causes are the wrong port/SSL combination, using the login
password instead of the app password, or SMTP not being enabled on the account yet.

### How it behaves at login

Pick the **Email** tab on the login screen and request a code. The code is 6 digits,
valid for **10 minutes**, usable **once**, and is held only in the backend process's
memory — it is never written to disk.

---

## Setting up the authenticator app

Scan the QR code in the wizard with Google Authenticator, Microsoft Authenticator, Authy,
or any standard TOTP app, then type the current 6-digit code to confirm the binding. The
seed can also be typed in manually if you can't scan. The seed is stored in Windows
Credential Manager, not in the repo.

## Setting up biometric unlock

This calls the real Windows Hello sensor. It needs the two `winrt-*` packages above plus a
PC with Windows Hello configured. If Hello isn't available, the wizard skips this step and
the other four methods are unaffected.

Tessera asks Windows which biometric sensors this machine actually has and picks the first
one available in the order **face → fingerprint → iris**. The tab is labelled and
illustrated to match: a machine with an infrared camera says "Face", one with only a reader
says "Fingerprint". Drawing a face on a machine that has no camera leaves people waiting for
a prompt that is never coming.

**Every place that asks for a password starts the biometric prompt on its own** — the login
screen, the re-authentication dialog that guards Settings, and the checks in front of
individual modules. You do not press anything first. If this machine cannot use biometrics,
or this vault has no biometric binding, the prompt is skipped entirely and the password tab
opens instead: no error box for a feature you never set up, and no Hello prompt that was
guaranteed to fail.

Windows Hello does not let an application choose *which* modality it prompts with — the OS
decides based on what you enrolled. The chain governs what Tessera shows you, which factor it
starts automatically, and what it falls back to.

## Recovery key — read this one

The recovery key is displayed **exactly once**, during setup. Save it somewhere safe and
*not* next to your encrypted files.

If you lose your password, lose the phone with your authenticator, and the machine with
your fingerprint binding dies — the recovery key is the only remaining way in. If that's
gone too, **the files cannot be recovered.** That is the design, not a bug.

---

## Everyday use

- **Encrypting** — pick one or more files, choose an algorithm, press Encrypt. One file
  gives you a Save As dialog; several files ask for a target folder and write
  `<name>.ivault` for each. A failure in one file doesn't abort the rest; you get a
  summary at the end.
- **Deleting the original** — off by default so you can't lose data by accident. But
  encryption is pointless if the plaintext is still sitting next to the ciphertext, so
  tick **securely delete the original file or folder after encrypting** when the point is secrecy. For a folder every file inside is overwritten one by one before the tree is removed, and links are never followed, so nothing outside the folder is touched. That
  delete overwrites with random bytes first and bypasses the Recycle Bin. (Best-effort:
  SSD wear-leveling means this isn't a guarantee against forensic recovery.)
- **Remnant scan** — after encrypting, the app scans for leftover plaintext: editor
  backups (`~$…`, `.bak`, `.wbk`, WPS auto-backup folders) and same-name copies elsewhere
  on disk. Items related to the source file are pre-ticked; disk-wide same-name matches
  are **never** pre-ticked, since one of them may be a file you meant to keep. You can
  re-run this any time against an `.ivault` file.
- **Locking** — closing the window hides to tray. The unlocked master key exists only in
  the backend process's memory and dies with it.

---

## Cleaning up the computer

Five panels under **PC cleanup** in the sidebar. They run at ordinary user permission;
only a few operations ask for administrator rights, and when they do a small helper
process appears, does that one job and closes. Tessera itself never runs elevated —
it holds decrypted key material, and a long-lived elevated process holding that is a
much bigger prize than an ordinary one.

### Checkup

One button scans nine categories at once and shows each as it lands. On a normal
machine the first results are on screen in well under a second; scheduled tasks take
five to nine seconds and arrive last, because a non-elevated process cannot enumerate
them any faster — Windows will not even let it list the folder.

Two things deliberately sit behind their own button rather than in that scan: analysing
the component store, and measuring every top-level folder on the system drive. Each
takes around a minute, and putting them in the one-button checkup turned it into a
two-minute wait. Each button says how long it will take before you press it.

Under **Scan options** you can set how long a file must have gone untouched to count
as junk. The default is one day, which is deliberately cautious — an installation in
progress writes to the temporary folder, and deleting those files can break it. On a
machine that was just restarted, setting it to *Any age* typically finds several times
as much.

### Disk space

- **System drive** — hibernation file, component store, restore points, `Windows.old`,
  and what the page file is using. The page file is shown for information only and
  cannot be selected: turning it off makes programs crash outright when memory runs
  short, instead of slowing down.
- **Large files** — biggest files first, in folders you choose, above a size you choose.
- **Space by folder** — which folders hold the space, largest first, one level at a time.
- **Duplicate files** — compared by size, then by a sample taken from the head and tail
  of each file, then in full. Only files that match all three are called duplicates. In
  each group the copy with the shortest path is kept for you and everything else is
  pre-selected, so a hundred groups do not become a hundred decisions.
- **Drives** — optimisation that reads the media type first. A mechanical drive gets
  defragmented; a solid-state drive gets TRIM. Tessera refuses to defragment an SSD even
  if asked directly: there is nothing to gain and it wears the drive out.

### Startup and menus

Everything that runs when you sign in, gathered from four places — the registry, the
Startup folder, scheduled tasks, and services. Tools that read only the registry report
a handful of entries while the machine still takes a minute to boot; the scheduled-task
list is usually the larger half.

Nothing here is deleted. Registry entries are moved to a backup key, startup shortcuts
are renamed, scheduled tasks are disabled through the scheduler, and services are set to
start manually rather than being disabled — disabling one makes everything that depends
on it fail to start too. Every one of them goes back with one click.

The same panel lists every handler attached to your right-click menu, with the publisher
read from the DLL, and the popup blocker. The blocker works at two levels: it disables
the scheduled tasks and startup entries that produce advertising popups, and it watches
for windows matching a rule and closes them with `WM_CLOSE`. It never terminates a
process, never touches the window you are working in, and logs everything it closed so a
mistake can be traced and whitelisted.

### Installed software

Everything installed on the machine, read from all three registry locations — 64-bit,
32-bit and per-user. Reading only the first misses more than half of what is installed,
and a list missing software you know you have is a list you stop trusting.

Sizes are measured from the installation folder rather than taken from what the installer
declared, because that number is frequently zero or wrong. Uninstalling runs the
program's own uninstaller; Tessera never deletes an installation folder itself, which
would leave services, drivers and registry entries behind.

The registry tab says plainly that cleaning stale references **will not make your PC
faster** — the registry is a tree, and stepping past a few dead entries costs nothing
measurable. It is worth doing for tidiness. The one category that genuinely helps is
component registrations pointing at DLLs that no longer exist, because Windows tries to
load those every time it builds a menu.

### Toolbox

Eighteen small utilities, each with a line saying when you would want it rather than what
it does: flush the DNS cache, see which program holds a port, find out what is locking a
file, restart Explorer, rebuild the icon cache, find empty folders, find paths too long
to copy or back up, rename in bulk with a preview first, and more.

The hosts file is shown read-only. Editing it can redirect any site on the internet, and
that is not a capability an encrypted vault should hold on your behalf.


## Cross-device sync

Your phone, your tablet and this PC share one clipboard and one file drop. Copy a formula
on the phone, paste it into Word on the PC. Drop a 4 GB video on the PC, pick it up on the
tablet. Nothing round-trips through a server: devices talk directly over your local network,
encrypted end to end.

### Pairing a device

Open **Cross-device → Add a device**, then either:

- **Show my code** — a QR code appears. Scan it from the other device.
- **Enter their code** — paste the code the other device is showing.
- **Nearby devices** — a list of unpaired devices on this network. Pick one and pair, no code
  changes hands at all. Names there are marked *unverified*: a device name is whatever its owner
  typed, so the security-code check below is what actually establishes identity.
- **Enter an address** — the fallback when nothing else works, and the most reliable one. The
  tab shows this computer's own address (`192.168.1.7:52140`) to read out, and a box for
  theirs. Enter either `IP:port` or just an IP address: with an IP alone, Tessera asks the
  device's discovery beacon for its current port. Any two devices that can reach each other
  by IP can pair this way: same subnet not required, and it works where multicast is blocked.

**If the nearby list stays empty, press "Scan the network" rather than "Refresh" again.**
Refresh only re-reads what has already been heard, and many routers drop the multicast packets
automatic discovery relies on — in those networks refreshing a hundred times still shows
nothing. Scanning asks every address on the subnet directly and sidesteps the problem.

A yellow banner at the top of the page means Windows Firewall is blocking the sync service, so
other devices cannot see this computer. Press **Allow it** — one elevation prompt, and the rule
covers every Windows network profile. It is limited to Tessera's sync program: it only answers
discovery probes and encrypted QUIC handshakes, and nothing can be read without pairing. Windows
drops blocked packets silently: no error, no log entry, nothing but an empty list, which is why
this is worth surfacing rather than leaving people to guess. **Connection check** below the list
goes through each step — listening, addresses, announcement, reply port, discovery, active scan —
and says which one is failing and what to do about it.

Both screens then display the same six digits and six emoji. **Compare them.** If they
match, confirm on both; if they differ, someone is sitting between you — choose "They don't
match" and the pairing is abandoned. A pairing code works exactly once and expires after a
minute, so a screenshot of an old one is useless to anyone.

When you pair, you choose what kind of device this is:

| | Long-term | Temporary |
|---|---|---|
| Meant for | Your own devices | Someone else's — a friend, a classroom PC |
| Reconnects automatically | Yes | No |
| Clipboard sync | Yes | No |
| Expires | Never, until you revoke it | 10 min / 30 min / 1 hour / 1 day |
| Credentials on disk | Stored, encrypted | Never written down at all |

Revoke any device at any time. Revocation is immediate and final: the old credentials stop
working on the next connection attempt, not at the next restart.

### Copying and pasting

Anything you copy goes to your long-term devices and lands in their system clipboard, ready
to paste. Text, rich text and images travel in the representation each receiving app can use;
formulas and physics symbols survive intact — `E = mc²`, `m/s²`, `θ̇`, `∑∫√`. Only the newest
clipboard item is synchronized, so an old copy cannot arrive late and replace what you copied
next. Windows checks every 200 ms; Android can read the system clipboard only while Tessera is
in the foreground on Android 10 and newer, so after copying in another Android app, open
Tessera once to synchronize it. Incoming content is still written to Android's clipboard.

Large images are not pushed; the other device fetches them only if you actually paste.

### Sending files

Select one or more devices in the list, then **Send files**. You can choose one target or
several explicitly; one file, many files, folders, images, and app packages are labelled in the
transfer list. Each device gets its own independent transfer: a slow phone never holds up a
fast PC, and one device refusing or failing does not cancel the others.

Each active transfer shows its name, progress, speed and estimated time remaining. Either side
can pause, resume or cancel it. A pause keeps the verified blocks already received; interruption
or reconnect resumes from that point instead of starting over. Every file is checksummed on
arrival — a transfer that fails verification is reported as failed, never as "done".

Received executables (`.exe`, `.msi`, `.apk`) are saved as ordinary files. Tessera never
runs or installs anything it receives.

### Staying connected

Devices that drop off — phone locked, laptop asleep, Wi-Fi switched, router rebooted —
come back on their own. There is no "reconnect" button and no need to scan anything again.

Reconnection tries every route at once (last known address, previously seen addresses, and
whatever the local-network discovery currently reports) and keeps whichever answers first.
On a local network this typically completes in well under a second. Because discovery is
live, a device that changed its IP address — new Wi-Fi, fresh DHCP lease, different subnet —
is still found.

### What this build ships

The Windows side is complete and included in this release: the sync service is bundled
inside the exe and starts with the app.

**The Android app is built from the same protocol core**, compiled to a native library
(`tesseracore.aar`) with `gomobile bind`, so pairing, encryption, the reconnect state machine
and resumable transfer are literally the same code that runs on Windows — not a reimplementation
that will drift. The Android side adds only what is genuinely platform-specific: a foreground
service to keep the process alive, a Wi-Fi multicast lock so mDNS discovery keeps working when
the screen is off, and system-clipboard read/write.

Enumerating network interfaces is one of those platform-specific pieces. Android blocks the
kernel call Go's standard library uses for it, so the Android client reads the interface list
through the Java API instead and hands it to the shared core before the node starts, and again
whenever the network changes. Without that, every discovery path — nearby devices, LAN scan,
this device's own address for manual pairing — would come back empty with nothing to explain
why. The connection check under **Nearby devices** reports the interface count first for the
same reason: it is the first thing worth ruling out.

The screen is built from collapsible sections. Anything you are not using right now — the
pairing code, app updates, the app lock, the fault log — folds down to a single title row and
stays folded next time. The paired-device list and the send actions do not fold: they are what
the screen is for. Live metrics are available too — devices online, link latency, current
transfer speed, session totals, listening port, local addresses, interfaces available — read
off the event stream that is already arriving, so nothing extra polls in the background, and
the timer does not run at all while the section is closed.

The Android client covers QR-scan pairing, the security-code check, the device list, clipboard
send/receive, choosing one or more targets for shared files, transfer controls, and choosing
where received files are saved. It can also select and revoke paired devices in bulk. Nearby
paired devices are shown as already paired and online instead of asking to pair them again.
Barcode recognition uses a bundled model rather than the downloadable one, because scan-to-pair
most often happens on a phone that is freshly installed and not yet on a network — exactly when
a download-on-first-use model is unavailable.

The Android app also checks for its own updates against this repository's releases. Automatic
checking is on by default: it looks once when the app opens and prompts only when a newer version
actually exists. Turn it off and nothing is checked until you press **Check for updates**. Only
the *check* is automatic — Android does not let a sideloaded app install a package silently, so
the last step is always the system's own install dialog, which you confirm yourself. Downloads
are accepted from github.com and nowhere else, and the system enforces that the new package
carries the same signing key as the installed one.

It has been compiled and signed but **not** run on physical hardware here, so treat the first
install as a trial.

### Security

Every device holds its own long-term key pair, generated on first run and kept in Windows
DPAPI — never a hardware serial, IMEI or MAC address, because those cannot be revoked.
Being on the same network grants nothing: every connection re-verifies identity against the
key you confirmed when pairing, and a device whose key suddenly changes is refused loudly
rather than accepted quietly.

Each connection negotiates fresh throwaway keys, so recording today's traffic and stealing a
device key later still does not decrypt it. Incoming file paths are checked before anything
is written — `../` escapes, absolute paths and reserved Windows names are rejected outright.

---

### Connecting your devices

On the computer open **Cross-device → Devices & Sync → Generate pairing code**; on the phone
tap **Scan to pair** and point it at the QR code. Both screens then show the same six digits
and six emoji — check they match, confirm on both, done.

Both ends can change where received files are saved, and renaming a device propagates to
every paired device within about ten seconds without reconnecting.

If a scan seems to hang, the usual cause is that the two devices are not on the same network,
or the Wi-Fi has AP isolation turned on (common on company, campus and hotel networks). A phone
hotspot is the quickest way to confirm which it is. Full walkthrough and a troubleshooting
table: [connection guide](modules/file_vault/crossdevice/docs/06-连接指南.md).

### Transfer progress

A progress card appears the moment a transfer starts — file name, target device, percentage,
bytes done over total, one row per file. It refreshes every 250 ms rather than per chunk: a
chunk is 4 MB, and on gigabit that is dozens per second, so per-chunk repaints make the bar
stutter rather than move.

### Diagnostic log

The diagnostic log has its own entry in the navigation bar, and the same log also sits in
Settings — one switch, one set of entries, reachable from either place. It is **off by
default**. Turned on, it records only what
failed — a shortcut that did not fire, a device that would not connect, a service that
would not start. It never records what you were doing: no clipboard text, no file names,
no paths, no keys.

It is deliberately plain text rather than encrypted, because the case that most needs a
log is the one where the vault will not open, and an encrypted log is unreadable exactly
then. That is also why it has to default to off. One button clears the whole thing.

### Guides

- [Desktop guide](docs/电脑端操作指南.md) — every shortcut, pairing, drag-and-drop sending, troubleshooting
- [Android guide](docs/安卓端操作指南.md) — pairing, the share sheet, background survival on Chinese OEM ROMs, app lock
- [Connection reference](modules/file_vault/crossdevice/docs/06-连接指南.md) — what a connection needs and how to diagnose it

### What the connection needs

Both devices must be on the same local network — same Wi-Fi, or a computer on Ethernet behind
the same router. Nothing else is required: no account, no cloud, no port forwarding.

The listening port is not fixed. If the default is already taken, the service falls back to one
the OS assigns; the pairing code and the mDNS record both advertise whichever port is actually
in use, so nothing downstream cares.

A VPN or proxy on the computer is fine — interfaces without multicast (which is what a VPN tunnel
is) are skipped when advertising addresses, so the phone is never handed a tunnel address it
cannot reach.

What the app cannot work around is **AP isolation**: many company, campus and hotel networks
forbid devices on the same Wi-Fi from talking to each other. A phone hotspot is the fastest way
to confirm that is what you are hitting, and also the fastest way around it.


## Security model

Two-layer envelope encryption:

```
                 ┌─ password ─────────┐
                 ├─ email code ───────┤
Identity Master  ├─ TOTP ─────────────┤  each wraps the IMK independently
Key (IMK)   ─────┼─ Windows Hello ────┤
                 └─ recovery key ─────┘
                          │
                          ▼
   per-file key (FK), fresh for every file, encrypts the content in chunks
   FK is then wrapped with the IMK and stored inside the .ivault header
```

- The identity layer (IMK wrapping, key slots) is always AES-256-GCM; your algorithm
  choice only affects file contents.
- **Where quantum computers do and do not matter here.** The vault uses no public-key
  cryptography at all, so Shor's algorithm has nothing to attack, and Grover's leaves
  AES-256 at 128-bit effective strength — out of reach. Two places still warranted
  attention. AES-128 is not offered for new containers, because Grover halves a
  symmetric search and 128 bits becomes 64; files already encrypted with it keep
  opening. And the post-quantum profile derives the file key's wrapping key from
  HKDF(IMK ‖ ML-KEM-1024 shared secret), so an attacker must break both constructions
  rather than either one. The ML-KEM keypair belongs to the identity and is itself
  sealed by the IMK, so every unlock factor reaches it and none has to know it exists.
- The cross-device channel is Noise over X25519 — the one part of Tessera that a
  quantum computer genuinely breaks, and the one where "record now, decrypt later"
  applies, because what travels over it is your files. Once both devices support it,
  the session is upgraded to **X25519 + ML-KEM-768** before any data flows: transport
  keys are derived from the ML-KEM shared secret *and* the Noise handshake hash, so
  they survive a break of either. A device that does not support it simply pairs and
  syncs as before.
- Password and recovery-key slots derive their wrapping key with **scrypt at
  N=2¹⁷, r=8** (the current OWASP minimum): ~134 MB of RAM per guess, which is what
  makes a GPU or ASIC farm impractical — parallel cores are useless if each needs
  its own 134 MB. Each slot records the cost it was sealed with, so raising it later
  never locks an existing vault out; a slot still on an older cost is silently
  re-wrapped the next time it is opened.
- Every salt and every key is generated on the user's own machine
  (`os.urandom`) at setup — **nothing secret is baked into the exe**, so the same
  download in a thousand hands yields a thousand unrelated vaults.
- Clipboard / notes / AI history databases are SQLCipher, keyed by an HKDF derivation of
  the IMK — no second password to remember, but they're unreadable while the vault is
  locked.
- The renderer process has no Node or filesystem access (`contextIsolation: true`,
  `nodeIntegration: false`); it can only call the backend through a narrow preload API.
- **Any one factor unlocks**, so overall strength is the weakest factor. A leaked
  password defeats the other four. Requiring multiple factors simultaneously is not
  supported in this version.

---

## Where your data lives

| What | Where |
|---|---|
| Identity, vault databases, attachments | `%LOCALAPPDATA%\Tessera\data` *(configurable — see below)* |
| TOTP seed, email key, SMTP password | Windows Credential Manager |
| Unlocked master key | Backend process memory only — never on disk |

> **Upgrading from a build named “Ideal1 File Vault”?** Your vault moves itself. On first launch the app looks for the old `%LOCALAPPDATA%\Ideal1 File Vault\` folder and migrates it, using the same transactional copy as any other move: consistent SQLite snapshots, verify-then-atomic-rename, and **the old folder is left untouched**, so a failed migration costs you nothing. A custom location you picked during the old installer is carried over too.

Nothing is uploaded anywhere. The only outbound connections are the ones you configure:
your own SMTP server for login codes, your chosen AI endpoint if you use the assistant,
the speed-test targets, and GitHub Releases for update checks.

Identity data is bound to one machine and does not transfer automatically. `data/` is
gitignored and never committed.


### Choosing where the vault lives

The default is fine for most people. You can put it elsewhere — a drive you actually
back up, an encrypted volume, a second disk — in three ways, in order of precedence:

1. **`IDEAL1_FILE_VAULT_DATA_DIR`** environment variable. Wins over everything, applies
   to that run only, and disables all automatic moving — set it and the app assumes you
   are managing that folder yourself.
2. **The installer.** `Tessera.Setup.exe` asks for a data folder on its own
   page, right after asking where to install the program. The two are separate
   decisions: reinstalling to a different folder is trivial, moving a vault you have
   been filling for six months is not.
3. **Settings → About → Change location.** Pick a new folder and restart. The move
   happens on the next start, *before* anything opens the vault — copying an encrypted
   SQLite database while its connections are open would silently drop whatever is still
   in the write-ahead log. The old folder is left intact, so a failed move costs you
   nothing.

Uninstalling never deletes the data folder.

---

## Development

```
core/       shared infrastructure: config, logging, module registry, subprocess helpers
modules/    pluggable feature modules — one folder = one self-contained scenario
scripts/    CLI entry point: run.py (list / run <module>)
docs/       architecture notes, design specs, release procedure
tests/      smoke tests
```

House rules — the full version is in [`CLAUDE.md`](CLAUDE.md) and
[`modules/README.md`](modules/README.md):

- Python ≥ 3.9. Use `python -X utf8 …` when output is non-ASCII.
- Modules never import each other; they only depend on `core/`. Shared logic gets
  promoted into `core/`.
- Modules may mix in JavaScript or C, invoked through `core/proc.py`'s `run_command()`.

Adding a scenario: copy `modules/_template/` → `modules/<name>/`, edit `MODULE_META`,
implement, then confirm with `python scripts/run.py list`.

```bash
python -m pytest tests/                        # smoke tests
python -m pytest modules/file_vault/tests/     # vault suite
cd modules/file_vault/ui && npm run lint       # UI lint
```

More detail: [`docs/architecture.md`](docs/architecture.md) ·
[`modules/file_vault/README.md`](modules/file_vault/README.md)

---

## Building a release

Builds must run on **Windows x64** — PyInstaller is not a cross-compiler.

```powershell
cd modules\file_vault\ui
npm install
npm run build:standalone
```

That compiles the native C helpers, freezes two PyInstaller targets, then runs
`tsc` + `vite build` + `electron-builder`, producing
`ui/release/<version>/` — both `Tessera.exe` (portable) and
`Tessera.Setup.exe` (installer).

Signing is handled by the build itself: it creates the certificate on first run, pins its public key into `signer-policy.json`, keeps the private key **non-exportable** in your certificate store, and writes a password-protected backup outside the repo. Nothing here needs doing by hand. (`secure-signing-key.cmd` in the repo root can re-protect that backup under a passphrase of your own, if you ever want the backup to survive losing this machine.)

Pushing a `v*` tag runs the same build in CI — see
[`.github/workflows/build-windows.yml`](.github/workflows/build-windows.yml). The
workflow uploads an **unsigned** exe to a **draft** release and stops there. Code signing
stays on your own machine **by design**: a signing key kept in CI secrets can be used by
anyone who can edit a workflow, which would reduce the signature to decoration.

Before publishing your own fork you must fill in two placeholders:

- `REPO` in `modules/file_vault/ui/electron/updateService.ts` → your `owner/repo`

That's the only one you edit by hand. The signing certificate is **created for you** the
first time you run `npm run build:standalone`, and its public-key fingerprint is written
into `signer-policy.json` for you to commit. Every later build re-checks that the
certificate it is about to sign with is still the pinned one, and stops if it isn't —
otherwise a rebuild on a different machine would silently produce a release that every
installed client refuses, with nothing looking wrong until a user hits Update.

**The certificate does not have to cost money.** Embedding the signature inside the exe
and paying a CA are two independent things: Authenticode's format does not care who
issued the certificate, and pinning the public key in the client is in fact *stronger*
than trusting a CA — an attacker would need your private key rather than a
same-named certificate from any of hundreds of public CAs. What money buys is
SmartScreen reputation, i.e. no "Windows protected your PC" prompt on first run.

Leave `SIGNER_POLICY` empty and the app still works — it just refuses to update itself
and says so.

Full procedure, including what happens when the certificate expires and why the release
contains exactly two:
[`docs/standalone-exe-release.md`](docs/standalone-exe-release.md)

---

## License

Tessera is **dual-licensed**. You pick one; you do not need both.

| You are… | Licence | Cost |
|---|---|---|
| Running it for yourself, or inside your company, without redistributing it | **AGPL-3.0** | Free |
| Studying, forking, patching, publishing your fork | **AGPL-3.0** | Free |
| Shipping it (or code derived from it) inside a **closed-source** product | **Commercial** | Paid |
| Offering it to others as a **hosted service** without publishing your source | **Commercial** | Paid |

**Community edition — [GNU AGPL-3.0](LICENSE).** Use it for anything, including
commercially. The one obligation: if you distribute it, a modified version of it, or let
others use it over a network, those users must be able to get the complete corresponding
source under the AGPL too. Personal and internal company use triggers neither.

**Commercial edition — [terms](LICENSE-COMMERCIAL.md).** Same software, without the
source-disclosure obligation, plus warranties and a support channel the AGPL edition
explicitly disclaims. Enquiries: `<your-licensing-email>`.

The decision tree, the contributor terms that make dual licensing possible, and the
answers to the two questions people always get wrong about AGPL are in
[`LICENSING.md`](LICENSING.md).

Third-party components (Electron, React, SQLCipher, the Python `cryptography` library,
tesseract.js and others) keep their own licences — neither licence above changes them,
and nothing here restricts a right you already hold under them. Full inventory:
[`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md), also visible in the app under
**Settings → About**.

The licences cover the code, not the project name or logo. Forks are welcome — call it
something else.
⁢‌‌​​‌‌‌⁠‌‌‌⁡‌​⁡‌​⁡​⁡‌⁡⁡​‌‌‌‌​⁡​⁠‌​‌⁠‌⁡​⁡‌⁠‌​‌​‌⁡‌‌‌⁠‌⁠⁠⁡‌‌⁠​​⁡​‌​⁠⁠⁡‌‌‌⁡‌​‌‌‌⁠​⁠‌​⁠⁠‌‌‌​‌​⁠‌‌​⁡⁠​⁡⁠‌​⁡⁠‌‌​​⁠‌​⁠‌‌​‌⁡‌⁡⁠‌‌​⁠​​⁡​‌​⁠⁡⁡‌⁡‌⁡​⁡​‌‌⁡⁠⁠‌⁡​⁡‌⁡‌‌‌⁡​⁠‌⁡​⁡‌⁡‌‌‌​​⁠‌⁡‌⁡‌⁠⁠​​⁡⁠​‌⁡​​​⁡‌⁠‌‌‌⁠‌⁡‌⁡‌‌⁠​‌⁡​‌‌​⁠​‌⁡‌⁡​⁡​​​⁡​​‌‌‌‌‌⁠⁠‌‌​⁡⁡‌‌‌‌‌​‌⁡‌⁠⁠​‌⁡⁠⁠‌​⁠‌‌‌‌⁠‌⁡‌​‌​​⁠‌⁠⁠‌‌‌​⁡‌⁡⁠⁠‌​‌⁡‌⁠‌‌‌​⁠​‌​⁡⁡​⁡⁠​‌⁠‌⁡‌‌⁠‌‌​⁠⁠‌‌​‌​⁠⁡⁡‌​​‌‌‌‌⁡‌‌‌​‌​⁠⁡‌⁠⁡⁠‌​⁠⁡‌​⁠⁡‌​⁡⁠‌⁡​⁠​⁠⁠⁡​⁠⁠⁡‌‌​​‌⁡⁠​‌⁠⁡⁠‌⁠⁡​‌⁠⁠​‌⁠⁡⁠​⁡‌​‌​⁠⁡‌⁡​⁡‌‌​‌​⁡​‌‌⁡​​‌‌‌⁠‌⁡​‌​⁡​⁡‌⁡‌⁠‌‌​‌‌⁡⁠​‌​⁡⁠‌⁠​⁠‌‌‌⁠‌⁠⁡⁡‌⁠⁠​‌​​⁠‌​⁠‌​⁡​⁠‌​⁡⁡​⁡​​‌⁡​⁡‌​⁠⁡‌⁠‌‌‌⁠‌​‌​​‌‌⁠​⁡‌⁡⁠​‌⁡​‌‌‌​⁠‌​​⁡‌​⁠⁡​⁡​​‌​‌⁡​⁡‌​‌⁠​⁠‌‌​​‌‌⁠⁠‌⁠‌‌‌‌⁠‌‌​⁠⁡‌​​⁠‌​⁠⁡‌⁡‌⁡‌⁡​⁠‌⁠​⁡‌⁠‌​‌⁡‌⁠‌‌​​‌​⁠‌‌⁠‌⁠‌⁡⁠​‌​⁡⁡​⁡​​‌​‌⁡‌⁡​​‌​⁡⁠‌​‌‌‌⁠⁡⁠‌‌⁠​‌‌⁠​‌⁡​⁠​⁡‌⁡‌⁠⁡⁡‌⁠⁠​‌⁠‌⁡‌​⁡⁠‌​‌⁠‌‌‌‌‌⁠⁡‌‌⁠⁡⁡‌​⁠⁡‌⁠‌⁠‌⁠‌⁡‌⁠⁡​‌⁠⁠​​⁡‌⁡‌⁠‌⁡‌⁠​⁠‌​⁠​‌⁡⁠‌​⁠⁠⁡‌​‌​​⁡‌​‌⁡‌⁡‌‌⁠​‌‌‌‌​⁡‌⁡‌​​⁠‌⁠⁡‌‌⁠‌⁠‌​‌⁠​⁡‌​‌⁡​​‌⁠⁠​‌​‌⁡‌‌⁠‌​⁡‌​‌⁡⁠​‌​‌⁡‌‌⁠⁠‌​⁠⁠‌⁡⁠⁠‌‌‌‌‌⁠‌⁠‌⁡​​‌⁠‌‌‌​⁡⁠‌⁠‌⁡‌⁡‌⁠‌⁠‌⁡​⁡​⁡‌‌​⁡‌​​‌​⁠⁡⁡‌⁠⁠⁡‌​‌​‌‌​‌‌⁠‌⁠‌​​⁡‌​​⁠​⁡​⁠‌​‌‌‌​⁡​‌⁠⁡‌‌⁠⁡​‌​⁡⁡‌⁡​‌‌⁡‌‌‌‌‌​​⁡‌⁡​⁡⁠‌‌⁠⁡​‌⁡⁠​​⁠⁠⁡‌​⁡​‌​⁠‌‌‌​⁡‌‌​⁡‌​⁠⁡‌⁠‌⁠‌⁠⁠⁡​⁡‌⁡‌​⁠⁡‌⁠​‌​⁠⁠⁡‌⁠​⁡‌⁠⁠⁠​⁡⁠‌‌⁠​⁠‌‌​‌​⁠⁡⁡‌‌⁠​‌‌‌‌‌​‌‌​⁡⁠​‌⁠⁡⁡​⁡‌⁡‌​⁠​‌‌‌​‌⁠⁠⁡‌‌‌​‌‌‌​​⁡‌​‌‌‌⁡‌⁡⁠⁠‌‌⁠​‌​​⁠​⁡‌​​⁡⁠​‌‌⁠‌‌⁠⁡‌‌​⁡​‌⁡⁠​‌​​‌‌⁠⁠‌‌⁠⁡⁠​⁡​⁡‌⁠​⁡‌​⁡⁠​⁡​⁠‌⁠‌⁠‌⁠⁠⁡‌⁡​​‌⁡​⁡‌‌⁠⁠‌⁠⁡⁡‌⁠⁠​‌⁠⁡​‌⁠⁠‌‌‌​⁡‌‌‌‌​⁡⁠​‌⁠⁡⁡‌⁡‌⁡‌⁠​⁡​⁡​⁠‌⁡⁠‌‌​‌⁠‌⁡‌⁠​⁠⁡⁡‌​‌⁠​⁡​‌​⁡‌⁡‌⁠​‌‌‌‌⁡‌‌‌​‌​⁠⁠‌‌‌⁡‌​⁠​‌⁡‌⁠​⁡⁠​‌⁡⁠⁠‌⁡​‌‌​⁠⁡‌⁠‌⁡‌​⁠⁠​⁠⁡⁡​⁠⁠⁡‌⁠⁡⁡‌⁠‌⁠‌⁠⁠‌‌⁠⁠⁡‌​⁠‌​⁡‌​‌‌​⁡‌​‌​‌⁠⁡⁡‌​⁠​‌⁡‌​‌⁡​⁠​⁡​​‌‌​⁠‌⁡‌⁡‌​⁡⁠‌​‌​‌⁡​⁠‌⁡​​‌⁠​‌‌⁠‌‌‌​⁡‌‌‌‌​‌⁡‌⁡‌​⁡‌‌‌​⁡​⁡‌​‌⁠​⁠​⁡⁠​‌⁡⁠⁠‌⁠​‌‌⁠⁠⁠‌​⁠⁠‌⁠​⁠‌⁠⁠⁠‌​⁠‌‌⁡⁠⁠‌​⁡‌‌​‌⁡‌​​⁠​⁡‌​​⁡​⁡​⁡‌⁠‌​​⁡‌‌​⁠‌​​⁠‌​⁠​‌​⁡‌‌⁡‌​‌⁡​⁠‌​‌‌‌⁡‌‌‌‌⁠‌‌​‌​‌‌​⁡‌​⁡‌‌⁠‌‌‌‌‌⁠‌⁠⁡⁡‌‌​​‌​‌⁡‌​⁡​‌​‌⁡‌⁡​‌​⁡‌⁡​⁡⁠​‌⁠⁠⁠‌‌​‌‌⁡⁠‌‌⁠⁠⁠‌​‌​‌​⁡⁡‌​⁠⁡​⁡‌‌‌‌​⁡‌⁠‌​‌​⁡‌‌⁠‌⁠‌‌​⁡‌‌‌​‌⁠⁡​‌​⁡⁠‌⁠⁠⁡​⁡⁠​‌⁠​⁠‌‌​​‌⁡‌⁠‌⁠⁡⁡‌⁡​⁠‌‌‌‌‌⁠⁡‌​⁠⁠⁡‌⁡​⁡‌⁠‌⁠‌​⁠​‌⁠⁠⁡‌‌⁠‌​⁡​​‌⁠⁠⁡‌⁠​‌‌⁠‌⁠​⁠⁡⁡​⁠⁡⁡‌​‌⁡‌⁠⁠‌‌‌⁠⁠‌⁡‌​‌‌⁠‌‌‌​‌‌​‌⁡‌⁡‌⁡‌⁠‌⁠‌⁡​⁠‌⁡⁠⁠‌⁠​⁠‌‌‌⁠‌⁠⁡‌​⁠⁡⁡‌​‌​​⁠⁡⁡‌⁠‌⁠‌⁡⁠⁠​⁡​‌‌⁠⁠⁠​⁡​‌‌⁠​⁡​⁡‌⁠‌⁡​​​⁡⁠‌‌​⁡‌‌​‌‌‌⁠‌⁠‌⁠⁠‌‌‌‌⁠‌⁡‌‌‌​‌⁡​⁡‌​‌‌​‌​⁡‌‌‌⁠​‌​⁡​​​⁠⁠⁡‌⁠‌⁠​⁠⁠⁡‌‌‌⁡‌⁡​​‌​​⁠‌⁠⁡⁠‌​​⁠‌‌‌⁠‌⁡‌​‌⁠‌⁠‌‌‌​‌⁡‌⁠‌‌⁠​‌​⁡‌‌⁠‌⁠‌⁠⁠​‌⁡‌‌​⁡‌‌​⁡‌​‌⁡⁠⁠‌‌⁠​‌​‌‌‌​⁡​‌⁡​‌‌⁠⁡⁠‌⁠⁡​​⁡​⁠‌⁠‌⁠‌​⁠⁡‌⁠⁡‌‌​⁠‌​⁡‌⁠‌⁠⁡​​⁡‌‌‌‌⁠‌‌⁠⁠⁠‌⁠‌⁠‌⁠‌⁡‌​​⁡‌⁠‌‌‌​‌⁠‌⁡⁠⁠‌⁠⁡⁠‌⁠​⁠‌​​⁡‌​⁠⁡‌⁠⁡‌‌⁠⁡⁠‌⁡⁠‌​⁡⁠‌‌⁠​⁠‌⁠⁠⁠‌‌​⁠‌​​⁡‌⁡​⁡‌⁠​⁡‌⁡⁠‌‌​⁠​‌⁠⁠​‌⁡‌​‌⁠⁡⁠​⁠⁡⁡‌​⁠‌‌⁠‌‌‌⁡​⁠‌​⁠⁠‌⁠⁡​‌⁠​⁡‌‌⁠‌‌⁡​⁠‌‌‌​‌‌⁠⁠‌​​⁡‌‌​‌‌⁠‌⁠‌‌⁠​‌​‌⁡‌​‌‌​⁠⁡⁡‌‌​⁡‌⁡⁠​‌​⁠⁠‌‌‌⁠‌‌​⁠‌‌⁠⁠‌​​⁠‌⁠⁡​‌​‌⁠‌‌‌​​⁡​​‌​‌‌‌⁡​⁡‌⁡‌⁠‌⁠‌​‌‌⁠⁠‌‌⁠⁠‌​⁡​‌​‌⁠‌​‌​‌⁠‌⁡‌​⁠⁡‌⁡​‌‌‌​​‌⁠⁡⁡‌​⁡⁠‌‌​​​⁠⁡⁡‌⁠⁡​‌​‌​​⁡‌⁠‌‌‌‌‌‌​​‌⁡⁠⁠‌​​⁡‌⁠​⁡‌‌​‌​⁠⁠⁡‌‌​‌‌​⁡‌‌‌⁠⁠‌⁠⁡⁠‌⁠⁡⁠‌​‌⁠‌‌‌​‌⁠‌⁠​⁡‌‌​⁡​⁡‌​⁡‌​⁡‌⁠‌‌‌⁠‌⁠‌⁡‌​​⁠‌⁡​‌‌‌‌⁠‌​⁡⁠‌​⁡‌‌​⁡⁠‌⁠‌⁠​⁡⁠​‌⁠⁡​‌‌​​​⁡​⁠​⁡⁠​‌⁠​‌‌​​⁠‌​‌​‌⁠⁡⁡‌​⁡​‌​⁡⁡‌​​⁠‌‌‌‌‌⁡‌⁡​⁡‌⁠‌​⁠​​⁠⁠⁡‌⁠‌⁠‌‌‌⁡‌⁠⁡⁡​⁡⁠‌‌⁡‌​‌⁡⁠​‌⁠⁠⁠‌​⁡⁠‌⁠⁠⁠‌‌​‌​⁡​​‌‌⁠‌‌‌‌⁡‌​⁡⁠​⁡​⁠‌⁠‌​‌⁡​‌‌⁠​⁡‌​​⁠‌‌​‌​⁡⁠​​⁡⁠‌‌⁡⁠‌‌⁠​‌‌⁡‌​‌⁠⁠‌​⁡‌​‌⁡‌​‌​⁡​‌⁡⁠⁠‌⁠⁠​‌⁡‌​​⁡​⁠‌⁠⁠​‌‌​⁡‌​⁡⁠‌⁠‌⁠‌⁡⁠‌‌​​⁡‌⁠‌​‌‌​⁡‌⁡​​‌⁡⁠‌‌⁡‌⁠‌​​⁡‌‌‌⁡‌‌‌⁡‌⁡‌‌‌⁡‌⁡‌⁡​⁠​⁡‌​‌​​‌​⁠⁡⁡‌⁡‌⁡‌⁠⁡‌‌⁠‌​‌​‌‌‌​‌​‌‌​⁠‌⁡​​​⁠⁡⁡​⁡⁠‌​⁡⁠‌‌⁠​⁡‌‌‌⁡‌​⁠⁡‌​​‌​⁡​⁡‌⁡​​‌​⁠‌‌⁡⁠‌​⁠⁠⁡‌​‌​‌‌‌​‌​⁡​​⁡‌​​⁡⁡‌⁢