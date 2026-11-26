# Amble

Daily Irish trivia guessing game, called Amble (formerly IrishGuessr; the repo is still named IrishGuessr). Single static page (`index.html`) hosted on GitHub Pages from `main` / root. No build step, no backend.

## Game rules (decided)
- Everyone gets the same 3 answers per day, one from each genre, always in this order: Music, Geography, Sport.
- Every answer is something Irish.
- Each answer has 5 hints, from vaguest to most obvious. Hint 1 is shown at the start.
- Hint cards are colour-coded by `HEAT`: hint 1 green → lime → yellow → orange → hint 5 red, so players can see hints running out. The points meter uses the same colours.
- Hint 5 also shows a letter pattern (first letter of each word, the rest as underscores).
- Unlimited guesses. Each wrong guess costs 5 points and reveals nothing. Repeating a guess already tried is ignored (no cost).
- Every guess is listed as a coloured chip: red = wrong, amber = close (shares a real word with an answer, or is within a couple of typos of matching), green = correct (`closeness()`).
- Players tap "Next hint" only when they want one. On hint 5 that button becomes "Give up", which ends the answer with 0 points.
- One attempt per day. Progress is saved after every step, so refreshing doesn't reset a puzzle.

## Scoring
- `POINTS=[100,80,60,40,20]`: points for solving on hint 1–5. 300 max per day.
- `WRONG_PTS=5`: taken off for each wrong guess (close guesses count as wrong).
- `MIN_SOLVED=5`: a solved answer never scores below 5. Missing or giving up scores 0.
- The share grid adds `✕N` after a row when there were N wrong guesses.

## Answer matching (deliberately forgiving)
- Ignores case, fadas, punctuation, apostrophes, spacing and word order.
- Ignores filler words (the, a, an, and, of, co, county, st, saint) and treats place words as optional (river, lough, lake, island, mountain, mount, cliffs, bay, head…).
- Doubled letters count as single ("Carantoohil" = "Carrauntoohil").
- Typos allowed by answer length: 0 for ≤3 letters, 1 for 4–6, 2 for 7–10, 3 for 11+. Swapped neighbouring letters count as one typo.
- A guess that contains the full main answer (e.g. "the band thin lizzy") also counts. This doesn't apply to `alt` answers, so "Robbie Keane" doesn't match the surname alt "Keane".
- Each puzzle has `a` (shown answer) and `alt` (other accepted answers: surnames, nicknames, Irish names).

## Puzzles
- `BANK` in `index.html`: an array of days, each holding `[music, geography, sport]`.
- Each entry: `{a, alt, h}` where `h` is 5 hints. Hint 1 should be hard: an obscure but true fact that only a real fan would know. Hints are written in the first person where it reads naturally ("I'm…", "We…"), and the last hint should make it gettable.
- Check facts carefully. Avoid answers with many equally valid spellings unless they're listed in `alt`.
- Puzzle number is days since 2026-11-26 (puzzle #1), using local midnight. It cycles through `BANK`.
- 30 days exist (puzzles #1–30, 26 Nov – 25 Dec 2026). After that it cycles back to #1, so add more before then.
- Each entry also has `p:[lat, lon, label]`, which pins the answer (or a person's home town or base) on the map when it's revealed. Leave `p` out if there's no sensible place.

## Mobile
- The answer box isn't auto-focused on touch screens (`TOUCH`), so the keyboard only opens when the player taps it.
- Wrong guesses update the page in place (no re-render), so the keyboard stays open between guesses.
- When the box is focused on a phone, the newest hint scrolls to the top so it stays visible above the keyboard.
- The guess box sits directly under the hints; "Reveal hint" is below it.

## State
- Kept in `localStorage` (key `irishguessr`, kept from the old name so streaks survive the rename) on each player's device: per-day progress, results, streak, best score.
- No shared leaderboard.

## Known limits
- Answers are visible in the page source.
