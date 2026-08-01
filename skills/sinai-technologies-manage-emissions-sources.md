---
name: Configure emissions sources and read monthly emissions
description: Create emissions sources under a business entity and read/close monthly carbon-accounting data.
api: openapi/sinai-technologies-openapi-original.yml
operations: [getProcessTypes, createEmissionsSource, getEmissionsSources, getMonthlyBusinessEntityEmissionsSummaries, closeActivityMonth]
---

# Configure emissions sources and read monthly emissions

## Auth
Two scopes are involved. Configuring sources needs `https://api.sinai.com/organization`; reading summaries and
opening/closing activity months needs `https://api.sinai.com/carbon_accounting`. Request both at the token
endpoint (space-separated) if the flow does all of it. Base URL: `https://api.sinai.com/v1`.

## Steps
1. `getProcessTypes` — look up the process types available so you can attach a source to the right process.
2. `createEmissionsSource` — create a source under a business-entity process (`processID`, `emissionsSourceTypeID`, `emissionsModelID`, `scope`).
3. `getEmissionsSources` — list sources, optionally filtered by `processID` or business entity ID, to confirm.
4. `getMonthlyBusinessEntityEmissionsSummaries` — read computed monthly emissions rolled up per business entity.
5. `closeActivityMonth` — close an activity month once its data is final (use `openActivityMonth` to reopen).

## Rules
- Emissions summaries are organized by reporting month / fiscal or calendar year (see `getReportingMonths`).
- `scope` on an emissions source is the GHG Protocol scope of the emission, distinct from the OAuth scope.
- Deleting a source (`deleteEmissionsSource`) removes all its activity data — irreversible; confirm before calling.
- No idempotency keys: treat `createEmissionsSource` / `closeActivityMonth` as non-idempotent.
