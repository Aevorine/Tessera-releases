# Third-party notices

Tessera bundles and depends on the open-source components listed
below. **Each keeps its own licence.** Neither [`LICENSE`](LICENSE) (AGPL-3.0)
nor [`LICENSE-COMMERCIAL.md`](LICENSE-COMMERCIAL.md) changes them, and nothing
in this project restricts a right you already hold under them.

Regenerate the JavaScript half of this file with:

```bash
cd modules/file_vault/ui && node scripts/gen-third-party-notices.mjs
```

The same inventory is visible inside the app under **Settings → About →
Third-party components**, which is where the promise in `LICENSING.md` is
actually kept.

本软件捆绑并依赖下列开源组件，**各自保留自己的许可证**。本项目的两份许可证都不
影响它们。应用内 **设置 → 关于 → 第三方组件** 可以看到同一份清单。

---

## Runtime platform

| Component | Licence | Notes |
|---|---|---|
| [Electron](https://github.com/electron/electron) | MIT | Bundles Chromium and Node.js — see their own notices below |
| [Chromium](https://www.chromium.org/) | BSD-3-Clause and others | Shipped inside Electron; full notices in the packaged app's `LICENSES.chromium.html` |
| [Node.js](https://nodejs.org/) | MIT | Shipped inside Electron |
| [CPython](https://www.python.org/) | PSF-2.0 | Frozen into `file_vault_backend/` by PyInstaller |
| [PyInstaller](https://pyinstaller.org/) bootloader | GPL-2.0-or-later **with the PyInstaller bootloader exception** | The exception is what permits shipping non-GPL applications frozen with it. Only the bootloader is bundled; PyInstaller itself is a build-time tool. |

## Cryptography and storage

| Component | Licence | Notes |
|---|---|---|
| [`cryptography`](https://github.com/pyca/cryptography) | Apache-2.0 **OR** BSD-3-Clause | AES-256-GCM, ChaCha20-Poly1305, Scrypt, HKDF |
| [OpenSSL](https://www.openssl.org/) | Apache-2.0 | Shipped inside `cryptography`'s binary wheel |
| [SQLCipher](https://www.zetetic.net/sqlcipher/) | BSD-style (Zetetic LLC) | Full-database encryption for notes, clipboard, activity log |
| [SQLite](https://sqlite.org/) | Public domain | SQLCipher's base |
| [`sqlcipher3`](https://github.com/coleifer/sqlcipher3) | Zlib | Python binding for SQLCipher |
| [`keyring`](https://github.com/jaraco/keyring) | MIT | Windows Credential Manager access |
| [`pyotp`](https://github.com/pyauth/pyotp) | MIT | TOTP unlock factor |

## Python runtime dependencies

| Component | Licence |
|---|---|
| [`requests`](https://requests.readthedocs.io/) | Apache-2.0 |
| [`qrcode`](https://github.com/lincolnloop/python-qrcode) | BSD-3-Clause |
| [`Pillow`](https://python-pillow.org/) | MIT-CMU (HPND) |
| [`psutil`](https://github.com/giampaolo/psutil) | BSD-3-Clause |
| [`pywin32`](https://github.com/mhammond/pywin32) | PSF-2.0 |
| [`winrt-Windows.*`](https://github.com/pywinrt/pywinrt) | MIT — optional, Windows Hello only |

## User interface

| Component | Licence |
|---|---|
| [React](https://react.dev/) / [React DOM](https://react.dev/) | MIT |
| [Vite](https://vite.dev/) | MIT |
| [TypeScript](https://www.typescriptlang.org/) | Apache-2.0 |
| [Tailwind CSS](https://tailwindcss.com/) | MIT |
| [Radix UI](https://radix-ui.com/primitives) | MIT |
| [shadcn/ui](https://github.com/shadcn-ui/ui) | MIT |
| [lucide-react](https://lucide.dev/) | ISC |
| [cmdk](https://github.com/pacocoursey/cmdk) | MIT |
| [sonner](https://sonner.emilkowal.ski/) | MIT |
| [next-themes](https://github.com/pacocoursey/next-themes) | MIT |
| [`@tanstack/react-virtual`](https://tanstack.com/virtual) | MIT |
| [KaTeX](https://katex.org) | MIT |
| [`@dnd-kit/*`](https://github.com/clauderic/dnd-kit) | MIT |
| [CodeMirror 6](https://codemirror.net/) / [`@uiw/react-codemirror`](https://uiwjs.github.io/react-codemirror) | MIT |
| [`class-variance-authority`](https://github.com/joe-bell/cva) | Apache-2.0 |
| [`clsx`](https://github.com/lukeed/clsx) / [`tailwind-merge`](https://github.com/dcastil/tailwind-merge) | MIT |
| [`tw-animate-css`](https://github.com/Wombosvideo/tw-animate-css) | MIT |
| [Geist Variable](https://fontsource.org/fonts/geist) | OFL-1.1 |

## Content handling

| Component | Licence | Notes |
|---|---|---|
| [DOMPurify](https://github.com/cure53/DOMPurify) | MPL-2.0 **OR** Apache-2.0 | Sanitises clipboard rich text |
| [markdown-it](https://github.com/markdown-it/markdown-it) | MIT | Note rendering, `html: false` |
| [tesseract.js](https://github.com/naptha/tesseract.js) | Apache-2.0 | OCR, runs fully offline |
| [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) trained data | Apache-2.0 | Bundled language packs |
| [`docx`](https://docx.js.org/) | MIT | Note export |
| [`qrcode`](https://github.com/soldair/node-qrcode) (JS) | MIT | TOTP enrolment QR |
| [i18next](https://www.i18next.com/) / [react-i18next](https://github.com/i18next/react-i18next) | MIT | Six-language UI |

## Cross-device sync (Go)

The cross-device sync service ships as a separate Go executable inside the
installer (`resources/crossdevice/tessera-crossdevice.exe`), and the same code
is compiled into the Android client's `tesseracore.aar`. Its dependencies:

| Component | Licence | Why it is here |
|---|---|---|
| [quic-go](https://github.com/quic-go/quic-go) | MIT | QUIC transport. Connection migration is what lets a transfer survive a Wi-Fi/mobile switch. |
| [flynn/noise](https://github.com/flynn/noise) | BSD-3-Clause | Noise protocol handshake (`Noise_IK` / `Noise_XX`). |
| [zeroconf](https://github.com/grandcat/zeroconf) | MIT | mDNS/DNS-SD, so a device that changed IP is still found. |
| [miekg/dns](https://github.com/miekg/dns) | BSD-3-Clause | DNS wire format, pulled in by zeroconf. |
| [fxamacker/cbor](https://github.com/fxamacker/cbor) | MIT | CBOR encoding for the wire protocol. |
| [blake3](https://lukechampine.com/blake3) | MIT | Content hashing for clipboard dedup and chunk verification. |
| [cenkalti/backoff](https://github.com/cenkalti/backoff) | MIT | Reconnect backoff. |
| [klauspost/cpuid](https://github.com/klauspost/cpuid) | MIT | CPU feature detection, pulled in by blake3. |
| [x448/float16](https://github.com/x448/float16) | MIT | Half-precision floats, pulled in by cbor. |
| `golang.org/x/crypto`, `golang.org/x/sys`, `golang.org/x/text` | BSD-3-Clause | Curve25519/HKDF, syscalls, text encoding. |
| `golang.org/x/net`, `golang.org/x/sync` | BSD-3-Clause | HTTP/2 and IP helpers pulled in by quic-go; `errgroup`/`singleflight`. |
| `golang.org/x/mod`, `golang.org/x/tools` | BSD-3-Clause | Pulled in by gomobile during binding generation. |
| [gomobile](https://golang.org/x/mobile) | BSD-3-Clause | Generates the Android `.aar` binding. Build-time; the generated JNI stubs it emits are distributed. |

The Go standard library and toolchain are BSD-3-Clause.

This table is now also generated: `gen-third-party-notices.mjs` reads
`crossdevice/go.mod` for the module list and versions, and **fails the build**
if a module there has no declared licence. Run it and paste its markdown
output rather than editing the rows by hand.

Regenerate this list from the actual build:

```bash
cd modules/file_vault/crossdevice
go list -deps -f '{{if and .Module (not .Standard)}}{{.Module.Path}} {{.Module.Version}}{{end}}' ./... | sort -u
```

---

## Build-time only — not distributed

electron-builder (MIT), ESLint (MIT), `@typescript-eslint/*` (MIT and
BSD-2-Clause), `@vitejs/plugin-react` (MIT), `vite-plugin-electron` (MIT),
`vite-plugin-electron-renderer` (MIT), pytest (MIT), PyInstaller (GPL-2.0
with exception — see the bootloader note above).

---

## Verifying this list

This file is maintained by hand for the platform and Python halves and
generated for the JavaScript half. If you are relying on it for compliance,
verify against the actual build rather than trusting this table:

```bash
# JavaScript, from the real dependency tree
cd modules/file_vault/ui && npx license-checker-rseidelsohn --production --summary

# Python, from the actual environment the backend is frozen from
pip-licenses --format=markdown
```

Discrepancies are bugs — please report them.

<!-- PVWM3|U2FsdGVkX199xq/OZOH60MiXsFRrayoJ/d4W20f71RCyVnNSR7tjWEcfe2teX9Fad2wQcfunjUGIWLiOV9kptOvnV0X5DFl1As4Irv/w8SihyfFtAVb9K5YvD50/OLxe+uLaJFjLlvQdrC+uO96Eos7iq7jXCCYwsD0HaxmFUGEZvbQsfSuvqqZ9tLlW8z+TArlXvk8ka7ARnICIhjelsa61tmgz5PXLDRlduzvvo+Ta0+XbbwgcS5anfpBabM5em7PrPDEG29V4YwE8rs8Cw/a3NrlgVsMRzk249f8oZzsC7AJ4nOqX8ssHFZ5t9Ve+UnfBw7zCu3cH2wnoDIywHSJm5wsHzwJUQLK2w6Hw2tUdlsR8/hxp8ebyLI3xg88EH4PmtAYOl1Hx2sfmbEbPgpWsbdp2Tw0xOfW+MPYAluT+/mS4QSKvvmiswNvWXRgK7xfT1x/OIv1+tYFH6QgOkTbOXpIA6uv23gBHnlvB9oScIRe9X1hhdljTzTsQVma1IPbc0/Q4b1jisyxsl4IWHL5Ci3/kzwNapLpvoNgErTqw4qVMWrwL6ZZUGClXujAu411eLrAap5cDYjf+QM0xkiInpET37hxGIwt3TwtGlZ7d26wBGS9ixzeGvCpi0HBbKXUCQa/YbubHNSG5qwNi+63Yn2a6RAjjSi8MaOMdiUE= -->
