# Practice Sessions (back-to-back practice)

## 1. Goal

Let a user set up a run of practices once, then practice through them back to back without touching the phone between practices.

## 2. Status

**Explored.** Decisions below are settled at exploration level. Nothing built. Not yet scoped — scope step is next, so treat this as direction plus settled rules, not an approved build list.

## 3. Decisions

All dated 2026-10-18 (single exploration session).

- 2026-10-18 — A session is a run through the player; a single guided or timed practice is a session of one.
- 2026-10-18 — Marking a practice complete on the tracker is NOT a session and never becomes one.
- 2026-10-18 — One player serves guided audio, timed countdown and self-directed practice; the existing separate timer player is retired.
- 2026-10-18 — Session setup is ONE screen. No separate preview/confirm step.
- 2026-10-18 — Default selection: first session ever = whole practice list in ideal sequence; every session after = the selection from the last session set up in the same time-of-day window.
- 2026-10-18 — Time-of-day window is determined at setup time, not completion time.
- 2026-10-18 — Durations are never asked for when the app can detect them. Only timed practices with an adjustable duration get an editable chip.
- 2026-10-18 — Internal taxonomy (guided/timed/unguided) is not exposed. Rows show either "Guided" or "On your own".
- 2026-10-18 — Add action pattern for selection, not checkboxes (checkbox on tracker means "mark complete" and is irreversible).
- 2026-10-18 — One instance of a practice per session. Sessions hold practices, not slots.
- 2026-10-18 — On completion, the first uncompleted instance of that practice on the tracker is marked.
- 2026-10-18 — Drag is for reordering within the session only, with a six-dot handle.
- 2026-10-18 — Player is one persistent frame; only the centre illustration and practice name change between practices.
- 2026-10-18 — Player is forward-only. No back, no scrubbing. Completion is irreversible.
- 2026-10-18 — Complete is press-and-hold with the button filling left to right; haptic and advance only on full fill.
- 2026-10-18 — Skip is press-and-hold (~1.5s). Quit is tap → confirmation modal. Pause is a plain tap, low visual weight.
- 2026-10-18 — Timer shows minutes only, both directions. No seconds, no progress bar, no sub-minute indicator of any kind.
- 2026-10-18 — Elapsed time is derived from a stored session start timestamp, not accumulated from a tick.
- 2026-10-18 — The 31-second completion heuristic is retired.
- 2026-10-18 — Completion rule differs by type: guided all-or-nothing; timed logs minutes rounded down; self-directed recorded on Complete.
- 2026-10-18 — Backgrounding never pauses anything. Only the pause control pauses.
- 2026-10-18 — Duration caps are set per practice. No formula.
- 2026-10-18 — Return window: end of the practice day it started in, or four hours, whichever comes first.
- 2026-10-18 — Guided audio failure holds the session in a persistent error state; it does not auto-skip and does not end the session.
- 2026-10-18 — Anything already completed is kept on every exit path.
- 2026-10-18 — Five silent seconds between practices.
- 2026-10-18 — Success screen lists completed practices individually, then the session total, then the existing bloom animation once at session level. Skipped practices are not shown.

## 4. How it works

### 4.1 Entry

Primary CTA on the practice list starts a session. The per-practice play button stays — playing a single practice opens the same player as a session of one.

### 4.2 Session builder (one screen)

Opens pre-populated. The user edits by subtraction.

Each row carries:
- Six-dot drag handle (reorder)
- Practice illustration
- Practice name
- Guided practices: `Guided · [n] min` as plain text, not editable
- Self-directed practices: `On your own`, no duration shown
- Timed practices with an adjustable duration: a tappable duration chip showing the current value
- Remove control (×)

Below the list: an "Add a practice" row. Tapping it opens a sheet listing only practices **not already in the session**, each with an Add button on the right. The row itself is not tappable.

Sticky footer: practice count and estimated total, directly above the CTA. Both update live as the selection changes.

