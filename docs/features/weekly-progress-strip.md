# Weekly progress strip

## 1. Goal
Add a pinned view of the current week (Mon–Sun) above Today on the Sadhana Tracker, with one tap into all past weeks and the journey.

## 2. Status
Explored and decided in HTML prototypes (Claude artifacts). Not built in Preview. Not shipped.
Ships only after practice goals exist, because the daily goal is the fill denominator.
The entry point (one door vs two doors) is not decided; both versions are to be tested.

## 3. Decisions
- 2026-10-07: Add the week above Today; do not replace Today. Today stays as the secondary row.
- 2026-10-07: Build after practice goals; the goal is the denominator for each day's fill.
- 2026-10-07: One window per day, Monday to Sunday, in a linga shape (slim ellipsoid top, after the Dhyanalinga).
- 2026-10-07: Window height = share of the daily goal, capped at 100%. Window colour = minutes.
- 2026-10-07: Heat colour is teal only, never saffron. Teal shows minutes and nothing else.
- 2026-10-07: Day letter inside the window at the top; percentage inside at the bottom; on every day.
- 2026-10-07: A day at 100% gets a soft glow, small enough that two full days side by side don't merge.
- 2026-10-07: No practice = grey window. A day still to come = lighter grey, no stroke. Nothing is marked "missed".
- 2026-10-07: The strip sits on the page background (no card) and is sticky while the list scrolls.
- 2026-10-07: No trend indicator on the strip.
- 2026-10-07: The strip never switches the day. Only the day switcher changes Today/Yesterday.
- 2026-10-07: Tapping any past or current window opens a view-only day detail sheet.
- 2026-10-07: Time is shown in minutes only, never converted to hours.
- 2026-10-07: Cumulative totals (total days, total minutes, current streak) live on the profile, not on the tracker.
- 2026-10-07: The progress surface leads with the journey (stages of growth driven by minutes).
- 2026-10-07: Expanded history is a vertical scroll; a week is always a row, everywhere.
- 2026-10-07: Detailed history = this month and the two before it, as whole weeks; then one card per year.
- 2026-10-07: A week belongs to the month that holds its Thursday.
- 2026-10-07: Year cards show months newest first.
- 2026-10-07: The week-over-week change appears only in all-weeks rows, not on the strip.
- 2026-10-07: The settings gear replaces the bell in the tracker header (settings hold reminders).
- 2026-10-07: Dark mode follows the current app; the heat ramp is reversed (more minutes = brighter).
- 2026-10-07: Keep two entry versions for testing: "One door" and "Two doors" (see section 4).

## 4. How it works

### 4.1 Tracker screen (top to bottom)
1. Header: title "Sadhana Tracker", settings gear on the right.
2. Weekly strip (sticky, on page background):
   - Line 1: section label "This week" on the left; "{minutes} min · {days} days" right-aligned to the last window.
     Hidden when the week has no practice.
   - Line 2: seven windows (Mon–Sun) filling the width, then a round teal chevron button on the right with a label under it.
   - The left padding of the first window equals the gap between the last window and the button column.
   - Each window:
     - Fill height = share of goal (capped at 100%). Fill colour = minutes band.
     - Day letter at the top inside. Today's letter is teal.
     - Percentage at the bottom inside; hidden when the day has 0 minutes or is in the future.
     - Text turns light (light theme) or dark (dark theme) when the fill reaches it on a deep band.
   - Selected day (the day shown in the Today box): ring around its window.
   - Day at 100%: soft glow (combined with the ring if selected).
   - No practice: grey. Future day, or a day before the user's first practice: lighter grey, no stroke.
   - Tap a window (today or any past day) → day detail sheet (4.4). Future windows don't respond.
   - Tap the round button → opens the expanded view (4.2). Its chevron flips and its label becomes "Close" when open.
3. Today box (card, not sticky):
   - A notch on its top edge points at the selected window and moves with it.
   - Row 1: day switcher "‹ Today ›" / "‹ Yesterday ›". Two doors only: a quiet outlined "Overall progress ›" chip, right-aligned on the same row.
   - Row 2: "{n} practices" | "{m} minutes", left-aligned under the switcher, with a thin separator between them.
   - Yesterday state: the box border and the label turn teal; the ring and notch move to yesterday's window.
