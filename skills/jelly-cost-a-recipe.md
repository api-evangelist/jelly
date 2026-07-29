---
name: Cost a recipe in Jelly
description: >-
  Create ingredients from supplier products and assemble them into a costed
  recipe with gross-profit (GP), using the Jelly GraphQL API.
api: graphql/jelly-api.graphql
endpoint: https://api.getjelly.co.uk/
operations:
  - createOneIngredient
  - createOneRecipe
  - recipe
  - ingredients
---

# Cost a recipe in Jelly

Jelly is a costing and kitchen-management platform. This skill builds a costed
recipe for a kitchen.

## Auth
All operations are GraphQL over `POST https://api.getjelly.co.uk/`. Send
`Authorization: Bearer <token>` on every request (an unauthenticated call
returns `extensions.code: UNAUTHENTICATED`). See
`authentication/jelly-authentication.yml`.

## Steps
1. **List existing ingredients** for the kitchen with the `ingredients` query
   (Prisma-style args: `where`, `orderBy`, `take`, `skip`, `cursor`) to avoid
   duplicates.
2. **Create any missing ingredient** with the `createOneIngredient` mutation,
   linking it to the supplier `Product` that prices it.
3. **Create the recipe** with `createOneRecipe`, attaching its ingredients
   (via the `RecipeToIngredient` join) and quantities.
4. **Read back the recipe** with the `recipe` query to confirm the derived cost
   and GP.

## Conventions
- Pagination/filtering: `take`/`skip`/`cursor`/`where`/`orderBy` (see
  `conventions/jelly-conventions.yml`).
- Mutations are **not** idempotent (no idempotency key); check with the
  `ingredients` query before re-creating.
- Errors arrive in the GraphQL `errors[]` array with `extensions.code`
  (`errors/jelly-error-codes.yml`).
