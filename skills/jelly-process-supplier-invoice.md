---
name: Process a supplier invoice in Jelly
description: >-
  Review pending supplier invoices and approve them so their line prices flow
  into ingredient costing, using the Jelly GraphQL API.
api: graphql/jelly-api.graphql
endpoint: https://api.getjelly.co.uk/
operations:
  - invoices
  - approveOneInvoice
  - suppliers
---

# Process a supplier invoice in Jelly

Approving invoices is how price changes reach recipe/dish costing.

## Auth
GraphQL over `POST https://api.getjelly.co.uk/` with
`Authorization: Bearer <token>`.

## Steps
1. **List invoices** for the kitchen with the `invoices` query, filtering with
   `where` on status (see the `InvoiceStatus` enum) to find pending ones.
2. **Inspect suppliers** with the `suppliers` query if you need to confirm the
   supplier a pending invoice belongs to.
3. **Approve the invoice** with the `approveOneInvoice` mutation. This finalises
   the invoice and propagates its line prices into ingredient costs.
4. Handle rejections: a rejected invoice carries an `InvoiceRejectionReason`
   (see `graphql/jelly-api.graphql`).

## Conventions
- Errors surface as GraphQL `errors[]` with `extensions.code`
  (`errors/jelly-error-codes.yml`).
- `deleteOneInvoice` is deprecated; prefer the current invoice mutations in the
  schema.
- Accounting sync (Xero) is handled through the `Acc*` types once an invoice is
  approved.
