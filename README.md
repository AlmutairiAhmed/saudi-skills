# 🇸🇦 Saudi Skills for Claude Code

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-D97757)](https://claude.com/claude-code)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Skills](https://img.shields.io/badge/skills-3-success)](#whats-inside)

A [Claude Code](https://claude.com/claude-code) plugin marketplace for teams building software for the **Saudi market**. It teaches Claude your local regulations and localization standards, so a first-pass compliance and Arabic/RTL review takes seconds instead of a meeting.

> ⚠️ **Not legal or tax advice.** These skills surface *engineering-level* risk so a **licensed Saudi attorney** or **ZATCA-experienced accountant** can confirm. They do not replace professional review.

---

## What's inside

The marketplace ships one plugin — **`saudi-compliance-kit`** — with three skills:

| Skill | What it checks |
|---|---|
| 🛡️ **`pdpl-privacy-check`** | PDPL (SDAIA) data protection + consumer protection — privacy notice, consent, data-subject rights, cookie/analytics consent, cross-border transfer, pre-charge price disclosure. |
| 🧾 **`zatca-einvoice-check`** | ZATCA (Fatoorah) e-invoicing + VAT — Phase-2 XML, cryptographic stamp, QR code, mandatory invoice fields, 15% VAT, SAR formatting. |
| 🌐 **`arabic-rtl-i18n`** | Arabic localization & RTL — translation key parity, English-leaking strings, `dir`/`lang`, mirrored layouts & column order, bidi numerics, per-locale contrast. |

Each skill is **stack-agnostic** — it searches your codebase, reports `Present` / `Missing` / `Unknown` with file-and-line evidence, and proposes concrete fixes.

---

## Install

In any Claude Code session:

```text
/plugin marketplace add AlmutairiAhmed/saudi-skills
/plugin install saudi-compliance-kit
```

Then invoke a skill explicitly, or just describe the task and Claude will trigger the right one:

```text
/pdpl-privacy-check
/zatca-einvoice-check
/arabic-rtl-i18n
```

To update later:

```text
/plugin marketplace update saudi-skills
```

---

## Example

> **You:** review my billing code for Saudi compliance
>
> **Claude** *(runs `zatca-einvoice-check`)*:
> ❌ **[critical] QR code** — Missing · `services/email_service.py:228` · Invoice PDF has no TLV QR code; required for ZATCA.
> ❌ **[high] VAT breakdown** — Missing · same file · Shows total only, no separate 15% VAT line.
> ✅ **SAR formatting** — Present · halala→SAR conversion correct.
> *Top blocker: this is a cosmetic PDF, not a ZATCA invoice. Confirm VAT registration + Phase-2 onboarding with an accountant.*

---

## Repository layout

```text
saudi-skills/
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest
└── saudi-compliance-kit/         # the plugin
    ├── .claude-plugin/
    │   └── plugin.json           # plugin manifest
    └── skills/
        ├── pdpl-privacy-check/SKILL.md
        ├── zatca-einvoice-check/SKILL.md
        └── arabic-rtl-i18n/SKILL.md
```

---

## Contributing

Issues and PRs welcome — especially regulation updates, new locales, or additional Saudi-market skills (Najiz, Nafath/SSO, Mada payments, Saudi address format, Hijri dates). Keep skills stack-agnostic and evidence-based.

## License

[MIT](./LICENSE) © Ahmed Almutairi

---

<sub>Built with [Claude Code](https://claude.com/claude-code). Regulatory references: [SDAIA / PDPL](https://sdaia.gov.sa) · [ZATCA / Fatoorah](https://zatca.gov.sa) · [Ministry of Commerce](https://mc.gov.sa).</sub>