**Default selection logic**
- No prior session: the entire practice list, in ideal sequence.
- Otherwise: the practice selection from the most recent session **set up** in the same time-of-day window as now.

**Time-of-day windows** (from the old Sadhana Tracker spec; never shown to the user)
- Morning: 12:00 AM – 10:59 AM
- Afternoon: 11:00 AM – 3:59 PM
- Evening: 4:00 PM – 8:59 PM
- Night: 9:00 PM – 11:59 PM

### 4.3 Player

One persistent frame for all practices. Between practices only the centre illustration and the practice name change. Five silent seconds of stillness between practices — not a countdown.

Layout:
- Top left: quit (×)
- Top right: skip, hold to activate
- Centre: position indicator, illustration, practice name, timer
- Bottom left: pause (small, low prominence, in the Miracle of Mind idiom)
- Bottom right: Complete, hold to activate — **self-directed practices only**

Timer behaviour by type:
- Guided: counts down. Completes when the audio track ends, then auto-advances after the 5s gap.
- Timed: counts down. Completes at zero, then auto-advances after the 5s gap.
- Self-directed: counts up. Completes only when the user holds Complete.

The displayed number never rests at zero — a countdown floors at `1 min left` until completion fires.

### 4.4 Interruptions and exits

- **Pause** — stops the timer and audio. Resume by tapping again. Reversible, no consequence.
- **Skip** — hold ~1.5s. Current practice not recorded. Moves to next, or to success screen if it was the last.
- **Quit** — tap × → confirmation modal. Current practice follows its type rule. Everything already completed is kept. Routes to the success screen if anything was completed.
- **Force-quit / app closed** — guided discarded, timed logged, self-directed per the recovery decision (Not decided, §9.1).
- **Past the return window** — session closes. Completed practices kept.
- **Backgrounding / screen lock** — nothing pauses. Audio continues, countdowns continue, count-ups continue. Caps and thresholds still apply.

### 4.5 Guided audio failure mid-session

Reuse the existing Mandala guided-audio pattern: loading state at 2 seconds, persistent in-player error at 5 seconds, player stays open, Retry available. Add a skip-to-next action alongside Retry.

