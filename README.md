# ilkeryayla.com

A one-page personal site: a daily scoreboard for Wordle, Connections and Sudoku.
It is plain static files with no build step.

## Files

| File | What it is |
|---|---|
| `index.html` | The page: markup, styles and script in one file |
| `log.json` | The data. Every number on the page is computed from it |
| `meadow.jpg` | The background photo |
| `bricolage-grotesque.woff2` | The typeface, self-hosted (SIL Open Font License 1.1) |

## Adding a day

Add one entry to `days` in `log.json` and push:

```json
{ "date": "2026-10-09", "wordle": 4, "connections": 1, "sudoku": "10:58" }
```

- `wordle`: guesses, 1 to 6, or `"X"` for a miss
- `connections`: mistakes, 0 to 3, or `"X"` for a loss
- `sudoku`: solve time as `"m:ss"`
- Leave a game out on a day you did not play it.
- Entries are separated by commas, and the last one has no trailing comma.

The headline, charts, averages and streaks update on their own.

## The baseline

`baseline` holds the totals from before daily logging started, copied from the NYT Games app.
`asOf` is the last date those totals include. Daily entries should start the day after,
otherwise a game is counted twice or the carried-in streak resets.

Streaks are strict, as in the games: a loss, a skipped day, or a last entry older
than yesterday resets the current streak to 0.

`sudokuDifficulty` is only the word used in the text ("the hard Sudoku").

## Things to edit in index.html

- **Links:** the About section has a commented-out block with GitHub and Email links.
  Fill in the addresses and remove the `<!--` and `-->` lines around it.
- **About text:** one paragraph in the About section.
- **Colours:** the variables at the top of the `<style>` block, once for light and once for dark.

## How the page behaves

- **Theme:** the page follows the visitor's system setting. The moon/sun button in the
  header overrides it, and that choice is remembered in the visitor's browser until they
  toggle back to what their system uses.
- **Glass:** in Chrome and Edge the panels bend the photo near their edges. Safari and
  Firefox show plain blurred glass instead. To use plain glass everywhere, set
  `REFRACTION` to `false` at the top of the first script in `index.html`.

## Preview locally

The page loads `log.json` with a request, so it has to be served, not opened from disk:

```sh
python3 -m http.server
```

Then open http://localhost:8000.

## Deploy on Cloudflare Pages

1. Push this folder to a GitHub repository.
2. In the Cloudflare dashboard open **Workers & Pages**, select **Create application**,
   and choose the option to connect a Git repository. Pick this repo.
3. Build settings: no framework preset, leave the build command empty, and set the
   output directory to the repository root (`/`).
4. Deploy. The site appears on a `*.pages.dev` address.
5. In the project open **Custom domains**, select **Set up a domain**, and add
   `ilkeryayla.com`. Add `www.ilkeryayla.com` the same way if wanted.

After that, every push redeploys the site.
