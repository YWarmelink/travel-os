---
name: manage-destinations
description: Use when adding a new country/destination to YourIdealTravel, or updating/correcting an existing one's costs, seasons, or flight data — editing the COUNTRIES and FLIGHTS tabs in the live Google Sheet and understanding how those fields feed calcTrip()/flightLegCost() in js/app.js.
---

## Source of truth

Edit the live Google Sheet directly (Sheet ID in `README.md`). The CSVs under `sheets-import/` are old one-off import snapshots, not where changes happen now — don't treat them as current data, and don't round-trip through them for a routine add or update.

## Two tabs to touch

**COUNTRIES** (gid 2119597216):
- `daily_cost_backpack` / `daily_cost_mid` / `daily_cost_premium` — Luxury is *not* a separate column, it's computed in `app.js` as `premium × 1.45`. Don't add one.
- Season month definitions — feed `countrySeasonScore()`
- `sub_region` — optional, but enables the 3-country combo adjacency check (`SUBREGION_ADJACENT` in `app.js`: A's sub_region must border B's, B's must border C's). Omitting it just skips that check for this country, nothing breaks.
- `flight_key` — links to the matching FLIGHTS rows

**FLIGHTS** (gid 99695727):
- NL↔country low/mid/high season costs, looked up by `flightLegCost()` based on the *destination* country's season (for the return leg X→NL, the departure country's season is used instead)
- `region_cluster` (`Europe` / `Intercontinental`) — drives the "Europe only" / "Outside Europe" filter, based only on the first leg (NL→A)

## Adding a new country

1. Add a row to COUNTRIES with real, researched daily costs per style tier and season months
2. Add NL↔country rows to FLIGHTS with real low/mid/high prices
3. Sync in the app — no code change needed for a pure data addition
4. Update the country count in `README.md` ("app bevat momenteel N landen")

## Correcting an existing country

Same two tabs — just edit the row's existing values. There's no migration/versioning system here (unlike YourAtlas's Route Builder): this data lives in the Sheet, not `localStorage`, and is fetched fresh on every Sync, so a correction just needs the Sheet edit. Make sure it's actually researched (real current prices/seasons), not guessed.

## Don't

- Don't add a luxury cost column — it's derived, not stored
- Don't skip `sub_region` "to be safe" if the real adjacency is known — that's the only thing that enables realistic combo trips through that country
- Don't edit `sheets-import/` CSVs expecting the app to pick it up — it reads the Sheet, not those files
