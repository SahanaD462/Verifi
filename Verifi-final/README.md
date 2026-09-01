# Verifi — Intelligent Reconciliation

Verifi is a client-side reconciliation prototype for the internship assessment. It compares payment and invoice records, normalizes common column names, calculates evidence-based match confidence, routes uncertain cases to an exception queue, and provides an audit trail.

## Run it

1. Extract this ZIP.
2. Open `index.html` in Chrome or Edge.
3. Click **Run demo analysis**. No backend, API, database, or internet connection is required.
4. To test your own data, open **Reconcile** and upload a CSV or XLSX payment file, invoice file, or both.

## Included demo data

- `demo/payments.csv` — 100 payment records.
- `demo/invoices.csv` — 100 invoice records.
- The demo intentionally contains amount mismatches, missing invoice identifiers, and merchant mismatches so the exception workflow is visible.

The website embeds the same demo dataset directly in the browser, so **Run prepared demo** does not depend on `fetch()` or a local server.

## Supported upload columns

The parser recognizes common variants such as:

- Payment ID / Transaction ID / transaction_id
- Invoice ID / invoice_id / invoice
- Merchant / Vendor / Customer / Company
- Amount / Payment Amount / Invoice Amount / Total
- Date / Payment Date / Invoice Date / Transaction Date

Both CSV and XLSX are supported without external JavaScript libraries. For XLSX, Verifi reads the first worksheet.

## What the analysis does

When both sources are present, Verifi evaluates candidate matches using:

1. Invoice/transaction identifier evidence.
2. Amount equality or closeness.
3. Merchant equality.
4. Date equality when supplied.

High-confidence exact matches are marked **matched**. Lower-confidence cases become **review** or **unlinked** rather than being silently forced into a match.

When only one source is supplied, Verifi performs standalone data-quality analysis and clearly labels the records as having no counterpart source.

## Important implementation note

This version intentionally has no external API call. That removes the `Failed to fetch` failure shown in the previous build and makes the demo reliable when opened locally.
