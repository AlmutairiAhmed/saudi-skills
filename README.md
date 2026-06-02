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

## What the output looks like

You run it on your project:

```text
/pdpl-check
```

Claude searches your codebase and returns a grouped report — every item marked
`✅ Present` / `❌ Missing` / `⚪ Unknown`, with file-and-line evidence and a fix:

````markdown
# Saudi Compliance Review — engineering-level

> ⚠️ Not legal/tax advice. Get licensed Saudi legal + ZATCA accounting sign-off.

## 1. PDPL (SDAIA — Data Protection)
| Item | Status | Evidence | Fix |
|---|---|---|---|
| Privacy notice published & linked | ✅ Present | `components/legal/PrivacyModal.tsx:14` (bilingual AR/EN) | — |
| Consent at signup | ✅ Present | `pages/SignUp.tsx:303` accept-terms checkbox | — |
| Data-subject deletion | ✅ Present | `pages/Settings.tsx` (30-day grace) | — |
| Data export ("download my data") | ⚪ Unknown | only content-export found | Add a true PDPL data export |
| Cookie / analytics consent banner | ❌ Missing | `index.html:22` Matomo loads on first paint | Gate trackers behind a consent banner |
| Cross-border transfer disclosed | ⚪ Verify | data sent to OpenAI/S3 (US) | Confirm privacy notice names sub-processors |

## 2. Consumer Protection
| Item | Status | Evidence | Fix |
|---|---|---|---|
| Pre-charge price disclosure | ⚠️ Partial | tooltips only (`CreditBadge.tsx:28`) | Add a consistent pre-charge confirm |
| Price incl. VAT shown | ✅ Present | `PaymentModal.tsx:652` | — |
| Refund / cancellation / auto-renew | ⚠️ Verify | likely in Terms HTML | Confirm clauses are explicit at point of sale |

## 3. VAT / ZATCA (Fatoorah)
| Item | Status | Evidence | Fix |
|---|---|---|---|
| QR code on invoice | ❌ Missing | `services/email_service.py:228` | Add TLV/base64 QR |
| VAT breakdown line | ❌ Missing | same file | Show taxable + 15% VAT + total separately |
| Seller VAT/CR number | ❌ Missing | same file | Add to invoice header |
| Phase-2 XML + clearance | ❌ Missing | no UBL 2.1 / ZATCA API call | Integrate ZATCA clearance/reporting |

## Top blocking gaps
1. ❌ Invoice is a cosmetic PDF, not a ZATCA invoice — **critical** for a VAT-registered seller.
2. ❌ Analytics track before cookie consent.
3. ⚪ Verify privacy notice discloses US sub-processors.

➡️ Get **licensed Saudi legal + ZATCA accounting sign-off** before launch.
````

*(Findings above are illustrative — your actual report reflects your real files and line numbers.)*

---

## Applicable industries

`pdpl-check` covers **horizontal** (cross-sector) Saudi rules — PDPL, consumer
protection, and ZATCA. The trigger is simple: **if your product handles the
personal data of people in KSA, and/or sells to consumers and issues invoices,
the skill applies.** That covers essentially every consumer-facing digital
product, including:

| Sector | Examples |
|---|---|
| 🛒 E-commerce & retail | online stores, marketplaces, D2C brands, dropshipping |
| 💻 SaaS & B2B software | dashboards, CRMs, dev tools, productivity apps |
| 🍔 Food & delivery | restaurant ordering, grocery, q-commerce |
| 🚗 Mobility & logistics | ride-hailing, fleet, last-mile delivery, shipping |
| ✈️ Travel & hospitality | booking, hotels, tourism, events, ticketing |
| 🏠 Real estate / proptech | listings, rentals, property management |
| 🎓 EdTech | courses, LMS, tutoring, e-learning |
| 🎬 Media & entertainment | streaming, content, gaming, publishing |
| 👔 HR & recruitment | job boards, ATS, payroll, freelancing platforms |
| 🛠️ On-demand & home services | maintenance, beauty, cleaning, bookings |
| 🤝 Nonprofit & membership | charities, associations, subscription clubs |
| 🚜 AgriTech / EnergyTech / IoT | any platform with user accounts + billing |

### Regulated verticals — applicable **plus** sector rules

These still need PDPL + ZATCA, **and** carry extra Saudi regulators this skill
does **not** yet cover (use a specialist + a dedicated skill):

| Sector | Extra regulator / framework |
|---|---|
| 💳 Fintech / payments / wallets | **SAMA** (+ Mada, open banking) |
| 🏥 HealthTech | **MOH / NHIC / SDAIA health-data** rules |
| 🛡️ Insurance (InsurTech) | **SAMA** / Insurance Authority |
| 📡 Telecom | **CITC** |
| 🏛️ GovTech & identity | **Nafath/Absher, Najiz, NCA ECC** cybersecurity controls |

> The skill reviews what's visible **in code & UI**. It can't verify VAT
> registration, contracts, or operational processes (breach response, data
> retention enforcement) — those need a human.

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
