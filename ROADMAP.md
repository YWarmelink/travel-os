# Roadmap

Where this project is headed. For how it currently works, see `README.md` and `CLAUDE.md`.

## Next up

Nothing concrete queued yet — pick this up during the planned brainstorm with YourAtlas (see below).

## Planned features

Agreed direction (2026-07 brainstorm), not yet designed or scheduled. **Criterion for this section: buildable with the current stack, no blocking dependency** — nothing here needs to wait on something bigger below.

- **Car as a transport option, chosen per trip** — a user-selectable preference (not automatic), similar to the existing "Flight range" filter. Needs a second cost model alongside `flightLegCost()`: not per-traveler like flights, but a shared-per-car cost (fuel/tolls/ferry), the same pattern YourAtlas' Route Builder already uses for its self-driven expeditions (Central European Grand Roadtrip, British Isles & Celtic Coast). Only meaningful for countries actually reachable by car from NL — not a replacement for flights everywhere.
- **Small/weekend trips from NL don't work well today** — likely cause (unverified against the live Sheet, but consistent with what's visible locally): `min_days` per country is set assuming a proper multi-day vacation, so a 2-day weekend trip may not have a valid duration to rank at all, regardless of budget. Fix goes directly into the main ranking (not a separate mode like 🎯 Ideal trip): lower `min_days` for nearby/cheap countries where a short trip genuinely makes sense, so weekend trips compete in the normal ranking rather than needing their own filter. Pairs naturally with the car option above — removing the fixed flight-cost floor is what makes a small budget viable for nearby countries in the first place.
- **CO2/footprint estimate per trip** — rough estimate from the flight/car legs, shown alongside cost. Directly useful once the car option exists (a real trade-off to weigh, not just cost/time).
- **Currency & local-cost cheat sheet per country** — a short reference card (exchange rate, tipping norms, typical daily spend) shown alongside a trip — mostly a presentation feature over data that's partly already there (`daily_cost_*`).
- **Shareable comparison card** — export the existing Vergelijkmodus (pin up to 3 trips) as something sendable to a friend for a quick opinion/vote. Builds on a feature that already exists, doesn't need new data.
- **Seasonal suggestion nudge** — "it's getting cold, here's a warm destination that fits your usual budget," shown as a banner on next visit using data that already exists (season scores, `U` budget/style defaults). Deliberately scoped to a banner, not a push notification — see the note below on why a real alert channel is a bigger step.
- **"Surprise me" button** — occasionally suggest a wildcard trip outside the usual top-ranked results (e.g. from the bottom half, or a country not recently suggested), for spontaneity. Just a different sampling over the already-computed ranked list, no new data.

## Long-term / someday

**Criterion for this section: either blocked on something else in this same section, or too large-scope to start without its own dedicated design discussion first.** Not "less important" — just not startable yet.

- **Self-hosted personal AI, integrated** — deliberately small and vague on purpose: for personal use only, not a product, explicitly scoped down so it doesn't balloon into its own project. No design yet — revisit only if/when it's still interesting, keep it small when it happens.

- **Shareable read-only link for a single trip** — a reisgenoot can view (not edit) one trip. Blocked on a real design question: without a backend, this needs the trip data encoded into the URL itself or some other static-hosting-compatible trick — worth designing properly once (or after) the backend-migration item below exists, rather than building a fragile workaround now.
- **Combined fit-profile for two travelers** — a partner adds their own preference weights and the app shows a score that fits both. This is genuinely multi-user territory (two people's settings existing side by side), not a small addition — ties directly into the multi-user item already flagged as a bigger step in YourAtlas' `ROADMAP.md`. Don't build a one-off "second profile" hack; fold it into that same multi-user design when it happens.
- **Price-drop alert** — notify when a tracked route's flight price drops. Explicitly blocked on two other long-term items already on the board: live flight-price scraping (needs real current prices to compare against) and the backend migration (needs somewhere to run a scheduled check and somewhere to send a notification from) — both detailed in YourAtlas' `ROADMAP.md`. Not buildable before those exist.

- **This app folds into YourAtlas as its ranking-engine mode** (decided direction, 2026-07 brainstorm — not yet designed or started):
  - **Goal is functional integration, not just a shared dataset** — should end up actually sharing logic with YourAtlas, not just reading the same Sheet.
  - **YourAtlas is the umbrella** — it's already the broader dashboard concept (Trips/Countries/Map/Route Builder); this app's ranking engine (`calcTrip`/`rankCalced` in `js/app.js`) becomes an additional mode inside it, this project doesn't stay a separate identity long-term.
  - **Sequencing: after YourAtlas' Route Builder Sheet-sync is done**, not alongside it — see YourAtlas' own `ROADMAP.md`.
  - **Candidate integrations floated in the brainstorm, none decided yet**:
    - Applying this app's season/budget fit-scoring to Route Builder's country blocks
    - One shared traveler profile (budget, travel-style weights, season preference) instead of each app keeping its own settings
    - One shared "visited" status, sourced from YourAtlas' Countries/Map tracker, read here instead of tracked separately
  - Revisit properly once YourAtlas' sync work is done — don't start designing the merge itself before then.

- **Live flight-price scraping** and **moving off Google Sheets to a real backend** — both floated in the same 2026-07 brainstorm, both bigger than this project on its own. Since the merge above puts YourAtlas in the driver's seat, the full write-up (what it'd unlock, what it costs, sequencing) lives in YourAtlas' `ROADMAP.md` rather than duplicated here — this app's FLIGHTS/COUNTRIES data would be in scope for both once picked up.
