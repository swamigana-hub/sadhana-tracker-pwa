# Sadhana Tracker Preview

PWA tested with real users. Swami explores and specs features for the Sadhguru App in the product-os repo; this repo adapts them to the PWA.

## Releasing
- `main` deploys to production on Vercel, used by study participants.
- Small changes go straight to `main`, with the build and tests passing first.
- Risky or big changes, such as login, data, sync or reminders, go to a branch first. Swami checks its Vercel test link and says "ship" before it's merged.
- Commit as Swami Gana `297094440+swamigana-hub@users.noreply.github.com`. On the Vercel Hobby plan, only the project owner's commits deploy.
- Supabase belongs to Augusto's account. Don't change the schema or edge functions without Swami's yes.

## Feature context
Read the file for a feature before building it. Each holds decisions, copy, logic, rejected options and open questions.

| Feature | File | Status |
|---|---|---|
| Weekly progress strip | `docs/features/weekly-progress-strip.md` | Explored, not built |
| Practice sessions | `docs/features/practice-sessions.md` | Explored, not built |
| Overall progress heatmap | `docs/features/overall-progress-heatmap.md` | Explored, not built |

The strip and the heatmap overlap: the strip's minutes bands are 12 linear bands of 20 minutes, dated 7 Oct; the heatmap's Scale A uses non-linear bands with the same 12 colours, dated 18 Oct. Ask Swami which applies before building either.

Copy is exact. Where a file has no agreed string, leave the element blank rather than invent one.