The session **holds**. It does not auto-advance (the user's eyes are closed and they would start the wrong practice unknowingly) and it does not end.

### 4.6 Count-up cap

When a self-directed count-up reaches that practice's cap, the timer freezes and a prompt asks whether the user is still practicing. Exact copy: Not decided.

### 4.7 Success screen

Reached after completing or skipping the last practice, or after quitting with at least one practice completed.

- Each completed practice listed with illustration, name and minutes
- Session total below
- The existing Today's Progress animation sequence (fraction → minutes → flower bloom) runs **once**, at session level
- Skipped practices are not shown

The preview build currently has only a minimal post-practice screen (still image, Skip, Continue) with no stats. This screen is being added, built to match what ships on the live app.

## 5. Copy

Exact strings agreed in exploration. Anything not listed is Not decided.

### Session builder
- Header: `"Your session"`
- Self-directed row label: `"On your own"`
- Guided row label: `"Guided · [n] min"`
- Add row: `"Add a practice"`
- Footer summary: `"[n] practices · about [x] min"`
- Primary CTA: `"Start session"`

### Player
- Position indicator: `"PRACTICE [n] OF [total]"`
- Skip control: `"Hold to skip"`
- Complete control: `"Hold to complete"`
- Countdown timer: `"[n] min left"`
- Countdown final minute: `"1 min left"`
- Count-up timer: `"[n] min"`

### Guided audio error
- Retry action: `"Retry"`
- Skip action: `"Skip to next practice"`

### Not decided (copy)
- Quit confirmation modal — title, body, button labels
- Cap prompt ("are you still practicing") — full copy
- Duration chip sheet — title and labels
- Success screen heading and total label
- Any recovery-prompt copy (see §9.1)

**Copy constraint:** micro-copy must be terse and must never use "we", "us" or "our". Any draft carried forward from exploration notes that uses those pronouns needs rewriting.

## 6. Numbers and logic

### Rounding
- All durations round **down** to the full minute.
- Displayed number always equals logged number (a consequence of minutes-only display).

### Completion
| Type | Counts when | Logged |
|---|---|---|
| Guided | Audio track completes | Full practice, all-or-nothing |
| Timed | Practice ends, however it ends | Minutes practiced, rounded down |
| Self-directed | User holds Complete | Minutes counted, rounded down |

Principle: a practice with a defined arc counts only when the arc completes; a practice that is only a duration counts the duration.

### Caps (self-directed count-up, minutes)
| Practice | Cap |
|---|---|
| Yoga Asanas | 120 |
| Surya Kriya | 120 |
| Surya Shakti | 120 |
| Angamardana | 60 |
| Bhuta Shuddhi | 20 |
| Shakti Chalana Kriya | 120 |
| Shambhavi Mahamudra Kriya | 30 |

Caps for the rest of the catalogue: Not decided.

### Minimum thresholds (self-directed)
Each self-directed practice needs a minimum below which an abandoned practice is not recoverable — a practice that cannot plausibly have been done in that time was not done. Example discussed: Yoga Asanas under 20 minutes.

Actual values: **Not decided** for all practices.

The threshold governs **only** the abandonment-recovery path. An explicit Complete always logs, even below the threshold.

### Return window
`min(end of practice day the session started in, start time + 4 hours)`

### Timing constants
- Gap between practices: 5 seconds, silent
- Skip hold: ~1.5 seconds
- Complete hold: duration Not decided; button fills left to right over the hold, haptic on full fill only
- Guided audio loading state: 2 seconds
- Guided audio persistent error: 5 seconds

### Practice default durations referenced
Used for the session estimate, not displayed on self-directed rows.
- Shambhavi Mahamudra Kriya: 21 min
- Samyama: 30 min (adjustable)
- Shakti Chalana Kriya: 45 min
- Shoonya: 15 min, fixed and not adjustable
- Surya Kriya: 12 or 15 min — **not confirmed** (see §9.4)

A self-directed practice takes however long it takes. V1 uses a set default purely to produce the session estimate, because nothing has been measured yet. Once sessions are logging real durations, the estimate should come from the user's own history.

## 7. Data and events

### Must be stored
- Session start timestamp. Elapsed time is **derived from this**, not accumulated from a tick, so the timer cannot silently freeze or drift and recovers correctly after background, lock or relaunch.
- Per-practice start time within the session.
- Session composition (ordered list of practices) — needed for the next session's default.
- Time-of-day window the session was **set up** in — needed for the default lookup.
- Completed practices and their logged durations.

### Known behaviour to change in the preview build
- Backgrounding currently pauses the player and leaves it paused until manually resumed. This must be removed.
- The preview build currently records nothing on leave ("nothing counts until the practice is complete"). This becomes type-dependent per §6.
- The live app currently rounds down and logs partial timed practices on early exit. That behaviour is retained for timed.

### Analytics events
**Not decided.** No events were defined in this exploration.

## 8. Rejected options

- **Select-then-preview, two screens** — once the preview gained inline add and remove, both screens did the same job.
- **Drag-and-drop as the selection mechanism** — slow on long lists, poor primary affordance, destroys ideal sequence on the way in.
- **Checkboxes for session selection** — visually confusable with the tracker's "mark complete" checkbox, which is irreversible.
- **Showing a disabled duplicate row in the add sheet** — a greyed row in an add-only list teaches nothing; omit it instead.
- **Empty default selection** — makes the user build from scratch every day.
- **Whole list as the default every time** — punishes practice shopping; twelve on the list and four run means unselecting eight daily.
- **"Last session used" as the default, ignoring time of day** — wrong for anyone with different morning and evening routines.
- **Saved sessions (up to three named routines)** — the time-of-day default delivers most of it without asking the user to name or manage anything. Revisit if the default proves wrong often.
- **Restore-ideal-sequence control** — only needed once a reorder persists, which is the same moment sessions become saved.
- **Complete as tap + confirmation sheet showing the duration** — nothing to confirm (a count-up cannot exceed elapsed time, only adjust downward), adds a decision at the end of a practice, and two taps instead of one gesture. The hold also reads as commitment.
- **Screen replacement between practices** — a visual jolt at the moment the session exists to protect.
- **Universal player (Sadhguru's photo and lamp) in V1** — answers a different set of opportunities; deserves its own exploration. The persistent frame makes it a later swap of the centre visual.
- **Session position strip (previous / current / next illustrations, tap to hide)** — costs significant visual space; belongs with the universal player.
- **Seconds on the timer** — turns a practice into something being timed; invites waiting out the rounding.
- **Sub-minute progress line or bar** — a bar visibly approaching full is a stronger invitation to wait for another minute than a seconds counter, and it is not a pattern users would recognise.
- **Session-level total duration timer** — ruled out, not deferred. A second running number makes the screen technical.
- **One completion rule across all three practice types** — the types measure different things.
- **Cap formula (2× default duration, floor 30 min)** — cycle counts vary enough between users that any multiple of a default is wrong for a large share.
- **Ending the session on guided audio failure** — disproportionate for a recoverable network fault.
- **Auto-skipping on guided audio failure** — the user's eyes are closed; they would start the wrong practice without knowing.
- **Discarding the session on quit** — the user has practiced, just less than planned.
- **Overlapping or stacked illustrations on the success screen** — readable at two practices, unreadable at four, and they obscure that each practice was individually recorded.
- **Running the bloom animation once per practice** — four replays would drain it.
- **Showing skipped practices on the success screen** — contradicts the job of a screen that acknowledges effort.

## 9. Open questions

1. **Abandoned self-directed practice recovery.** Deferred to spec. A user practices, finishes, never picks the phone back up to hold Complete (app swiped away, battery dies, walks off). Three options explored: (a) discard, consistent with guided but loses real practice; (b) log silently above the threshold, which teaches that Complete is optional and records time that may not have been practice; (c) ask on return — lightest as a state on the practice's own tracker row rather than a modal, but asks the user to vouch for something recalled hours later, and becomes furniture if frequent. Choice depends on how often it actually happens, which the preview build will show.
2. **Minimum thresholds** for every self-directed practice, and **caps** for the rest of the catalogue beyond the seven listed.
3. **Background feasibility.** Engineering confirmation that timers and audio can run for the full cap duration while backgrounded, on both platforms.
4. **Surya Kriya default duration** — 12 or 15 minutes, unconfirmed.
5. **Should self-directed rows show an estimated duration?** Currently they show only "On your own", but the footer total is built from per-practice defaults. Worth settling at scope whether `"On your own · ~45 min"` is better.
6. **Complete hold duration** — not specified.
7. **Copy** for the quit modal, cap prompt, duration sheet and success screen (see §5).
8. **Analytics events** — none defined.
9. **DND prompt** — designed and spec'd on the live app, absent from the preview build. Out of this build; worth bringing across since the work is done.
10. **Sunday Guided Shambhavi inside sessions** — same routing logic without the discovery-card machinery. Separate iteration.

## 10. Links

- Exploration document (canonical, always current): https://claude.ai/artifact/HVRdExDXgH8XE1GUsL8dbk
- FigJam decision log, "Practice Sessions" page: https://www.figma.com/board/1cb2UHcbOo4zncG9VV9IMq?node-id=1874-340
- Drive folder for exploration PDF snapshots: https://drive.google.com/drive/folders/1j7gCBwkgO048aKmuZtTb6_ZoSeOw2YFc — no PDF generated yet
- Slack: none
- Related existing FigJam page, possibly overlapping territory for the universal player iteration: "Sadhana Space / Session Player" (backlog section of the same board)
