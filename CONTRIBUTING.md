# Contributing to Tessera

Thanks for taking a look. This is a security-sensitive project, so a few things
are stricter here than in a typical repository.

## Before you open a pull request

- **Say what problem it solves, not just what it changes.** A diff that explains
  its own motivation gets reviewed faster than a larger one that doesn't.
- **One concern per pull request.** A formatting sweep mixed into a behaviour
  change is very hard to review, and the behaviour change is what matters.
- **Do not weaken a security boundary for convenience.** Widening an update
  source, relaxing signature checks, disabling `contextIsolation`, or skipping
  the unlock gate on anything that reveals hidden content will be declined even
  if the rest of the change is good. If a boundary is genuinely in the way, open
  an issue about the boundary first.

## Running the desktop app

```bash
cd modules/file_vault/ui
npm install
npm run dev
```

Python 3.9+ is required for the backend, which the dev server starts for you.

## Checks that must pass

```bash
cd modules/file_vault/ui
npx tsc --noEmit     # types
npm run lint         # eslint + all six locales have matching keys
npm run test:update  # the guard suite
```

Go code lives in `modules/file_vault/crossdevice`; run `go test ./core/... ./test/`
from that directory. Note that `./...` from inside `core/` misses the end-to-end
tests one level up.

## About the guard tests

Several tests in `modules/file_vault/ui/tests/` exist to stop a specific defect
from coming back rather than to describe intended behaviour — typography scale,
clickable stat tiles, markdown chunking, README anchors. If one of them fails,
the useful question is usually "what did I change that it is protecting?" rather
than "how do I update the assertion".

If you add a guard, **check it against a known-broken version first**. A guard
that stays green when the thing it protects is removed is worse than no guard,
because it reads as coverage.

## Translations

The interface ships in English, 简体中文, Français, Español, Русский and العربية.
Every user-visible string lives in `modules/file_vault/ui/src/i18n/locales/<lang>/`,
and `npm run lint` fails if the six locales do not have identical key sets. Adding
a string means adding it in all six; a rough translation that is clearly marked is
more useful than a missing key, which renders as the raw key name.

## Reporting a vulnerability

Please do not open a public issue. See [SECURITY.md](SECURITY.md).

## Licence

Contributions are accepted under the AGPL-3.0 licence that covers this repository
(see [LICENSE](LICENSE)). If you need different terms for your own use, see
[LICENSE-COMMERCIAL.md](LICENSE-COMMERCIAL.md).