4. "My practices" section header (on background) with an edit icon, then a card listing the practices.
   - Ticking or unticking updates the selected day's window fill and percentage live, plus the Today numbers.

### 4.2 Expanded view (round button)
- Opens in place: it extends down from the strip, which stays pinned at the top with the chevron showing "Close".
- Close it with the round button or the floating "Close" button at the bottom.
- Content, top to bottom:
  1. Weekly rows, newest first, starting with last week (the current week is the pinned strip).
     - Each row uses the same windows as the strip, aligned to the strip's columns so the pinned day letters apply.
     - Rows have no letters or percentages; fill and colour only. 100% days glow.
     - Right column, three lines: "{minutes} min" (bold), "{days} days", then the change chip "▲ {d}%" or "▼ {d}%".
       Minutes and days are hidden for weeks with no practice; the chip is hidden when no change is shown.
     - A short month label (e.g. "SEP") appears above the first week of each month (month of the week's Thursday).
       If that month is in another year, the year is added.
     - Tapping a window opens the day detail sheet.
     - Range: all weeks whose Thursday falls in this month or the two before it.
  2. Year cards, newest year first. Each card:
     - Header: "{YYYY}" plus "{n} days · {minutes} min".
     - Grid of small month calendars (4 per row), months newest first (Dec → Jan), each labelled with its short month name.
     - Calendars have 7 columns (Mon–Sun) and weeks as rows; cells are coloured by minutes band.
     - Only months before the detailed range, and on or after the user's first practice, are shown.
     - Seam rule: the newest calendar (the month just before the detailed range) extends its last row into the
       next month's days up to the day before the first detailed week. Those extra days are shown lighter.
  3. Legend: "Less" [12 band swatches] "More".
  4. One door only: a pinned journey bar at the bottom, next to "Close": sapling icon, "Your journey · Stage {k}", "{x} min to Stage {k+1}".
- Empty state (no earlier weeks): "Earlier weeks show up here as you practice."

### 4.3 Entry versions to test
- One door: round button label "Overall progress" (two lines). It opens 4.2 with the journey bar. No chip in the Today box.
- Two doors: round button label "All weeks" (two lines). It opens 4.2 without the journey bar.
  The "Overall progress" chip in the Today box opens a separate Overall progress screen (4.5).
  Both doors sit in the right-hand column: the round button above, the chip below.

### 4.4 Day detail sheet (bottom sheet, view only)
- Title: "Today", "Yesterday", or the weekday name (e.g. "Wednesday"); the full date below (e.g. "7 October 2026").
- Close button (×); tapping the scrim also closes it.
- Stats row: "{n} practices" | "{m} minutes" | "{p}% of daily goal" ("–" when 0 minutes).
- Grid of the practices done that day (illustration, name, "{x} min"), 4 per row.
- Empty day: "No practice recorded."
- Footer note with an eye icon:
  - Today: "View only. To change today, tick in My practices."
  - Yesterday: "View only. To change yesterday, switch to Yesterday and tick in My practices."
  - Older days: "View only."
- Nothing can be ticked or unticked in the sheet.

### 4.5 Overall progress screen (two doors only)
- Back arrow and title "Overall progress".
- Journey hero card: large sapling illustration, "Your journey", "Stage {k}" (plus " · {name}" where a name exists),
  "{total} min practiced · {x} min to Stage {k+1}", and a progress bar to the next stage.
- Section "So far": three stat cards, "total days", "total minutes", "current streak".
  Not decided whether these stay here or live on the profile only (see open questions).
- Placeholder box for future views.
- This screen never shows the weeks visual.

## 5. Copy
Copied from the final prototype. Strings marked (placeholder) were not agreed as final copy.

Tracker header
- "Sadhana Tracker"

Weekly strip
- "This week"
- "{minutes} min · {days} days" (singular: "1 day")
- Round button label, one door: "Overall progress" (shown on two lines: "Overall" / "progress")
- Round button label, two doors: "All weeks" (two lines: "All" / "weeks")
- Round button label when open: "Close"
- Window contents: day letters "M" "T" "W" "T" "F" "S" "S"; percentage "{p}%"

Today box
- "Today" / "Yesterday" (displayed in uppercase)
- "{n} practices" (singular "practice"), "{m} minutes"
- Two doors chip: "Overall progress"

Practices list
- "My practices" (section header, displayed in uppercase)
- Row duration: "{x} mins"

Expanded view
- Week row: "{minutes} min", "{days} days" (singular "1 day"), "▲ {d}%", "▼ {d}%"
- Month label: "{MON}", e.g. "SEP"; with the year when it's not the current year, e.g. "DEC 2025"
- Year card: "{YYYY}", "{n} days · {minutes} min"
- Month labels in year cards: "Jan" … "Dec"
- Legend: "Less", "More"
- Empty: "Earlier weeks show up here as you practice."
- Floating button: "Close"
- One door journey bar: "Your journey · Stage {k}", "{x} min to Stage {k+1}"

Day detail sheet
- "Today" / "Yesterday" / weekday name; date "{d} {Month} {YYYY}"
- "{n} practices" (singular "practice"), "{m} minutes", "of daily goal"
- "{x} min" under each practice
- "No practice recorded."
- "View only. To change today, tick in My practices."
- "View only. To change yesterday, switch to Yesterday and tick in My practices."
- "View only."

Overall progress screen (two doors)
- "Overall progress"
- "Your journey"
- "Stage {k}"; stage names known so far: "Seed" (Stage 1), "First Leaves" (Stage 7)
- "{total} min practiced · {x} min to Stage {k+1}"
- "So far"
- "total days", "total minutes", "current streak"
- (placeholder) "Per practice, this year, and other views to explore here."

## 6. Numbers and logic
- Week: Monday to Sunday.
- Daily goal: the denominator for fill. Its source is Not decided; it comes from the practice goals feature.
  The prototype used 60 min as a stand-in.
- Fill % = min(100, round(day_minutes / daily_goal × 100)).
- Full day (glow) = fill % ≥ 100.
- Colour band = ceil(day_minutes / 20), clamped 1–12 (12 bands of 20 min, capped at 240 min). 0 minutes = grey.
- Teal ramps from the prototype:
  - Light: #C2EBDC #9EE0C9 #7CD5B6 #5DCAA5 #45BE96 #31B188 #22A47B #17966F #0F8764 #0A7659 #07634D #044034
  - Dark: #123A33 #144439 #164F42 #185B4A #1A6853 #1C765C #1F8566 #259571 #2FA67E #3DB78D #4FC89D #66D9B0
- This week minutes = sum of minutes Mon–today. Days = count of days with minutes > 0, Mon–today.
- Practices count for a day = number of practices completed that day.
- Week row change chip:
  - d = round((this_week_minutes − previous_week_minutes) / previous_week_minutes × 100), using full weeks.
  - Shown only when |d| ≥ 5, and only when both weeks have minutes and the previous week is after the user's first practice.
- Month of a week = the month of its Thursday.
- Detailed range: weeks whose Thursday is on or after the 1st of the month two months before the current month,
  excluding the current week.
- Year cards: months before the detailed range, newest first, starting from the user's first practice.
  The seam extension follows 4.2.
- Journey stage threshold (placeholder, fitted to the journey reference): T(k) = round(18 × (k − 1)^2.2) minutes.
  The stage is the highest k with T(k) ≤ total minutes. Progress bar = (total − T(k)) / (T(k+1) − T(k)).
- Current streak (two doors screen): consecutive days with minutes > 0, counting back from today
  (from yesterday if today has 0).
- Units: minutes everywhere, with thousands separators.
- Day detail and history are view only; only today and yesterday can be edited, through the switcher and My practices.

## 7. Data and events
- Uses per-day practice completions and minutes (existing tracking data), including yesterday edits via the switcher.
- Needs the user's daily practice goal (from the practice goals feature).
- Needs the date of the user's first practice (start of history).
- Events and analytics: Not decided. The stated test question is which door users open (one door vs two doors)
  and what they do next.

## 8. Rejected options
- Week strip in its own card above the switcher (V1): too tall; the practice list fell below the fold.
- Tapping a window to switch day: only today and yesterday could respond, so 5 of 7 windows would ignore a tap.
- Trend arrow vs last week on the strip: turns every week into a race nobody wins; it moved to week rows.
- Trend comparing a part-week with a full last week: always read down (fixed, then the strip trend was removed).
- Eight-week mini bar trend in the header: repeated the windows and was misread as months.
- Sentence about the week on the strip: repeated the numbers in words; parked until copy rules exist.
- "Goals met" count in the week summary: becomes a weekly scorecard of misses.
- Saffron heat colour: teal only.
- Dashed outlines for future days: read as six boxes waiting to be filled.
- White windows with a dark edge: dropped to keep the grey look.
- Shrinking the strip to thin bars on scroll: looked wrong; not needed once letter and percent moved inside.
- Month label in front of the strip: took width from the windows.
- Hours or hours-and-minutes units: the journey and practices run on minutes; mixed units force conversion.
- Tabs inside the expanded view (Weeks / Totals / Practices): too many choices at once.
- Cumulative totals on the first fold of the expanded view: not what users want to see daily; moved to the profile.
- "See all progress" link inside the expanded overall progress: two "progress" labels in the same view.
- Horizontal swipe history: turns weeks into columns and breaks at month and year compression.
- Fixed 12 detailed weeks: cuts the oldest month in two.
- Month blocks with split weeks: rows stagger and split weeks can't show their numbers.
- Dated rows without month names (option D): too much text on the right.
- Weeks all the way back with no year cards (option E): history becomes endless.
- Small heat-cell rows for weeks: too abstract; lost the fill.
- Year cards with months across and days down: weeks become columns.
- A separate month-card tier between weeks and years: folded into the year cards.
- Two-line week summary with the chip beside it: pushed the windows out of line with the day letters.
- Overall progress door inline in the Today row next to the numbers: crowded and breaks in longer languages.
- Plant or flower metaphor for today's progress: reserved for the journey.

## 9. Open questions
1. Entry point: one door or two doors? Not decided; test both.
2. Daily goal source and definition (fixed, total of My practices, or self-set): Not decided. Depends on practice goals.
3. Streak: removed from the tracker, kept on the profile. Is the "So far" stat block on the two-doors
   Overall progress screen kept or moved to the profile only? Not decided.
4. Day detail: add a shortcut from yesterday's sheet that switches the tracker to Yesterday? Not decided (left out for now).
5. Journey card or bar tap target before the full journey screen exists: inert, or a door to a placeholder? Not decided.
6. Journey stage thresholds and stage names beyond "Seed" and "First Leaves": Not decided (placeholder formula).
7. Profile heatmap: replace the planned horizontal week-columns swipe with month calendars that keep weeks as rows? Not decided.
8. Contrast of grey empty windows on the page background (below WCAG 3:1 non-text contrast): Not decided; flagged for design.
9. Selected-day ring colour in dark mode (teal ring on teal fills): Not decided.
10. Window proportion against an actual Dhyanalinga photo (only its height, 13 ft 9 in, is published): to verify.
11. Animation or emphasis when a ticked practice raises a window, and a full-week moment: Not decided.
12. Copy rules for a sentence about the week: Not decided (parked).
13. Number formatting locale for thousands separators: Not decided (the prototype used en-IN).

## 10. Links
- Decision summary and final prototype (dark; one door / two doors; New, On and off, Deep):
  https://claude.ai/artifact/WR6jh67jspT7zAxrYtn4W1
- Prototype, one door or two (with history options C/D/E): https://claude.ai/artifact/M5ioGZe9NJkF5HQwjvokAA
- Prototype, iteration seven (day detail, journey, minutes): https://claude.ai/artifact/5kXdjZDUZ2VWNumq5Hg7um
- All weeks — weeks against months, and units: https://claude.ai/artifact/T47phAzvQnhbnPhpGmATvU
- Progress entry point — options and critique: https://claude.ai/artifact/AADYtPUGhLnYxbJvV2WRpZ
- Weekly strip — emotional and usability reviews: https://claude.ai/artifact/XWBaJGzPNnqDiDzHiwZEiw
- Compression options reference (year card vs monthly bars): https://claude.ai/artifact/7UZX6sqZssm2XKE1TefoPd
- Journey reference (Practices — states and flows v2): https://claude.ai/artifact/Q8akaeJWiDQ7huPJYwmp5q
- Practice progress survey findings: https://claude.ai/artifact/1DqjrqbXPmpaUVgVAUrhnW
- FigJam exploration record, Practices board, page "Weekly Progress Strip":
  https://www.figma.com/board/1cb2UHcbOo4zncG9VV9IMq/?node-id=1910-340
- Drive: none. Slack: none.
