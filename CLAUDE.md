# Sadhana Tracker Preview

PWA tested with real users. Features are explored in the product-os repo; this repo builds them only when a feature carries a learning goal that real use can answer.

## Releasing
- `main` deploys to production on Vercel, used by study participants.
- Small changes go straight to `main`, with the build and tests passing first.
- Risky or big changes, such as login, data, sync or reminders, go to a branch first. Swami checks its Vercel test link and says "ship" before it's merged.
- Commit as Swami Gana `297094440+swamigana-hub@users.noreply.github.com`. On the Vercel Hobby plan, only the project owner's commits deploy.
- Supabase belongs to Augusto's account. Don't change the schema or edge functions without Swami's yes.

## Build briefs
- A feature's exploration lives in `product-os/features/<name>/`. When it goes to Preview, Swami pastes or hands over the brief, and it's saved as `docs/briefs/<name>.md`.
- Each brief names its learning goal. Build the smallest version that answers it.
- Copy is exact. Where a brief has no agreed string, leave the element blank rather than invent one.
- Results from Preview go back into the feature's STATE.md in product-os.
