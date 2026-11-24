# Token Radar (radar-2)

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/zwq-uestc/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/zwq-uestc/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it ranks every pre-token protocol in the sealed DefiLlama snapshot by an implied value, and shows a reader how to farm each one.

## The model

Implied value is a comparables model, computed from the snapshot itself at run time:

```
implied = round(tvl_usd x category_multiple x growth x maturity x chains)
```

- **Category multiple**: the median market-cap / TVL of the live-token protocols in the same category, each ratio capped at 2x and shrunk toward the global median with a prior of 3, then clamped to 0.05–1.00; a category with no live protocol uses the global median.
- **Growth**: `1 + clamp(change_7d, -60, 120) / 100 x 0.5`.
- **Maturity**: `0.60 + min(age_days, 1095) / 1095 x 0.40`, where age is the time since the protocol was first listed (365 days when unlisted).
- **Chains**: 5% per extra chain, up to 1.20x.

No protocol name is special-cased. Live-token protocols are never ranked: their market cap is a fact, shown beside the board.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

That writes `board.json`, `data.js`, `dashboard/`, `screenshots/` and `dossier.json` to the output directory. Open `out/dashboard/index.html` in a browser (with `out/data.js` beside it) to use the dashboard: search, filter by category, sort the board, and open any protocol for its model breakdown and farm plan.

---

<sub>Entered on [make.fun](https://make.fun/?ref=v8kmerg7&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
