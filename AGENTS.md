# travel-agent — Canonical Agent Rules

Itinerary planner of the Libre AI constellation (couche 1). The model composes,
the code vetoes. Destination knowledge lives in versioned city packs; reasoning
lives in a city-agnostic prompt; **trip instances live nowhere in this tree**.

Authoritative documents, in order: `project.v1.yaml` (state), then
`docs/specs/2026-08-07-travel-agent-design.md` (architecture), then
`docs/specs/2026-08-07-travel-agent-phases.md` (phases, and the corrections it
carries over the design document).

## The three strata — the rule that outranks the others

| Stratum   | Lives in               | May contain                                                   |
| --------- | ---------------------- | ------------------------------------------------------------- |
| Reasoning | `prompts/`             | planning rules, **zero facts about any city**                 |
| Facts     | `data/cities/`         | facts about a city, **zero planning rules, zero stay window** |
| Instance  | outside the repository | stay window, party, budget, lodging                           |

Crossing a boundary is a bug, never a shortcut. Concretely:

- A fact in the prompt makes city N+1 multiplicative instead of additive.
- A stay window in a pack — including in a comment — publishes when a home is
  empty. A lodging publishes where its occupant is.
- A pack calendar listing only the events that overlap one stay is itself a
  disclosure: the calendar _is_ the signature. Packs cover the year.

`bun run check:trip-isolation` enforces this. Run it before staging, not after.

## What the code is for

The code computes what a language model computes badly — pass-window placement —
and refuses what violates a verifiable constraint. It does **not** write
itineraries: composing prose in TypeScript reimplements the model, worse.

Adding a code path that generates itinerary text is out of scope by decision,
not by omission.

## Facts and freshness

Every tier-2 fact carries `source` and `verified_on`. Never write a price, an
opening time or a door-to-door duration you have not read on an official source.
`⚠ UNVERIFIED` is always better than a confident guess: a wrong opening time
costs a traveller a morning.

Freshness is measured **against the stay window**, not against today. A pack
verified out of season passes an absolute TTL while carrying wrong hours.

## Working here

Run `bun run check:local` before pushing; never hide a red test. Security >
quality > performance > completeness. Commits and code in English, documentation
in French. No machine-local absolute path in a tracked file.
