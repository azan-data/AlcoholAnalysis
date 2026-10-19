# Headlines to Holdings

A single-page stock screener that turns the week's top U.S. world, business and tech headlines into a ranked list of stocks to consider. It weighs each stock's 52-week price history, its latest earnings and historical base rates, and you can adjust how much each factor counts.

`index.html` is the whole app: data, scoring model and UI in one file. It is published as a claude.ai Artifact. Open the file in a browser and everything works except the two "Ask Claude" panels, which need claude.ai.

On screens 700px wide or narrower it switches to a phone layout: one section at a time, chosen from a bottom tab bar (Picks, News, Markets, Ask). Each tab can be linked directly with `#picks`, `#news`, `#markets` or `#ask`.

## What's in the snapshot (refreshed Oct 19, 2026)

- **News sentiment:** 46 stories from Oct 1–19, 2026, from the outlets Perplexity Discover aggregates (CNBC, Reuters, Bloomberg, CNN, Yahoo Finance and others), with company-specific news for most of the 24 stocks. 44 are confirmed by two or more independent outlets; the other 2 are shown but not counted (see Cross-checking). Claude read each story and tagged the sectors and tickers it helps or hurts (−3 to +3). Each story shows a tone (positive, negative, mixed or neutral), a plain-language market read and what it is good or bad for, and links to its source.
- **Universe:** 24 large U.S.-listed stocks across energy, defense, semiconductors, big tech, internet, consumer, financials, healthcare, gold and power. Each has its latest close confirmed by two sources (Oct 16 for most; Amazon's Oct 19 close was confirmed), 52-week range, latest earnings result and next catalyst, with source links.
- **Base rates:** midterm-year Q4 seasonality, 52-week-high momentum (George & Hwang 2004), short-term reversal (Jegadeesh 1990) and post-earnings drift (Bernard & Thomas 1989).
- **Headwinds:** names the news argues against (DAL, CRWV, EIX). These are listed but not scored.

## Can you trust it?

The page's Trust tab sums up what research says about predicting stocks, with each finding cross-checked against the study and independent coverage. In brief:
- Most professionals fail to beat an index fund: 79% of active large-cap funds trailed the S&P 500 in 2025.
- Most published signals fade after publication or fail to replicate.
- News sentiment is mostly priced in within a day.
- Momentum, quality and profitability, value, low volatility, unusual insider buying, short interest and rising estimates have the best records.

Each part of this page's score gets an evidence grade. The default **Evidence-weighted** style weights them accordingly: price trend 35, news 20, earnings 15, economic backdrop 10, room to rebound 5, risk aversion 50. The model has no backtest or track record yet.

## Cross-checking

No score rests on a single article. A story moves a score only when at least two independent outlets report it, and each story lists the outlets that confirm it. Syndicated copies of one article count once. Single-source stories stay on the page marked "Not counted", with a note on why. Closing prices were matched across at least two quote pages, and anything resting on one source (Conoco's close, Micron's close, Goldman's 52-week range, two earnings dates) is flagged on that stock. The rules are written down in `CLAUDE.md`, so future updates follow them.

Cross-checking changed this snapshot. A report that Northrop lost an Air Force drone contest this week turned out to describe a June decision, so it no longer counts. Netflix and Vistra prices were corrected to their Oct 16 closes. Broadcom's AI revenue was updated to its latest filing. Tesla's delivery story flipped to positive once the beat was confirmed. Exxon's refining gain was weighted down after a $2.5 billion offset turned up in other coverage.

## Scoring model

Five factors, each scored 0–100:

| Factor | How it's computed |
|---|---|
| News sentiment | Sum of headline tags for the stock (full weight) and its sector (60%), with weight halving every 7 days; `50 + 50·tanh(sum/3)` |
| Price trend | Position of the price in its 52-week range |
| Room to rebound | Distance below the 52-week high (35% or more = 100); the contrarian counterpart to Price trend |
| Earnings | Latest quarter vs. estimates (big beat and raise = 100, miss = 25); blank if not yet reported |
| Economic backdrop | Judgment of how the sector fits today's backdrop (oil > $100, 5% 10-year yield, 3.7% CPI, U.S.–Iran conflict, midterms) |

`Score = weighted mean of available factors − risk × aversion × 0.3`. Risk comes from volatility estimated from the 52-week range (`σ ≈ ln(high/low)/1.6`), plus 15 points when earnings are due within 10 days. The Evidence-weighted (default), News-driven, Conservative and Bargain hunter styles set the weights, or you can set your own with the sliders. Verdicts: Buy ≥ 62, Lean buy ≥ 54, Hold ≥ 45, Avoid below.

Each stock shows its news sentiment as Bearish → Bullish with a count of good and bad stories, up to three plain-language reasons ("Big earnings beat", "Sector headwind", "Earnings Oct 21"), and a one-sentence summary of why it ranks where it does. The full factor table sits under "Show the math".

## Pulling in Perplexity headlines

perplexity.ai has no public news-feed API, and this build environment's network policy blocked it, so the snapshot uses the original outlets. To use Perplexity Discover directly, copy headlines from perplexity.ai/discover into the **Add today's headlines from Perplexity** box on the News tab. Claude tags each one against the same sectors and tickers. A pasted headline counts toward scores only after you confirm that at least two outlets report it (Perplexity lists its sources under each story). Pasted headlines are saved in your browser only.

## Refreshing the snapshot

Prices and headlines are fixed at Oct 16–19, 2026. To refresh, update the `TAPE`, `HEADLINES` and `STOCKS` arrays in `index.html`, with at least two independent sources for each story (in `also`) and each price, then republish. The scoring code between `/*MODEL-START*/` and `/*MODEL-END*/` doesn't need changes.

*Research tool, not investment advice. Check live prices before trading.*
