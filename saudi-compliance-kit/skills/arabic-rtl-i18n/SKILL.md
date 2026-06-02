---
name: arabic-rtl-i18n
description: Audit and fix Arabic localization & RTL correctness in a web/mobile UI — translation key parity across locale files, untranslated/English-leaking strings, dir/lang handling, mirrored layouts and column order, bidi numerics/currency, and per-locale contrast. Use when adding UI text, editing locale files, or auditing Arabic/RTL support.
---

# Arabic RTL & i18n parity audit

Arabic is **right-to-left (RTL)** and is the primary consumer language in Saudi Arabia. Most i18n bugs in Arabic apps are not mistranslations — they are **structural**: missing keys, layouts that flip text but not order, and Latin-numeral/currency direction glitches. This skill finds them.

## How to run the audit

1. **Locate the locale files** (e.g. `en.*` / `ar.*` in `locales/`, `i18n/`, `messages/`, or an i18n library's resources).
2. **Run the checks below**, reporting concrete key paths and file/line evidence. Do **not** invent translations — surface untranslated keys for a human/translator unless the Arabic is unambiguous.

## Key parity

- [ ] **Exact key-set parity** between the base locale (usually `en`) and `ar` (and any other locales). List:
  - keys in `en` missing from `ar` (untranslated),
  - keys in `ar` not in `en` (orphaned/stale).
- [ ] **English-leaking values** — `ar` entries whose value is still Latin/English text (likely placeholder).
- [ ] **Interpolation parity** — the same `{param}` / `%s` tokens appear in both locales (a dropped token breaks rendering).
- [ ] **Suggest a CI guard** — a test that fails the build when locale key-sets diverge. Provide a ready-to-paste snippet for the project's test runner.

## RTL correctness

- [ ] `dir="rtl"` and `lang="ar"` set on the document/root when Arabic is active.
- [ ] **Layout mirrors, not just text** — navigation, icons that imply direction (chevrons, back/forward, progress), and **data-table column order** reverse for RTL (rightmost → leftmost), not only text alignment.
- [ ] **Logical CSS** — prefer `margin-inline-start` / `padding-inline-end` / `start`/`end` over hard-coded `left`/`right`.
- [ ] **Bidi numerics & currency** — Latin digits, prices ("SAR 25"), dates, and phone numbers render in the correct direction inside RTL text (use bidi isolation where needed).
- [ ] **Mixed-content fields** (e.g. a URL or English brand inside Arabic text) don't scramble.
- [ ] **Per-locale contrast** — re-check WCAG AA color contrast in Arabic + each theme; translated strings can change emphasis/weight and reveal contrast issues.
- [ ] **Fonts** — an Arabic-capable font with proper shaping is loaded; no tofu (□) glyphs.

## Output format

Two sections (Key parity / RTL correctness). For parity, list exact key paths in each bucket. For RTL, list issues with `path/to/file:line` + a one-line fix. End with: whether a parity CI guard exists, and an offer to add the guard and fill unambiguous missing keys.

## Notes
- Works alongside any i18n approach (i18next, vue-i18n, hand-rolled key maps, gettext).
- Pair with `pdpl-privacy-check` and `zatca-einvoice-check` for full Saudi-market readiness.
