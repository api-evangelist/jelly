---
name: Build a menu in Jelly
description: >-
  Turn costed recipes into sellable dishes and assemble them into a menu, using
  the Jelly GraphQL API.
api: graphql/jelly-api.graphql
endpoint: https://api.getjelly.co.uk/
operations:
  - createOneDish
  - createOneMenu
  - dishes
  - createOrUpdateDishesSold
---

# Build a menu in Jelly

This skill takes recipes to market: create dishes, group them into a menu, and
record sales for GP tracking.

## Auth
GraphQL over `POST https://api.getjelly.co.uk/` with
`Authorization: Bearer <token>`.

## Steps
1. **Create a dish** from a recipe with `createOneDish`, setting its sale price
   (Jelly derives GP from recipe cost vs. sale price).
2. **List dishes** with the `dishes` query (`where`/`orderBy`/`take`) to confirm
   what exists for the kitchen.
3. **Create the menu** with `createOneMenu`, attaching dishes via the
   `MenuToDish` join.
4. **Record sales** with `createOrUpdateDishesSold` to feed GP/insight
   reporting.

## Conventions
- Pagination/filtering follows `conventions/jelly-conventions.yml`.
- Mutations are not idempotent; list before re-creating.
- Errors: GraphQL `errors[]` with `extensions.code`
  (`errors/jelly-error-codes.yml`).
