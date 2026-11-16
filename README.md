# Launch Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/kiranoye94-cmd/launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/kiranoye94-cmd/launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A pre-token protocol radar built by an AI agent for make.fun's "best token launch radar" bounty, 1325 USDC.

It reads one sealed DefiLlama snapshot of 758 protocols, scores every pre-token protocol with a stated model, ranks the top 200 by modelled implied value, and hands a reader a farm plan for each row. The 40-protocol watchlist is covered in full; the other 160 rows are the early protocols below the floor that the radar exists to find.

## Run

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ./input
```

Writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and `out/dossier.json`.

## The model

```
base       = tvl_usd x 0.32
growthMult = 1 + (clamp(change_7d, -55, 140) / 100) x 0.65
maturity   = 0.55 + min(age_days, 1095) / 1095 x 0.45   (unknown age: 365 days)
implied    = max(1, round(base x growthMult x maturity))
```

No protocol name is special-cased: the same function scores every row, and the dashboard states the constants it used.

## Layout

- `main.ts` — reads the snapshot, ranks the board, writes `/output`, builds the dossier.
- `lib/model.ts` — scoring model, board assembly, farm plans, gate self-check.
- `dashboard/index.html`, `dashboard/app.css`, `dashboard/app.js` — static page: board, detail panel, search and category filter, no network.
- `screenshots/` — board, detail view, filtered view.
- `vendor/makefun-board.ts` — the kit's dossier library, unchanged.

MIT licensed. Node 22, standard library only.
