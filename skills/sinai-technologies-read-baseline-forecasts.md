---
name: Read baseline forecasts and decarbonization projections
description: Retrieve baseline scenarios and their activity / main-metric projections for decarbonization planning.
api: openapi/sinai-technologies-openapi-original.yml
operations: [getBaselineScenarios, getActivityProjections, getMainMetricProjections, getProjectionsSummaryForBusinessEntity]
---

# Read baseline forecasts and decarbonization projections

## Auth
OAuth2 Bearer token with scope `https://api.sinai.com/baseline_forecasts`. Base URL: `https://api.sinai.com/v1`.

## Steps
1. `getBaselineScenarios` — list baseline scenarios for the org; note the `isDefault` one.
2. `getActivityProjections` — read projected activity levels underlying a scenario (paginate with `limit`/`offset`).
3. `getMainMetricProjections` — read the projected main-metric (e.g. emissions) trajectory.
4. `getProjectionsSummaryForBusinessEntity` — get the rolled-up projection summary for a specific business entity.

## Rules
- These endpoints are read-only (GET); safe to retry.
- Scenarios and projections belong to an organization (`orgID`); a 404 means the ID is not in the caller's org.
- Pair the baseline (this skill) with carbon-accounting actuals to compare planned vs. realized decarbonization.
