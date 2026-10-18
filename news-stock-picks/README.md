# Headlines to Holdings

A single-page stock screener that turns the week's top U.S. world, business and tech headlines into a ranked list of stocks to consider. It weighs each stock's 52-week price history, its latest earnings and historical base rates, and you can adjust how much each factor counts.

`index.html` is the whole app: data, scoring model and UI in one file. It is published as a claude.ai Artifact. Open the file in a browser and everything works except the two "Ask Claude" panels, which need claude.ai.

## What's in the snapshot (Oct 18, 2026)

- **Wire:** 23 headlines from Oct 1–18, 2026, from the outlets Perplexity Discover aggregates (CNBC, Yahoo Finance, CNN, Axios, Business Standard and others). Each headline is tagged with the sectors and tickers it moves (−3 to +3), and every one links to its source.
- **Universe:** 24 large U.S.-listed stocks across energy, defense, semiconductors, big tech, internet, consumer, financials, healthcare, gold and power. Each has its Oct 16 close, 52-week range, latest earnings result and next catalyst, with source links.
- **Base rates:** midterm-year Q4 seasonality, 52-week-high momentum (George & Hwang 2004), short-term reversal (Jegadeesh 1990) and post-earnings drift (Bernard & Thomas 1989).
- **Headwinds:** names the news argues against (DAL, CRWV, EIX). These are listed but not scored.

## Scoring model

Five factors, each scored 0–100:

| Factor | How it's computed |
|---|---|
| News | Sum of headline tags for the stock (full weight) and its sector (60%), with weight halving every 7 days; `50 + 50·tanh(sum/3)` |
| Trend | Position of the price in its 52-week range |
| Discount | Distance below the 52-week high (35% or more = 100); the contrarian counterpart to Trend |
| Earnings | Latest quarter vs. estimates (big beat and raise = 100, miss = 25); blank if not yet reported |
| Macro fit | Judgment of how the sector fits today's backdrop (oil > $100, 5% 10-year yield, 3.7% CPI, U.S.–Iran conflict, midterms) |

`Score = weighted mean of available factors − risk × aversion × 0.3`. Risk comes from volatility estimated from the 52-week range (`σ ≈ ln(high/low)/1.6`), plus 15 points when earnings are due within 10 days. The Conservative, Balanced and Aggressive profiles set the weights, or you can set your own with the sliders.

## Pulling in Perplexity headlines

perplexity.ai has no public news-feed API, and this build environment's network policy blocked it, so the snapshot uses the original outlets. To use Perplexity Discover directly, copy headlines from perplexity.ai/discover into the **Add headlines from Perplexity** box on the page. Claude tags each one against the same sectors and tickers, and the ranking updates. Pasted headlines are saved in your browser only.

## Refreshing the snapshot

Prices and headlines are fixed at Oct 16–18, 2026. To refresh, update the `TAPE`, `HEADLINES` and `STOCKS` arrays in `index.html` (each entry carries its source URL), then republish. The scoring code between `/*MODEL-START*/` and `/*MODEL-END*/` doesn't need changes.

*Research tool, not investment advice. Check live prices before trading.*
