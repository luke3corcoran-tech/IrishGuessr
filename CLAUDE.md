# IrishGuessr

Daily Irish trivia guessing game. Single static page (`index.html`) hosted on GitHub Pages from `main` / root. No build step, no backend.

## Game rules (decided)
- Everyone gets the same 3 answers per day, one from each genre, always in this order: Music, Geography, Sport.
- Every answer is something Irish.
- Each answer has 5 hints, from vaguest to most obvious. Hint 1 is shown at the start.
- Hint 5 also shows a letter pattern (first letter of each word, the rest as underscores).
- Unlimited guesses. Wrong guesses are free and don't reveal anything (they're listed, crossed out).
- Players tap "Next hint" only when they want one. On hint 5 that button becomes "Give up", which ends the answer with 0 points.
- One attempt per day. Progress is saved after every step, so refreshing doesn't reset a puzzle.

## Scoring
- `POINTS=[100,80,60,40,20]`: points for solving on hint 1–5. 300 max per day.

## Answer matching
- Case, fadas, punctuation, spaces and a leading "The" are ignored.
- Typos allowed: 1 edit for answers of 5–7 letters, 2 edits for 8+ letters, none under 5.
- Each puzzle has `a` (shown answer) and `alt` (other accepted answers: surnames, nicknames, Irish names).

## Puzzles
- `BANK` in `index.html`: an array of days, each holding `[music, geography, sport]`.
- Each entry: `{a, alt, h}` where `h` is 5 hints. Hints are written in the first person where it reads naturally ("I'm…", "We…"), and the last hint should make it gettable.
- Check facts carefully. Avoid answers with many equally valid spellings unless they're listed in `alt`.
- Puzzle number is days since 2026-11-26 (puzzle #1), using local midnight. It cycles through `BANK`.
- Only 8 sample days exist. Add more before sharing widely.

## State
- Kept in `localStorage` (key `irishguessr`) on each player's device: per-day progress, results, streak, best score.
- No shared leaderboard.

## Known limits
- Answers are visible in the page source.
