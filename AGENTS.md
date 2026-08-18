# travel-agent — Canonical Agent Rules

## Purpose

Itinerary planner of the Libre AI constellation (couche 1). The model
composes, the code vetoes. Destination knowledge lives in versioned city
packs; reasoning lives in a city-agnostic prompt; **trip instances live
nowhere in this tree**. Authoritative documents, in order: `project.v1.yaml`
(state), then `docs/specs/2026-08-07-travel-agent-design.md` (architecture),
then `docs/specs/2026-08-07-travel-agent-phases.md` (phases). Doctrine and
the fleet template come from:
https://raw.githubusercontent.com/libre-ai/governance/main/docs/method/CONTEXT-TEMPLATE.md

## Domain doctrine

The three strata — the rule that outranks the others:

| Stratum   | Lives in               | May contain                                                   |
| --------- | ---------------------- | ------------------------------------------------------------- |
| Reasoning | `prompts/`             | planning rules, **zero facts about any city**                 |
| Facts     | `data/cities/`         | facts about a city, **zero planning rules, zero stay window** |
| Instance  | outside the repository | stay window, party, budget, lodging                           |

Crossing a boundary is a bug, never a shortcut: a fact in the prompt makes
city N+1 multiplicative instead of additive; a stay window in a pack —
including in a comment — publishes when a home is empty; a pack calendar
listing only the events that overlap one stay is itself a disclosure. Packs
cover the year.

The code computes what a language model computes badly — pass-window
placement — and refuses what violates a verifiable constraint. It does
**not** write itineraries: composing prose in TypeScript reimplements the
model, worse. Adding a code path that generates itinerary text is out of
scope by decision, not by omission.

Every tier-2 fact carries `source` and `verified_on`; freshness is measured
**against the stay window**, not against today. `⚠ UNVERIFIED` is always
better than a confident guess.

## Commands

- `bun run check:trip-isolation` — enforces the three-strata boundary; run
  before staging, not after.
- `bun run check:local` — the full local gate chain; never hide a red test.

## Working here

Security > quality > performance > completeness. Commits and code in
English, documentation in French. No machine-local absolute path in a
tracked file.
