# Licensing / 许可证说明

Ideal1 File Vault is **dual-licensed**. You choose which of the two licences you
take it under — you do not need both, and you do not need to ask which one applies.

Ideal1 File Vault 采用**双授权**。你自己选择在哪一份许可证下使用它——两份不必都
满足，也不需要来问「我算哪一种」。

---

## The short version / 一句话版本

| You are… | Licence | Cost |
|---|---|---|
| Running the app for yourself, or inside your company, without redistributing it | **AGPL-3.0** | Free |
| Studying, forking, patching, publishing your fork | **AGPL-3.0** | Free |
| Shipping it (or code derived from it) inside a **closed-source** product | **Commercial** | Paid |
| Offering it to third parties as a **hosted / network service** without publishing your source | **Commercial** | Paid |

| 你的情况 | 许可证 | 费用 |
|---|---|---|
| 自己用，或公司内部用，不对外分发 | **AGPL-3.0** | 免费 |
| 阅读、fork、改代码、公开发布你的 fork | **AGPL-3.0** | 免费 |
| 把它（或它派生出的代码）放进**闭源**产品里分发 | **商业许可证** | 付费 |
| 作为**在线服务**提供给第三方，且不公开你的源码 | **商业许可证** | 付费 |

---

## Option 1 — GNU AGPL-3.0 (community edition)

Full text: [`LICENSE`](LICENSE) · SPDX identifier: `AGPL-3.0-only`

This is the default. Anyone can use it, for anything, including commercially,
with one obligation: **if you distribute the software or a modified version of
it — or let others use it over a network — you must make the complete
corresponding source available to those users under the AGPL too.**

The network clause is section 13, and it is the difference between AGPL and
GPL. It matters here because a vault is exactly the kind of thing someone might
be tempted to wrap in a web service. If you do that, your users get the source.

### What AGPL does *not* require

Two misreadings come up often enough to be worth stating plainly:

- **Personal and internal company use is unrestricted.** AGPL obligations are
  triggered by *conveying* the software to someone else, or by letting someone
  else *interact with it over a network*. Installing it on your own machines —
  or on 500 machines inside your own organisation — triggers neither.
- **Your own unrelated software is not affected.** The copyleft reaches
  derivative works of *this* code. A separate program that happens to sit on the
  same disk, or that talks to it over a documented interface, is not a
  derivative work.

If you are unsure whether your case is covered, the honest answer is that this
is a legal question about your specific facts, and this file is not legal
advice. Ask a lawyer, or ask us — see below.

---

## Option 2 — Commercial licence

Full terms: [`LICENSE-COMMERCIAL.md`](LICENSE-COMMERCIAL.md) · SPDX identifier:
`LicenseRef-Ideal1-Commercial`

Buy this if the AGPL's source-disclosure obligation is not something you can
live with. It grants the same software under terms that **do not require you to
publish your source**, plus written warranties and a support channel that the
AGPL version explicitly disclaims.

Typical buyers:

- An ISV embedding the vault or its crypto container in a closed-source product
- A company offering a hosted variant to its own customers
- Anyone whose procurement or compliance process cannot accept a copyleft licence

To enquire: **`<your-licensing-email>`** — include what you are building, how it
is distributed, and roughly how many end users. A one-paragraph description is
enough to get a quote.

---

## Contributing

Contributions are accepted under the AGPL-3.0. By opening a pull request you
confirm that you have the right to submit the code and that you licence it to
the project under AGPL-3.0, **and** that you grant the maintainer permission to
also distribute your contribution under the commercial licence above.

That second clause is what makes dual licensing possible at all: without it,
every outside contribution would make the commercial edition undistributable.
It is the standard arrangement for dual-licensed projects (Qt, MySQL, and
others use the same shape). If you are not comfortable granting it, say so in
the PR — a patch can still be discussed and reimplemented independently.

贡献代码即表示你同意上述两点。第二条是双授权能成立的前提：没有它，任何一个外部
贡献都会让商业版无法分发。这是双授权项目的通行做法（Qt、MySQL 等同理）。如果你
不接受，请在 PR 里说明——补丁仍然可以讨论，只是需要独立重新实现。

---

## Third-party components

Neither licence above changes anything about the third-party open-source
components this software bundles. Each keeps its own licence, and nothing here
restricts a right you already hold under them.

The full inventory is in [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md),
and the same list is reachable in the app under **Settings → About**.

上面两份许可证都不影响本软件捆绑的第三方开源组件——它们各自保留自己的许可证。
完整清单见 [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md)，应用内
**设置 → 关于** 也能看到同一份。

---

## Trademark

The licences cover the code. They do not grant rights to the project name or
its logo. A fork is welcome — call it something else.

许可证覆盖的是代码，不包括项目名称和图标。欢迎 fork，但请换个名字。

<!-- PVWM3|U2FsdGVkX18dOdi+HKqG6lHXBuBRXM0HX6phDrv8wFt1LL1B1FxSqfNIY1d/jU4a1IOtNB9Gf3iUwgh33Y2ngIHvDoEEpP4exL0SZ2nJKtrkwzQZAHSJnLefFqzdRTl/rLcffcCOErzl7ydQOLWtj2rS2k319r0ThtCMyclvZto3yHsUaFFhaPBuxONjj+NIQjCn8duC+sFbiswhBNWHTbm82JxAvy5bnquHUMSNfjBJSUZgmXYgqkQ9Ydw5YW8Bhnefz6mEArt7V7r9Q+iuQ5V1P1F7wb8irWni340V7ck5Uzs8vZIZb2HlZTR0bXEnx4baDxOJJ5aTzhcGrjJjLJG7jRJhQq4P6YIU2nZ6T3/blqXXNCNxd6oL2Idh+15uQ/VbWGSZDRYUjb+yi/Py4pnYbbOMk6gwIQIBFJLAjABS4AaogFOfPiuypzRIWMXFgVyqK3rgYSe53/pmn9qpHiAbwBdHWwVik24LWNDSNCsCocDPGdt69oEiXHYf6a1yfd6ZisOHiRlepb19J02y0albEhBKaxLuh5PWTzODTiauyWpdOrMz/CZ8tZOulEyP3MVTixwXe3fodW+tmHz/ybqPfUX6kTwGsRDZ3zPzP7+l7+dtmMb0YiWogde6URoaA//chpVTTZKkWTGYTlnZulV43muUvLxWCPiAlhcbS1k= -->
