# Headlines to Holdings: working rules

## Cross-check every story

Never base a score, a pick or a statement on a single news article.

- A story counts toward scores only when at least two independent outlets report the same facts. Record the extra outlets in the story's `also` array in `index.html`; the page enforces this through `isConfirmed()` (`MIN_SOURCES = 2`).
- Syndicated copies count once. Yahoo Finance republishing 24/7 Wall St., TIKR or Benzinga, TradingView or KuCoin republishing Benzinga, and the MarketBeat network (Defense World, The Stock Observer, Daily Political) are not independent of their originals.
- A company's own release or SEC filing counts as an independent source for its own numbers.
- When a story can't be confirmed, keep it with `also: []` and a `note` explaining why, so it shows as "Not counted". When other reporting contradicts it, say so in the note and fix any thesis or risk text that relied on it.
- Prefer the most recent confirmed figures. A commentary piece can quote stale numbers, so check the latest filing.
- Closing prices must match across at least two quote pages. Anything resting on one source (a 52-week range, an earnings date, a price) gets a `flag` on the stock.
- Pasted Perplexity headlines start unconfirmed and count only after the viewer marks them confirmed.
