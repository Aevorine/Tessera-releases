# Security Policy

## Reporting a vulnerability

**Please do not open a public issue for a security problem.**

Use GitHub's private reporting instead: go to the repository's **Security** tab →
**Report a vulnerability**. That opens a private thread visible only to the maintainer.

If private reporting is unavailable to you, open a normal issue that says only *"security
issue, please provide a private channel"* — with no details — and wait for a reply.

What helps most, in order:

1. What an attacker gains (read the vault? bypass unlock? run code?)
2. The smallest reproduction you have, ideally with the app version and Windows build
3. Whether it needs local access, an already-unlocked vault, or neither

## Supported versions

Only the latest release. This is a one-person project; backporting fixes to older
versions is not something it can honestly promise.

## Scope

In scope, roughly ordered by how much they matter:

- Anything that reads vault contents without the correct unlock factor
- Anything that lets a malicious file (a note, a clipboard entry, an imported CSV,
  an `.ivault`) run code or exfiltrate data
- Anything that gets a modified build accepted by the auto-updater
- Weaknesses in the key derivation or envelope encryption

Out of scope:

- Attacks that assume the attacker already runs code as your Windows user. At that point
  they can read the vault while it is unlocked, keylog the password, or replace the app
  outright — no application-level defence survives that, and pretending otherwise would
  be dishonest.
- SmartScreen warning on first run. That is expected: the build is signed with a
  self-signed certificate. See the README.
- The absence of anti-rollback protection in the updater. It is a known and documented
  trade-off, written up in `docs/standalone-exe-release.md`.

## Known limitations

These are documented rather than hidden, and are not treated as vulnerabilities:

- **Any one factor unlocks the vault.** Overall strength equals the weakest factor; a
  leaked password defeats the other four. Requiring several factors at once is not
  supported.
- **No anti-freeze in the update channel.** Someone controlling your view of GitHub can
  keep replaying an older, genuinely-signed release. Signature checks all pass, because
  the release really was signed by us.
- **Electron version.** See `docs/security-audit-2026-07.md` for the current state and
  the plan.

## What this project does not claim

