---
name: pdpl-privacy-check
description: Review a web/mobile app for Saudi PDPL (SDAIA Personal Data Protection Law) and consumer-protection risk — privacy notice, consent, data-subject rights, cross-border transfer, cookie/analytics consent, and pre-charge price disclosure. Use when touching auth, signup, uploads, analytics, billing, pricing, account deletion, or anything handling personal data in a product serving Saudi Arabia.
---

# Saudi PDPL & consumer-protection review (engineering-level)

> **Not legal advice.** This skill surfaces engineering-level compliance risk so a **licensed Saudi attorney** can confirm scope and current regulation text. Always end the review recommending counsel sign-off for anything legal.

Saudi Arabia's **Personal Data Protection Law (PDPL)**, enforced by **SDAIA**, applies to any product processing the personal data of people in KSA — regardless of where the company is based. This skill checks an app's *code and UI surface* against the obligations most often missed by engineering teams.

## How to run the review

1. **Map the surface.** Identify where personal data enters and leaves: signup/auth, profile, file/data uploads, analytics, third-party APIs (especially LLM providers), payment, and any export/deletion paths.
2. **Search the codebase** for the markers below (adapt globs to the stack):
   - `privacy`, `consent`, `terms`, `cookie`, `gdpr`, `pdpl`, `data-protection`
   - `delete.*account`, `export`, `download.*data`
   - analytics SDK names (`matomo`, `gtag`, `analytics`, `segment`, `posthog`, `mixpanel`)
   - LLM/cloud providers (`openai`, `anthropic`, `s3`, `azure`, `gcp`)
3. **For each checklist item, report `Present` / `Missing` / `Unknown`** with concrete file evidence, a severity, and a one-line remediation. A *missing legally-required* item is `critical`/`high`.

## PDPL checklist

- [ ] **Privacy notice** published and linked from signup, footer, and any data-collection point (ideally bilingual AR/EN).
- [ ] **Consent** captured where required (signup, and again at data upload if uploads may contain personal data), with a recorded timestamp.
- [ ] **Data-subject rights** are actionable in-product: access, correction, **deletion**, and **data portability/export** ("download my data" — distinct from a product's content-export feature).
- [ ] **Cookie / analytics consent banner** gating non-essential trackers *before* they fire. Analytics that load on first paint without consent is a common violation.
- [ ] **Cross-border transfer disclosed.** If personal data is sent to LLM APIs, cloud storage, or any processor outside KSA, the privacy notice must name these sub-processors and the transfer. **Read the actual notice text — do not assume.**
- [ ] **Lawful basis** documented for each processing purpose.
- [ ] **Breach-notification** process exists (often operational, not in code — flag as `Unknown` and ask).
- [ ] **Data minimization & retention** — only necessary fields collected; a retention/auto-purge policy exists.

## Consumer-protection (e-commerce) checklist

- [ ] **Pre-charge price disclosure** — the cost of any paid action is shown *before* the user commits, not only in a post-failure error. Disclosure must be consistent, not buried in tooltips.
- [ ] **Full price incl. VAT** displayed.
- [ ] **Merchant identity + Terms of Service** accessible before purchase.
- [ ] **Auto-renewal, cancellation, and refund** terms explicit and shown at the point of sale.
- [ ] No misleading pricing.

## Output format

Three grouped sections (PDPL / Consumer-protection / Open questions). Each finding:

> **[severity] Item** — `Present` / `Missing` / `Unknown`
> Evidence: `path/to/file:line`
> Fix: one concrete sentence.

Close with: the top 1–3 blocking gaps, and a one-line reminder to get **licensed Saudi legal sign-off** before launch.

## References
- PDPL / SDAIA: https://sdaia.gov.sa
- Consumer protection & e-commerce (Ministry of Commerce): https://mc.gov.sa
- Pair with `zatca-einvoice-check` for VAT/e-invoicing and `arabic-rtl-i18n` for localization.
