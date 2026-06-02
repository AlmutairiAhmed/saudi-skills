---
name: zatca-einvoice-check
description: Review billing/invoice code for Saudi ZATCA (Fatoorah) e-invoicing and VAT compliance — Phase-2 structured XML, cryptographic stamp, QR code, mandatory invoice fields, 15% VAT handling, and SAR formatting. Use when touching payments, invoices, receipts, subscriptions, or any code that issues a proof of purchase to a customer in KSA.
---

# Saudi ZATCA (Fatoorah) e-invoicing & VAT review (engineering-level)

> **Not legal/tax advice.** Confirm scope, VAT registration, and Phase-2 onboarding dates with a **ZATCA-experienced accountant**. This skill checks whether *invoice-generating code* looks compliant — it cannot confirm regulatory standing.

A **VAT-registered** seller in Saudi Arabia must issue **ZATCA-compliant electronic invoices** (Fatoorah). A styled PDF emailed to the customer is **not** sufficient. The two regimes:

- **Phase 1 (Generation):** structured electronic invoices with all mandatory fields + a **QR code** (TLV-encoded, base64).
- **Phase 2 (Integration):** invoices generated as **XML (UBL 2.1)**, **cryptographically stamped**, and either **cleared** (B2B/standard) or **reported** (B2C/simplified) through ZATCA's platform, with a cryptographic stamp identifier.

## How to run the review

1. **Find the invoice/receipt generator** (search: `invoice`, `receipt`, `pdf`, `vat`, `tax`, `zatca`, `fatoorah`, `qr`, `weasyprint`, `pdfkit`).
2. **Check what it actually emits.** A common anti-pattern: a cosmetic PDF with a logo + amount and nothing else.
3. For each item below report `Present` / `Missing` / `Unknown` with file evidence + severity. Missing mandatory fields on a VAT-registered seller's invoice = `critical`.

## Mandatory invoice-field checklist

- [ ] **Seller name + VAT registration number** (and CR number).
- [ ] **Buyer details** where required (standard/B2B invoices).
- [ ] **Invoice issue date & time**, and a unique sequential invoice number.
- [ ] **Line items** with unit price, quantity, and per-line tax.
- [ ] **VAT breakdown** — taxable amount, **15% VAT** shown as a separate line, and total incl. VAT (not just "price includes VAT").
- [ ] **Totals in SAR**, correctly formatted (watch halala→SAR conversions: amount/100).
- [ ] **QR code** (TLV, base64) encoding seller name, VAT number, timestamp, total, and VAT total.
- [ ] **Invoice type** correct (standard/tax invoice vs. simplified).
- [ ] Arabic present on the invoice (Arabic is the primary language for Saudi tax documents).

## Phase-2 integration checklist

- [ ] Invoice produced as **UBL 2.1 XML**, not only a rendered PDF.
- [ ] **Cryptographic stamp** / signature applied; CSID (Cryptographic Stamp Identifier) obtained via onboarding.
- [ ] **Clearance** (standard) or **reporting** (simplified) call to the ZATCA API, with the returned status persisted.
- [ ] Failures are retried/queued — an invoice that fails clearance must not be silently dropped.

## Output format

Two sections (Mandatory fields / Phase-2 integration). Each finding:

> **[severity] Item** — `Present` / `Missing` / `Unknown`
> Evidence: `path/to/file:line`
> Fix: one concrete sentence.

Close with whether the current output could plausibly pass ZATCA, the top blocking gaps, and a reminder to confirm **VAT registration + Phase-2 onboarding** with a ZATCA-experienced accountant.

## References
- ZATCA E-Invoicing (Fatoorah): https://zatca.gov.sa/en/E-Invoicing/Pages/default.aspx
- Pair with `pdpl-privacy-check` for data-protection/consumer items.