It has not been independently audited. It is written by one person. If you are
protecting something whose loss would be serious, treat it accordingly — keep your own
backups, and do not rely on any single tool.
⁢‌‌​​‌‌‌⁠‌‌‌⁡‌​⁡‌​⁡​⁡‌⁡⁡​‌‌‌‌​⁡​⁠‌​‌⁠‌⁡​⁡‌⁠‌​‌​‌⁡‌‌‌⁠‌⁠⁠⁡‌‌⁠​​⁡​‌​⁡⁠‌‌⁡​‌‌​⁡⁡‌​⁠‌​⁡‌‌‌⁠⁡⁠‌⁠⁡​‌⁠​⁠​⁡​‌‌⁠⁠​‌‌⁠​​⁡​⁠‌​‌⁡‌⁡‌​‌‌​⁡‌⁠⁠​‌⁡​​‌‌​⁠​⁡​​‌⁡​​‌‌​⁡‌⁡‌⁡‌⁠⁠⁡‌⁡‌‌‌‌​‌‌⁠⁡⁡‌⁡​⁡​⁠⁠⁡​⁡‌‌​⁡​​​⁡​​​⁡⁠‌‌⁠⁠⁠‌‌​‌‌‌‌⁠‌​‌​​⁡⁠‌‌⁡⁠​‌‌​‌‌​⁠⁡‌‌⁠‌‌‌‌​​⁡‌​‌​⁠⁠‌⁡⁠‌​⁡‌​‌​‌‌‌⁠​‌‌​​⁠​⁡‌‌​⁡‌​​⁡​⁠‌⁠⁡‌‌⁠​‌‌⁠⁡‌‌​⁠⁡‌​⁠​‌​⁠‌‌⁡⁠​‌⁠⁠‌‌⁠⁠⁡‌​⁠‌‌⁡​‌‌⁠⁠​‌⁡⁠⁠‌⁠⁡‌‌⁡​‌‌⁠⁡⁡‌⁠‌⁡‌⁡​⁡‌⁠⁡⁠‌​⁡‌​⁡‌‌‌⁡⁠‌‌​⁠⁡‌‌​‌‌⁠⁡⁠‌​‌‌‌​‌⁡‌​‌⁠‌⁡​​‌‌‌​‌​⁠​‌⁡​⁡‌‌​​‌​⁡​‌⁡‌⁠‌⁠‌⁡‌​‌​‌​​⁠​⁡⁠‌​⁡⁠‌‌⁡‌‌‌⁠‌⁡‌‌‌​‌‌‌⁡‌⁡​⁠‌⁠⁡‌‌⁠​‌​⁡⁠‌​⁡​⁠​⁠⁡⁡‌⁠⁠‌‌⁡​​‌⁠​⁠‌‌​​‌⁡⁠​​⁡‌​​⁡​​​⁠⁡⁡​⁡‌⁠​⁠⁡⁡‌‌‌​‌​⁠⁡‌⁠⁠‌‌⁡‌‌‌⁡​‌‌​‌⁠‌⁠⁡​‌⁠‌⁡‌⁡​⁠‌​​‌‌​​⁡‌⁠⁡‌‌​⁠​‌​‌⁠‌​‌​‌​‌⁠‌⁡⁠⁠​⁡​‌‌​‌⁠‌​​⁠‌⁡‌⁠‌⁡⁠‌‌‌​‌‌​‌⁠​⁠⁠⁡‌⁠⁠⁠‌‌⁠‌‌⁠⁠⁠‌⁠⁠⁠‌⁠⁡⁠‌​⁡​​⁡‌​​⁡‌‌‌​⁠‌‌⁡‌‌‌⁠‌⁠​⁡‌⁡‌‌‌‌‌⁡​⁡‌‌⁠​‌⁠⁡‌​⁡‌​‌‌​⁠‌​⁠⁠‌⁡‌⁠​⁡‌⁡‌⁡‌‌​⁡​⁠‌​​⁡‌​‌⁠‌‌‌​‌⁠‌⁡‌​‌‌‌​​‌‌⁠⁡​‌⁡​⁠‌⁡‌⁠‌‌⁠‌‌‌⁠⁠​⁡‌⁡‌⁠⁡‌‌​⁡‌​⁡⁠​‌‌​‌‌⁠⁠⁠‌⁡​​‌⁡⁠⁠‌​⁡⁡​⁡​⁡‌⁡‌⁡‌⁠⁡⁠‌⁠⁡​​⁡​‌‌‌⁠​‌​⁡‌‌⁠​⁡‌‌‌⁡‌​‌‌‌​⁠​​⁡​⁡‌⁠​‌‌⁡‌⁡‌⁡‌‌‌⁡‌⁠‌​⁠​‌‌⁠‌​⁡​⁠‌⁡‌⁠‌‌⁠‌‌⁠​⁠‌⁠‌​‌⁡⁠⁠‌‌⁠​​⁡⁠‌‌⁡​⁡‌​‌​‌‌⁠‌​⁡​⁠‌⁠⁡⁡​⁡​‌‌‌‌⁡​⁡⁠‌​⁠⁡⁡‌⁡​⁡‌⁠⁡‌​⁡​‌‌‌​⁠‌⁠‌​‌​⁠⁡‌⁠⁠⁠‌⁠⁡‌‌​‌‌‌⁡‌‌​⁡​⁡‌​​⁡‌⁠‌⁠‌⁠⁡​​⁡​‌‌‌⁠‌‌⁡‌⁠​⁡⁠​‌⁡⁠​‌⁠⁡‌‌‌​‌​⁡‌⁡‌​‌⁡​⁠⁡⁡‌‌‌⁠‌​⁠⁡‌⁡‌‌‌​⁠​​⁡​⁠‌⁠⁠‌‌‌‌⁡‌⁠⁡​‌⁠⁠⁠‌‌​⁡‌​⁠⁠‌⁠​⁡‌⁠⁡​‌⁡​⁠‌⁠​⁡‌‌⁠​‌​​⁠‌⁡​​‌⁠⁠​‌⁠​⁠‌​​⁠‌⁠⁡​‌⁠​‌​⁡⁠​‌⁡​⁡​⁡⁠​​⁡​⁠‌‌⁠​​⁡⁠​‌‌​⁡​⁠⁠⁡​⁡‌⁡‌⁡⁠⁠‌⁠‌‌‌​‌⁡‌⁠​‌‌‌⁠​​⁡‌⁠‌‌‌⁠‌⁡⁠​‌‌​⁠​⁡​⁡‌⁡‌​‌⁡​⁡‌⁠​‌‌⁠‌⁠‌⁡‌⁠​⁡​⁡‌⁡​‌‌​‌⁡​⁡​​‌⁠‌⁡‌⁡​‌​⁠⁡⁡‌‌⁠‌‌⁠⁡⁡‌⁡‌⁠‌⁠​⁠‌⁡​‌‌‌​⁠‌‌​​‌⁡‌‌‌​‌​‌⁠⁡⁡‌​⁠⁡‌‌​‌‌‌​⁡‌⁡‌​‌​‌​‌​​‌‌​​‌‌⁡‌⁠​⁡‌‌‌​​⁡​⁡‌⁠‌​⁡​‌​⁡⁡‌⁡‌‌‌⁠‌⁠‌⁠‌⁠‌⁠‌⁠‌⁠⁠⁡‌‌⁠⁠​⁡‌⁠‌​⁡⁠‌⁠⁡​‌⁡‌‌‌​⁠⁡​⁡‌‌​⁡​​​⁡‌​‌​‌⁡‌⁠⁡⁡‌⁡⁠​‌⁡⁠⁠​⁠⁡⁡‌⁡​‌‌⁠⁠​‌‌⁠​‌⁠‌‌​⁡​⁡‌⁠​‌‌⁡‌⁡‌​⁡⁡‌​‌​​⁡‌‌‌⁠⁡‌‌⁡​​‌‌⁠⁠‌​​‌‌⁡‌​‌‌​​‌​‌⁡‌‌‌⁠‌⁠‌​‌‌​⁡‌⁡⁠⁠‌‌‌⁡‌‌​⁡‌⁠⁡‌‌⁠‌⁡‌⁠⁠⁡‌⁠⁡​​⁡​‌‌‌⁠⁠‌⁡‌​‌‌​⁡‌​​⁠‌​⁠‌​⁡‌​‌⁠‌‌‌​‌⁠​⁡⁠​‌​⁡⁡‌‌‌⁡‌​⁠‌​⁡⁠‌‌⁠‌‌‌⁠‌​‌⁠​‌‌‌​​‌‌⁠‌‌⁠‌⁠‌​⁡‌‌⁠‌⁡‌⁡‌⁠​⁡‌​‌​⁠⁠‌⁠⁠⁡‌⁠‌‌‌​⁠⁡‌​‌​‌​‌‌‌​‌⁠‌‌‌​‌⁡​‌‌⁡⁠​‌⁠⁠​‌‌‌​‌⁠​⁡‌⁡​‌‌​⁠⁡‌‌​⁡‌​⁡⁠‌‌⁠​‌‌​⁡‌⁠⁠⁡​⁡⁠‌‌​​⁡​⁡⁠‌​⁡⁠​‌⁡‌​‌⁠⁡⁡‌​⁡​​⁡​⁡​⁡‌​‌⁡⁠‌‌⁠​‌‌⁡​⁠‌⁠⁠‌‌⁠‌​‌​‌‌​⁡‌‌‌​‌⁡​⁡​⁠​⁡‌‌​⁡​⁠​⁠⁠⁡‌⁡​⁠‌⁠⁡⁡‌⁡​⁠‌​‌​‌⁠⁡‌‌⁠⁡‌‌‌‌‌‌⁡⁠⁠‌​​‌‌⁠​‌‌‌​⁠‌‌‌​‌‌​⁡‌​‌​‌⁠​‌‌⁠⁡​​⁡‌⁡​⁡‌⁠‌⁠​⁡​⁡​⁡​⁡⁠​‌‌⁠⁠‌‌‌​​⁡​⁠‌​⁠⁠‌‌⁠⁠‌⁡‌⁠‌⁡​⁠‌⁠⁡⁠‌‌‌‌‌​⁡‌​⁡​⁠‌‌​⁡‌⁡⁠⁠‌⁠‌⁠‌⁡​​‌‌​⁠‌​‌⁡​⁠⁠⁡​⁡‌⁡‌‌​⁠‌​⁡⁡‌‌‌‌‌⁠⁠‌​⁡​‌​⁡⁠​​⁡​‌‌⁡⁠​‌⁠‌⁡‌‌​​‌⁡​‌‌⁠​‌‌⁠⁡‌‌⁡⁠‌​⁠⁡⁡‌‌‌‌‌‌​‌‌⁡‌⁡‌‌​⁡‌​⁠​‌⁡‌⁠‌​​⁠‌⁡‌‌‌⁠⁠​‌⁠‌⁡‌⁠⁠‌‌​‌⁠‌⁡⁠‌‌​⁠‌‌​⁠‌‌⁡​⁡‌​⁡​‌​⁠‌​⁠⁠⁡‌‌​⁠​⁡​⁡‌⁠‌‌‌​⁠​‌⁡​⁠‌⁠‌‌‌⁡⁠​‌​​⁠​⁠⁡⁡‌‌​​​⁡​⁡‌​‌‌​⁡‌​​⁡⁠‌‌​​⁠‌‌​⁡​⁡‌⁡‌​⁡‌‌⁠⁠​‌⁠​‌‌⁡‌‌‌​⁠⁡‌⁡‌⁡‌⁠​⁡​⁡⁠​​⁡​⁡‌⁠⁠⁡‌​⁠⁠‌⁠⁡⁡‌⁠⁡⁠‌⁡​⁠​⁡​​‌​​‌‌⁡​​‌​⁡​‌‌​⁠‌⁠⁠​‌​‌⁠‌‌‌⁠​⁡‌⁡‌⁠‌⁠​⁡‌⁠‌‌⁠‌‌‌​⁡​⁡⁠‌‌‌‌⁠‌⁠​‌‌‌​⁡‌⁡‌‌‌​‌⁡‌⁡‌⁡​⁡‌​‌⁡​⁡​⁡​​​⁡​⁡‌‌​⁡‌​⁠⁡‌⁠​⁠‌⁠⁡‌‌​​⁠‌⁠​⁡‌​​‌‌⁠​‌​⁠⁡⁡​⁠⁠⁡‌​⁡‌‌⁠⁠⁡‌‌‌⁠‌⁡⁠‌‌⁠⁠‌​⁠⁡⁡​⁠⁡⁡‌⁠⁡​‌⁠⁡​‌‌​​​⁡​⁠​⁠⁠⁡‌⁠⁠⁡‌⁠⁡⁠​⁡​⁡‌​‌⁡‌​‌⁠‌​​⁠​⁡‌⁠‌​⁡​​⁡​⁡‌​⁠⁡‌​⁠​​⁡⁠​‌​​‌‌​⁡⁠‌⁠‌⁠​⁠⁡⁡‌⁡‌​‌⁠⁠⁠‌⁡‌⁡‌⁠‌⁡‌⁠⁠​‌⁠​⁠‌​​⁠‌​⁠⁠‌​‌​‌⁠⁠⁡‌​​⁠‌⁠⁠⁠​⁡⁠‌‌⁠⁡​‌‌‌⁠‌‌​⁠‌​‌⁠‌⁠⁡​​⁡‌⁡‌⁠​⁡‌‌⁠⁠‌⁠‌⁠‌⁠​⁡‌‌‌⁠​⁠⁠⁡‌⁡‌⁠‌​​‌​⁠⁠⁡​⁡‌​‌⁡​​​⁡​‌‌⁠⁡​‌‌⁠‌‌⁡​⁠‌​​‌​⁡‌⁠‌​⁠⁡‌⁠‌⁠‌​‌⁠‌⁠⁡⁡‌‌​⁡‌⁠⁡‌‌⁠‌‌‌⁡​⁡‌‌⁠‌‌​⁠⁠‌‌​⁡‌​⁡​‌⁡​⁡‌‌⁠​‌⁠‌⁡‌⁠⁡​‌‌​⁠‌⁡⁠⁠‌​‌⁡‌⁠⁠⁡‌‌⁠​​⁠⁠⁡‌⁡​⁡​⁡⁠‌‌​‌‌‌⁡​⁠‌⁠‌​​⁡​​‌​‌⁠‌⁠⁡​‌⁡‌‌‌​⁠‌‌​‌⁠‌‌‌⁡​⁡​⁡‌‌⁠‌‌⁠​⁠‌⁠‌⁡‌⁠⁡‌‌​⁠⁡‌​⁡‌‌⁠⁠‌‌​⁡‌​⁡⁡‌⁢