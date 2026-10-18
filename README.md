# NBA 9-Cat Checker

A player-value tool for our **ESPN fantasy basketball head-to-head league** (YTBBB), scoring **FG%, FT%, 3P%, REB, AST, A/TO, STL, BLK and PTS**. It is modeled on Hashtag Basketball's projections page.

- Ranks every player by z-score in each of the 9 categories, colored green (helps you) to red (hurts you). The categories are set in the `CATS` list in `index.html`.
- **Punt builds:** tap a category to switch it off and the rankings recalculate.
- **League settings:** number of teams and roster size set the player pool the values are measured against.
- **Per game / totals**, **2026-27 projections**, **2026-27 season so far** (once games start) and **2025-26 actual**.
- **My Team:** star (★) your players to see which categories you're strong in, which you need, and the best players to add for those needs.
- **Taken:** mark players other managers own (✕) so suggestions only show free agents.
- **CSV import** if you want to use your own projections.

Your starred and taken players are saved in your browser.

**Your roster** lives in `data/my-team.js` and is starred automatically on every device. To change it, edit the list and raise `version` by 1. Players you star or remove on the site are remembered in that browser until the version changes.

**Your league** lives in `data/league.js`: each manager's team name and the players they own. Their players show the team name in the table and are left out of pickup suggestions. In the **League** panel you can pick a team, add players to it, and once a team has 8+ players, see a head-to-head matchup preview against yours. Same rule: edit the file and raise `version` to push changes to every device.

## Where the data comes from

`scripts/fetch-espn.mjs` downloads ESPN Fantasy's own player projections and season stats.
The GitHub Action in `.github/workflows/update-data.yml` runs it **once a day** and saves the result to `data/players.js`, so the app stays current as the season goes on.

If `data/players.js` is missing, the app falls back to `data/seed-players.js`, a set of hand-made estimates.

## Using it

**Option 1: on your computer.** Download the repository (green **Code** button, then **Download ZIP**), unzip it, and double-click `index.html`.

**Option 2: as a website (recommended, free).**
1. Merge this branch into `main`.
2. On GitHub, open **Settings → Pages**, set **Source** to "Deploy from a branch", pick `main` and `/ (root)`, and save.
3. After a minute your site is live at `https://<your-username>.github.io/<repo-name>/`. Bookmark it on your phone.

To refresh the data by hand: **Actions → Update player data → Run workflow**.

## How values are calculated

For each category, a player's z-score is `(player − pool average) ÷ pool standard deviation`, where the pool is the top `teams × roster` players. FG%, FT%, 3P% and A/TO are team ratios, so they use volume-weighted impact (`makes − league% × attempts`, or `assists − league A/TO × turnovers`). **Value** is the sum of the categories you have switched on.
