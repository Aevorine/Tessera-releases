# Contributing

This repository holds **release builds**, not source. That shapes what helping looks like
here — there is no pull request to send, but four of the five most useful things you can do
happen in this repo anyway.

## Report a bug

[Open an issue](../../issues/new/choose). The template asks for the version, how you installed
it, and what you expected — those three answers are what turn "the cleaner is broken" into
something fixable.

**Never paste vault contents, passwords, recovery keys or TOTP seeds into an issue.** Issues
are public and permanent. If a problem cannot be described without them, report it privately
through [Security advisories](../../security/advisories/new) instead — see
[SECURITY.md](SECURITY.md).

One thing worth knowing before you file: the diagnostic log inside the app
(**Settings → Diagnostics → Copy report**) collects the version, the OS build and the recent
errors, and it redacts paths under your user folder. Pasting that is usually faster and more
accurate than describing it from memory.

## Report a false positive in the cleaner

This is the single most valuable report we get, and it deserves its own heading.

If Tessera offered to remove something it should not have — a folder you pinned, a setting you
configured, an app you use — say so. Include what the item was labelled, which category it was
under, and what you lost. Every such report becomes a permanent guard in the test suite, so it
cannot come back.

The reverse is also worth reporting: something obviously safe that Tessera refuses to clean, or
labels as risky. Being over-cautious is the safer failure, but it is still a failure.

## Improve a translation

Tessera ships in English, 简体中文, Français, Español, Русский and العربية. Translations are
done by machine and reviewed by hand, which means the wording in your language is probably
serviceable rather than good.

If a string reads awkwardly, open an issue with the screen it is on, what it currently says,
and what it should say. Do not worry about finding the key — a screenshot of the sentence is
enough.

## Tell us what a report should have told you

If a number in the app was true but useless, that is a bug in this project's terms. "Freed
6.4 GB" when the drive gained 200 MB, a category that says "nothing found" when it was actually
blocked by permissions, a warning that does not say which program to close — these are the
things the app is supposed to get right, and every one of them was found by someone saying it
looked wrong.

## Ask a question

Setup, pairing, choosing an encryption profile, moving the vault to another drive — use
[Discussions](../../discussions). Questions asked there stay searchable for the next person,
which an email cannot do.

## What happens to source contributions

The source is not currently public, so there is no pull request path. Tessera is
AGPL-3.0-licensed, and the licence obligations that come with distributing it apply the same
way — see [LICENSING.md](LICENSING.md).
