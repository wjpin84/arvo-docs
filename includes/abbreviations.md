*[deflation]: The correction for having searched. If you tried nine configurations, the best of nine must clear the bar for "best of nine draws from noise", not the bar a single guess would face.
*[deflated]: Weighed against how many configurations were tried, so the best of many must clear a higher bar than a single guess.
*[out-of-sample]: Data the configuration was not chosen on. Arvo picks on one slice of history and judges on another, because choosing and judging on the same data is the commonest way a backtest lies.
*[in-sample]: The slice of history a configuration was chosen on.
*[walk-forward]: Instead of splitting history once, re-select on a rolling schedule: choose on a window, trade the next, roll forward. It tests whether choosing this way works, not whether one configuration held up.
*[conservative costs]: The same result re-run with transaction costs increased. A rule whose edge is thinner than its costs is common and looks fine at optimistic ones.
*[regime]: What kind of market a result was earned in — trending up, trending down, or ranging — labelled after the fact over the closes.
*[Sharpe]: Return divided by volatility, annualised. A ratio, not a verdict: it says nothing about how many things you tried before finding it.
*[drawdown]: The largest fall from a peak in account value, as a fraction.
*[ATR]: Average true range — the typical size of a bar's move, used to set stops in units that mean the same thing at any price.
*[VWAP]: Volume-weighted average price over the session.
*[delta]: How much an option's price moves per unit move in the underlying. Used to choose strikes.
*[0DTE]: Zero days to expiry — an option expiring the same day it is traded.
*[DTE]: Days to expiry.
*[dividend gap]: The return a price-only series misses because it does not include dividends. Arvo measures it and reports it beside the result rather than folding it in.
*[cross-sectional]: Ranking instruments against each other at each point in time, rather than judging each one on its own history.
*[universe]: A named list of instruments with a stated reason for being that list.
*[panel]: One rule tested across many instruments, with one configuration chosen for all of them.
*[finding]: A stored result: the verdict, the advice, the evidence, and the full specification needed to run it again.
*[verdict]: Arvo's answer about a rule — Supported, NotSupported or Inconclusive.
*[Inconclusive]: There is not enough evidence to say whether the rule works. A real answer, not a failure.
*[the gate]: The risk policy every entry passes through: it sizes the position or refuses it with a named reason. The same code runs in backtests and live.
*[risk gate]: The risk policy every entry passes through: it sizes the position or refuses it with a named reason. The same code runs in backtests and live.
*[divergence]: The measured difference between what a backtest assumed and what the broker actually did — fill prices against decision prices, and orders that never filled.
*[executor]: Where orders go: a broker's paper endpoint, or a real account.
*[session]: One finding's rule running against one venue, live or on paper.
*[ruleset]: A rule plus the parameter values to try for it. A ruleset is a search.
*[MCP]: Model Context Protocol — how an AI agent connects to tools like Arvo's research surface.
*[LGPL]: Lesser General Public License.
*[out of sample]: Data the configuration was not chosen on. Arvo picks on one slice of history and judges on another, because choosing and judging on the same data is the commonest way a backtest lies.
*[in sample]: The slice of history a configuration was chosen on.
*[search]: Trying many configurations. How much you searched decides the bar the winner must clear.
*[advice]: The plain-language findings attached to a result — each one a problem, a severity and an action — meant to be read before the metrics.
*[content hash]: A fingerprint of a file's contents, so a study records exactly which data produced it and a changed file is a different dataset.
*[promotion]: The gate in front of real money: at least five paper days without diverging from the backtest.
*[reconcile]: Square a session's own book against what the broker reports, after the two disagreed.
*[halt]: The kill switch: arm the gate, flatten what can be flattened, and stay stopped until a person clears it.
*[paper]: Trading against a broker's simulated-money endpoint, with a real API, real fills and real latency.
*[expectancy]: What a strategy earns per trade on average: win rate times average win, minus loss rate times average loss. Win rate alone says nothing without it.
*[slippage]: The difference between the price a decision was made at and the price the order actually filled at.
*[survivorship bias]: Testing on the names that still exist. The failures were removed from the data, so the result is flattered by an answer nobody searched for — which is why no statistical correction can detect it.
*[look-ahead bias]: Using information in a backtest that did not exist at the time the decision would have been taken.
*[efficiency ratio]: Net movement divided by total movement over a lookback. A path straight up scores 1.0; one that ends where it started scores 0.0. Arvo calls anything above 0.35 a trend.
*[variance risk premium]: The historical tendency for options to be priced above what the subsequent volatility turned out to justify, which is what an option seller is paid for.
*[market impact]: The price move your own order causes. A paper fill does not have it; a real one at size does.
*[microstructure]: The mechanics of how orders meet — quotes, queues and spreads — which decide what your fill actually is.
*[volatility clustering]: Large moves tend to follow large moves and quiet periods follow quiet ones. Reliable about size, much weaker about direction.
