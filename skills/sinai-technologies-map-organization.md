---
name: Map an organization's carbon-accounting hierarchy
description: Read the current SINAI organization and walk/extend its business-entity tree.
api: openapi/sinai-technologies-openapi-original.yml
operations: [getCurrentOrganization, getBusinessEntities, getBusinessEntityDescendants, createBusinessEntity]
---

# Map an organization's carbon-accounting hierarchy

Use the SINAI API to understand and build out the corporate structure that carbon data hangs off of.

## Auth
Get an OAuth2 client-credentials token from `https://auth.sinai.com/oauth2/token` with scope
`https://api.sinai.com/organization`. Send it as `Authorization: Bearer <token>`. Base URL: `https://api.sinai.com/v1`.

## Steps
1. `getCurrentOrganization` — read the org; note `rootBusinessEntityID`, `defaultFunctionalUnitID`, and `fiscalYearStartMonth`.
2. `getBusinessEntities` — list business entities (filterable); paginate with `limit`/`offset`.
3. `getBusinessEntityDescendants` — for the root (or any entity) ID, walk the full subtree of children and their children.
4. `createBusinessEntity` — add a new entity, setting `parentID`, `name`, `businessEntityTypeID`, and (for facilities) `facilityTypeID`/`processTypeID`.

## Rules
- The hierarchy is a self-referential tree via `parentID`; the org's `rootBusinessEntityID` is the top.
- Writes are not idempotent — no idempotency-key is supported, so do not blindly retry `createBusinessEntity`; on a timeout, re-list before re-creating.
- A 403 means the token lacks the `organization` scope; a 404 means the entity ID is not visible to this org.
