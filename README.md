# 🇸🇦 Saudi Skills for Claude Code

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-D97757)](https://claude.com/claude-code)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

A [Claude Code](https://claude.com/claude-code) plugin for teams building software for the **Saudi market**. It teaches Claude your local regulations, so a first-pass compliance review takes seconds instead of a meeting.

> ⚠️ **Not legal or tax advice.** This skill surfaces *engineering-level* risk so a **licensed Saudi attorney** or **ZATCA-experienced accountant** can confirm. It does not replace professional review.

---

## What's inside

The marketplace ships one plugin — **`saudi-compliance-kit`** — with one skill:

| Skill | What it checks |
|---|---|
| 🛡️ **`pdpl-check`** | **PDPL (SDAIA)** data protection — privacy notice, consent, data-subject rights, cookie/analytics consent, cross-border transfer · **Consumer protection** — pre-charge price disclosure, refund/cancellation/auto-renewal terms · **VAT / ZATCA (Fatoorah)** — Phase-2 XML, cryptographic stamp, QR code, mandatory invoice fields, 15% VAT. |

The skill is **stack-agnostic** — it searches your codebase, reports `Present` / `Missing` / `Unknown` with file-and-line evidence, and proposes concrete fixes.

---

## Install

In any Claude Code session:

```text
/plugin marketplace add AlmutairiAhmed/saudi-skills
/plugin install saudi-compliance-kit
```

Then invoke it explicitly, or just describe the task and Claude will trigger it:

```text
/pdpl-check
```

To update later:

```text
/plugin marketplace update saudi-skills
```

---

## Example

> **You:** review my billing code for Saudi compliance
>
> **Claude** *(runs `pdpl-check`)*:
> ❌ **[critical] ZATCA QR code** — Missing · `services/email_service.py:228` · Invoice PDF has no TLV QR code; required for ZATCA.
> ❌ **[high] Cookie consent** — Missing · `index.html` · Analytics load before any consent banner.
> ✅ **Privacy notice** — Present · linked at signup (bilingual AR/EN).
> *Top blocker: invoice is a cosmetic PDF, not a ZATCA invoice. Get licensed legal + accounting sign-off before launch.*

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
        └── pdpl-check/SKILL.md
```

---

## Contributing

Issues and PRs welcome — regulation updates or additional Saudi-market skills (Arabic RTL/i18n, Nafath/SSO, Mada payments, Hijri dates). Keep skills stack-agnostic and evidence-based.

## License

[MIT](./LICENSE) © Ahmed Almutairi

---

<sub>Built with [Claude Code](https://claude.com/claude-code). References: [SDAIA / PDPL](https://sdaia.gov.sa) · [ZATCA / Fatoorah](https://zatca.gov.sa) · [Ministry of Commerce](https://mc.gov.sa).</sub>
