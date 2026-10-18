# RimRunnerNorthDakota

Unofficial North Dakota HS boys basketball ratings. Not affiliated with the NDHSAA or MaxPreps.
Status: **HELD (waiting on hosting)**. Preview: https://joshua-kpg.github.io/RimRunnerNorthDakota/ (GitHub Pages). No Firebase project yet
(the Google accounts are at their project quota) and Cloudflare Pages is at its project limit, so accounts, picks and follows save only in the
visitor's browser until the site's own Firebase project exists.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's future Firebase project (accounts, picks, follows).

## Teams
122 teams: the NDHSAA (North Dakota High School Activities Association) "2026-27 Boys Basketball Classifications" (Approved - Boys Basketball
Enrollments sheet, Fall 2025 enrollments; cutoffs AA 650+, A 162.5-649, B under 162.5): Division AA 19, Division A 29, Division B 74.
Shanley and Bismarck St. Mary's opt up to AA. Many teams are co-ops; each co-op is one team under its NDHSAA name, and MaxPreps pages for its
member schools are folded into it. Minnewaukan is listed by the NDHSAA as not in a co-op and is not placed in any division, so it is not shown.
No Other (non-NDHSAA) programs met the 8-game rule.
Conferences are the NDHSAA's own groupings: Division AA East Region (9) and West Region (10), Division A Regions 1A-4A, and Division B
Districts 1-8 (inside Regions 1B-4B).

## Season rule
NDHSAA 2026-27 boys basketball: Division AA opening date Nov 23, first contest Fri Dec 4, 2026, state tournament Mar 11-13, 2027; Divisions A and B
opening date Nov 30, first contest Fri Dec 11, 2026, state tournaments Mar 18-20, 2027.
Games involving an NDHSAA team before its division's first contest date are left out (schedule build and nightly updater); games between two
non-NDHSAA schools are kept. Duplicate next-day listings of one game are removed (shared tournament-repeat rule, tested on both seasons).
Game times are Central, as MaxPreps posts them (the southwest corner of the state is on Mountain time).

## Nightly scores
`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC (1:30 am / 9:00 am Central standard time) and
on manual dispatch. Before `SEASON_START` in index.html (Fri Dec 4, 2026) the script makes no requests. From then on it reads boys basketball
finals for the last 3 days from the MaxPreps North Dakota scores-by-date page (team-page fallback), matches them to the schedule by MaxPreps team
id + date (co-op teams also by the partner-school page ids in `mpAliasIds`), records them in `data/results.json` and bakes scores and W-L records
into `index.html`. The page recomputes Elo from those finals. It commits only when data changed; GitHub Pages updates from the commit.
