---
name: pdpl-check
description: Review a web/mobile app for Saudi compliance risk — PDPL (SDAIA) data protection, consumer-protection / e-commerce disclosures, and VAT/ZATCA (Fatoorah) e-invoicing. Reports Present/Missing/Unknown with file evidence and fixes. Use when touching auth, signup, uploads, analytics, billing, pricing, invoices, or account deletion in a product serving Saudi Arabia.
---

# Saudi compliance review (engineering-level)

> **Not legal or tax advice.** This skill surfaces *engineering-level* compliance risk so a **licensed Saudi attorney** and a **ZATCA-experienced accountant** can confirm scope and current regulation text. Always end the review recommending professional sign-off for anything legal or fiscal.

Covers the three regimes most often missed by engineering teams shipping to Saudi Arabia: **PDPL (SDAIA)** data protection, **consumer-protection / e-commerce** disclosures, and **VAT / ZATCA (Fatoorah)** e-invoicing. Stack-agnostic — it reads the codebase and UI surface.

## How to run the review

1. **Map the surface.** Find where personal data and money enter/leave: signup/auth, profile, uploads, analytics, third-party APIs (especially LLM providers), payments, invoices, and export/deletion paths.
2. **Search the codebase** for the markers below (adapt globs to the stack):
   - `privacy`, `consent`, `terms`, `cookie`, `gdpr`, `pdpl`, `data-protection`
   - `delete.*account`, `export`, `download.*data`
   - analytics SDKs (`matomo`, `gtag`, `analytics`, `segment`, `posthog`, `mixpanel`)
   - LLM/cloud processors (`openai`, `anthropic`, `s3`, `azure`, `gcp`)
   - billing (`invoice`, `receipt`, `vat`, `tax`, `zatca`, `fatoorah`, `qr`, `pdf`)
3. **Report `Present` / `Missing` / `Unknown`** per item, each with concrete `path:line` evidence, a severity, and a one-line fix. A missing legally-required item is `critical`/`high`.

## 1. PDPL (SDAIA — Personal Data Protection)

- [ ] **Privacy notice** published and linked from signup, footer, and any data-collection point (ideally bilingual AR/EN).
- [ ] **Consent** captured where required (signup; and again at data upload if uploads may contain personal data), with a recorded timestamp.
- [ ] **Data-subject rights** actionable in-product: access, correction, **deletion**, and **data portability/export** ("download my data" — distinct from a content-export feature).
- [ ] **Cookie / analytics consent banner** gating non-essential trackers *before* they fire. Analytics loading on first paint without consent is a common violation.
- [ ] **Cross-border transfer disclosed.** If personal data is sent to LLM APIs, cloud storage, or any processor outside KSA, the privacy notice must name these sub-processors and the transfer. **Read the actual notice text — do not assume.**
- [ ] **Lawful basis** documented for each purpose; **data minimization & retention** policy exists.
- [ ] **Breach-notification** process exists (often operational — flag `Unknown` and ask).

## 2. Consumer protection / e-commerce

- [ ] **Pre-charge price disclosure** — the cost of any paid action is shown *before* the user commits, not only in a post-failure error, and consistently (not buried in tooltips).
- [ ] **Full price incl. VAT** displayed.
- [ ] **Merchant identity + Terms of Service** accessible before purchase.
- [ ] **Auto-renewal, cancellation, and refund** terms explicit and shown at the point of sale.
- [ ] No misleading pricing.

## 3. VAT / ZATCA e-invoicing (Fatoorah)

A VAT-registered seller must issue **ZATCA-compliant electronic invoices**. A styled PDF emailed to the customer is **not** sufficient.

- [ ] **Seller name + VAT registration number** (and CR number) on the invoice.
- [ ] **Unique sequential invoice number**, issue **date & time**.
- [ ] **VAT breakdown** — taxable amount, **15% VAT** as a separate line, total incl. VAT (not just "price includes VAT").
- [ ] **Totals in SAR**, correctly formatted (watch halala→SAR: amount/100).
- [ ] **QR code** (TLV-encoded, base64) with seller name, VAT number, timestamp, total, VAT total.
- [ ] **Phase-2 integration** — invoice generated as **UBL 2.1 XML**, **cryptographically stamped**, and **cleared** (standard) or **reported** (simplified) via ZATCA's API, with status persisted and failures retried.
- [ ] Arabic present on the invoice (primary language for Saudi tax documents).

## Output format

Three grouped sections (PDPL / Consumer protection / VAT-ZATCA). Each finding:

> **[severity] Item** — `Present` / `Missing` / `Unknown`
> Evidence: `path/to/file:line`
> Fix: one concrete sentence.

Close with the top 1–3 blocking gaps and a one-line reminder to get **licensed Saudi legal + ZATCA accounting sign-off** before launch.

## References
- PDPL / SDAIA: https://sdaia.gov.sa
- Consumer protection & e-commerce (Ministry of Commerce): https://mc.gov.sa
- ZATCA E-Invoicing (Fatoorah): https://zatca.gov.sa/en/E-Invoicing/Pages/default.aspx
