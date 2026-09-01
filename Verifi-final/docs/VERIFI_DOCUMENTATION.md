# Verifi — Technical & Product Documentation

## 1. Problem being solved

Financial operations teams often receive payment/settlement records and invoice records from different systems. The same business event can appear with different identifiers, formatting, merchants, dates, or amounts. Manual comparison is slow and makes it difficult to explain why a transaction was accepted or sent for review.

Verifi is designed as an evidence-first reconciliation control. It does not treat every plausible similarity as a safe match. Instead it normalizes incoming records, compares multiple signals, assigns a confidence score, and sends uncertain cases to a human-review queue.

## 2. Product flow

**Ingest → Normalize → Match → Explain → Report**

### Ingest
The user can provide a payment file, an invoice file, or both. Both sources are optional because standalone data-quality analysis is useful when only one export is available.

### Normalize
Column names are mapped to a small canonical schema. Common variants are accepted so the user does not have to rename their spreadsheet manually.

Canonical payment fields:
- transaction_id
- invoice_id
- merchant
- amount
- date
- status

Canonical invoice fields:
- invoice_id
- merchant
- amount
- date

### Match
When both datasets are supplied, Verifi first uses the invoice identifier where available, then evaluates amount, merchant and date evidence. Exact matches receive a high-confidence result. Conflicting evidence is deliberately routed to review.

### Explain
Every non-exact result has an issue label such as:
- Amount mismatch
- Merchant mismatch
- Missing invoice
- Payment record missing
- Likely match; identifier differs

### Report
The UI exposes:
- Records processed
- Automatic matches
- Exceptions
- Reconciled value
- Match-rate visualization
- Exception table
- Transaction ledger
- Audit trail
- CSV exception export

## 3. Why the previous build showed “Failed to fetch”

The uploaded project supplied for this revision contained only `index.html`, while that HTML referenced `styles.css` and `app.js`. More importantly, the earlier design depended on a frontend/backend communication path. A browser cannot successfully complete that request when the backend is not running or is otherwise unavailable.

This revision removes that fragile dependency for the assessment demo. The complete application is self-contained in `index.html`, and the prepared demo dataset is embedded in the page. Therefore the demo path does not call an API or use `fetch()`.

## 4. Demo dataset

The demo contains 100 payment records and 100 invoice records. It intentionally includes several controlled discrepancies:

- 7 amount mismatches.
- 4 invoice identifiers that do not correspond to the payment identifier.
- 2 merchant mismatches.

These cases make the exception center demonstrable instead of presenting a perfectly clean dataset.

## 5. Upload behavior

Payment and invoice uploads are separate. Either can be omitted.

CSV parsing is implemented locally in the browser with support for quoted fields and commas inside quoted values.

XLSX parsing is implemented locally using the browser's ZIP/decompression APIs and XML parsing. The first worksheet is read. No external spreadsheet library is required.

## 6. Safety against false matches

Verifi intentionally prefers a review state over a forced automatic match when evidence conflicts. This is important in financial reconciliation because a visually plausible match is not necessarily an accurate accounting decision.

## 7. UI/UX decisions

The interface remains dark, but uses a different visual language from the original reference: near-black blue-gray surfaces, cyan operational signals, indigo-violet action accents, thin borders, soft radial lighting, and restrained motion. The goal is a finance operations control room rather than a generic dashboard.

Animations are used for:
- ambient background lighting
- engine status pulse
- hero orbital rings
- page transitions
- analysis progress
- button hover states

## 8. Technology

- HTML5
- CSS3
- Vanilla JavaScript
- Browser File API
- Browser DecompressionStream API for XLSX compressed entries
- No framework
- No external API
- No database
- No external runtime dependency

## 9. Suggested demonstration sequence

1. Open the page.
2. Start **Run demo analysis** from the header.
3. Show the overview metrics and confidence profile.
4. Open **Exceptions** and explain why uncertain records were not auto-matched.
5. Open **Transactions** and search for a merchant or payment ID.
6. Open **Audit trail** to show traceability.
7. Open **Reconcile**.
8. Upload the included `demo/payments.csv` and `demo/invoices.csv` to demonstrate the real file-ingestion path.
9. Run intelligent analysis and show that the same type of reconciliation is produced from actual uploaded files.
10. Export the exception queue as CSV.

## 10. Limitations

This is an internship assessment prototype, not a production accounting system. It does not connect to payment gateways, ERP systems, authentication, a persistent database, or an enterprise ML service. The matching engine is deterministic and explainable so its behavior can be demonstrated reliably in a local environment.
