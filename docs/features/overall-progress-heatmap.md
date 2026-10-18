# Overall Progress — calendar heatmap colour scale

## 1. Goal
Make the Overall Progress calendar heatmap feel motivating for users at every practice level, while staying an honest, permanent record of minutes practised.

## 2. Status
Explored and decided (colour scale and surface split). Prototyped as a standalone HTML file only. Not built in Preview. Not shipped.

## 3. Decisions
- 2026-10-18 — Minutes are the unit. Practice count is not used.
- 2026-10-18 — The fixed 240-minute linear scale is replaced. It left most users with near-empty cells.
- 2026-10-18 — The calendar heatmap shows absolute minutes, never a percentage of a personal target.
- 2026-10-18 — Today's Progress shows percentage of the user's personal daily target. It resets daily.
- 2026-10-18 — Today's Progress does not need to carry over into the calendar. The two surfaces answer different questions and may show different things for the same day.
- 2026-10-18 — Recorded days are never recomputed. A target change affects forward days only.
- 2026-10-18 — Programme periods (Mandala, Samyama) are not excluded or routed elsewhere. Their minutes render in the calendar like any other day.
- 2026-10-18 — One universal scale for all users ("Scale A"). No per-user denominator for the calendar.
- 2026-10-18 — 12 bands, single green hue, light to dark.
- 2026-10-18 — Two non-linearities: minutes map non-linearly to bands, and bands map non-linearly to shade (front-loaded, so low bands are clearly visible).
- 2026-10-18 — The darkest band (12) is reached only at 240 minutes and above.
- 2026-10-18 — No more than 12 bands. Extra bands would not be distinguishable on screen.
- 2026-10-18 — Presence of practice must read first; depth of shade is the second layer. Any coloured cell, however light, must read clearly as a day practised.
- 2026-10-18 — Over-target treatment on Today's Progress is parked, not part of this work.

## 4. How it works

### Surfaces and the question each answers
| Surface | User question | Unit | Horizon |
|---|---|---|---|
| Today's Progress | Am I doing today what I set out to do? | % of personal daily target | Today only, resets |
| Calendar heatmap | How has my practice evolved, and how much have I built? | Absolute minutes, banded | Whole history |
| Totals | How big is what I have built? | Total days, total minutes | Whole history |

### Calendar heatmap
1. Each day is one cell.
2. Cell colour = the band that day's total minutes falls into (section 6).
3. A day with 0 minutes uses the empty-cell treatment. Final treatment: Not decided.
4. A day before the user joined: treatment Not decided. Today it looks identical to a skipped day; it must not read as failure. Likely a start-date boundary — Not decided.
5. Past cells never change after the day is recorded.
6. Long periods of heavy practice (e.g. Samyama prep) simply render as the darkest stretch of the record. No special handling.

### Layout (prototype only — all Not decided, see open questions)
- Prototype renders 364 days per year as a grid of 7 rows (days) with weeks as columns, flowing left to right.
- Cells have 1px gaps and 1px corner radius.
- Prototype cell size is adjustable from 3px to 12px; default 5px.
- Prototype shows one row per year with total days and total hours beside it.
- Prototype uses a dark background.

### Today's Progress
- Shows percentage of the user's personal daily target.
- How the personal target is set: Not decided (see section 9, practice target).
- Caps at full bloom. Display of practice beyond the target: Not decided (parked).

## 5. Copy
No user-facing copy is final.

Prototype-only labels (not agreed as product copy):
- Year rows: "2024 — starting out", "2025 — settling in", "2026 — deepening"
- Legend ends: "none", "240+"
- Practitioner presets: "Beginner", "Steady", "Committed", "Deep"

The framing "starting out", "settling in", "deepening" was liked as a way to describe a user's evolving record. Whether it becomes product copy: Not decided.

Today's Progress tone: framed as striving towards what the user committed to, not as "compliance". Do not use the word "compliance" or audit-style language. Exact strings: Not decided.

## 6. Numbers and logic

### Band mapping (Scale A, final)
A day's total minutes `m` maps to a band:

| Band | Minutes | Shade |
|---|---|---|
| empty | 0 | Not decided |
| 1 | 1–5 | #C2EBDC |
| 2 | 6–10 | #9EE0C9 |
| 3 | 11–15 | #7CD5B6 |
| 4 | 16–20 | #5DCAA5 |
| 5 | 21–30 | #45BE96 |
| 6 | 31–45 | #31B188 |
| 7 | 46–60 | #22A47B |
| 8 | 61–95 | #17966F |
| 9 | 96–135 | #0F8764 |
| 10 | 136–180 | #0A7659 |
| 11 | 181–239 | #07634D |
| 12 | 240+ | #044034 |

Upper bounds, inclusive: `[5, 10, 15, 20, 30, 45, 60, 95, 135, 180, 239, ∞]`
