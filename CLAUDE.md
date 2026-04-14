# TradingView Claude MCP: BTCUSDT.P Liquidity Sniper (20x)

> ⚠️ **Important:** This strategy is designed exclusively for **TRADING VIEW CLAUDE MCP**. The AI must follow these rules mechanically when analyzing TradingView data.

---

## Account Info
- **Asset:** BTCUSDT.P (Bitcoin Perpetual Futures) only
- **Margin per trade:** 25% of total capital
- **Leverage:** 20x Isolated
- **Max open trades:** 1 at a time
- **Platform:** Binance Futures USDT-M

## Timeframes
- **1H:** Determine the main trend direction (above or below EMA 200)
- **15m:** Identify liquidity zones (previous highs/lows loaded with stop losses)
- **5m:** Execution frame — wait for the fakeout and reversal candle here

## Indicators
1. **EMA 200** — on 1H — trend filter (above = Long bias, below = Short bias)
2. **VWAP** — on 15m — first take profit target (TP1) and price magnet zone
3. **Volume** — on 5m — confirms the sweep (high volume = stop hunt confirmed)
4. **ATR 14** — on 5m — measures volatility to size the stop loss, capped at 0.8% max

---

## 5 Long Entry Conditions (ALL must be met)

| # | Condition | How to verify |
|---|-----------|---------------|
| 1 | 1H trend is bullish | Price is above EMA 200 on the 1H chart |
| 2 | Liquidity pool identified (15m) | A clear previous low that traders are using as support on 15m |
| 3 | Stop hunt / Sweep (5m) | Price drops sharply and breaks below the low on 5m, triggering retail stop losses |
| 4 | High volume on the sweep | The sweep candle (5m) shows a clear volume spike above the 10-candle average |
| 5 | Reversal candle closes above the low | ⚠️ Wait for the 5m candle to fully close — it must close **above** the broken low with a long wick (Pin Bar or Engulfing) |

## 5 Short Entry Conditions (Mirror opposite)

| # | Condition | How to verify |
|---|-----------|---------------|
| 1 | 1H trend is bearish | Price is below EMA 200 on the 1H chart |
| 2 | Liquidity pool identified (15m) | A clear previous high that traders are using as resistance on 15m |
| 3 | Stop hunt / Sweep (5m) | Price spikes up and breaks above the high on 5m, triggering retail stop losses |
| 4 | High volume on the sweep | The sweep candle (5m) shows a clear volume spike above the 10-candle average |
| 5 | Reversal candle closes below the high | ⚠️ Wait for the 5m candle to fully close — it must close **below** the broken high with a long upper wick |

---

## Stop Loss
- **Long:** Below the lowest wick of the fakeout candle on 5m
- **Short:** Above the highest wick of the fakeout candle on 5m
- **Max allowed distance:** **0.8%** from entry price (use ATR 14 to calculate)
- ⛔ If the distance exceeds 0.8% → cancel the trade — find a better setup
- ⛔ Stop loss never moves backward — only forward

## Breakeven Rule
- Once price moves **+0.5%** in your favor → move stop loss to entry price immediately
- Never let a winning trade turn into a loser

## Take Profit — 2 Stages

| Stage | Target | Close % | What happens next |
|-------|--------|---------|-------------------|
| TP1 | VWAP line | Close 50% | Move stop to entry (breakeven) |
| TP2 | Opposite structure high/low | Close remaining 50% | Trade complete |

---

## Daily Safety Rules (Non-negotiable)
- ❌ Max daily losses: 2 losing trades — then stop for the day
- ❌ Max weekly loss: 15% of capital — then stop for the week
- ❌ Never re-enter the same trade after getting stopped out
- ❌ Never widen the stop loss under any circumstances

## Allowed Trading Hours (Makkah Time UTC+3)

| Session | Time | Status |
|---------|------|--------|
| London (Europe) | 10:00 AM - 1:00 PM | ✅ Allowed |
| New York Open | 4:30 PM - 7:30 PM | ✅ Best session (highest liquidity) |
| New York Afternoon | 7:30 PM - 11:00 PM | ✅ Allowed |
| Asian Session | 4:00 AM - 9:00 AM | ❌ No trading (random chop) |
| Weekends | Saturday & Sunday | ❌ No trading (low liquidity) |
| Before CPI/FOMC/NFP/PPI/GDP | 2 hours before + 1 hour after | ❌ No trading |

## News Filter (Mandatory)
- ❌ No new trades 2 hours before and 1 hour after: **CPI / FOMC / NFP / PPI / GDP**
- ⚠️ If a trade is open during news, secure it at breakeven or close it
- 📅 Check daily: [forexfactory.com/calendar](https://www.forexfactory.com/calendar)

---

## AI Response Protocol (When analysis is requested)
When the user asks (via TRADING VIEW CLAUDE MCP) to scan the chart and evaluate a Bitcoin opportunity, follow these steps:

1. **Response must be organized and in Arabic.**
2. **Use available MCP tools to gather live data:**
   - Use `get_ta_summary` for `BINANCE:BTCUSDT.P` on timeframes `5, 15, 60` to get Buy/Sell/Neutral signals per frame.
   - Use `lookup_symbols` for `BINANCE:BTCUSDT.P` to get current price, daily change, volume, and high/low.
   - Use `screen_crypto` with RSI filter to confirm overbought/oversold status.
3. **Analyze the collected data** against the strategy conditions (5 Long or 5 Short conditions).
4. **Deliver the report in this structure:**
   - **TA Summary:** (Signals from all 3 timeframes: 1H / 15m / 5m)
   - **Current Opportunity:** (🟢 Long Entry / 🔴 Short Entry / ⏳ No setup — waiting for liquidity sweep)
   - **Stop & Liquidity Status:** (Which high/low was swept + volume status)
   - **Execution Points (if entry exists):**
     - Entry price: [...]
     - Stop loss: [...] (with proof that distance ≤ 0.8%)
     - TP1 target (VWAP): [...]
     - TP2 target (opposite structure): [...]
5. **Safety reminder:** Always remind the user to apply trailing stop and breakeven rules.

## Advanced MCP Commands (Slash Commands)
The user can use these built-in commands to enhance analysis:
- `/macro-dashboard` — Full macro snapshot (VIX, Dollar, Gold, Bitcoin, Yields)
- `/market-regime` — Determine global market state (Bull / Bear / Correction)

## Disclaimer
This is a decision-support tool only — the final call is mine as the trader. Any losses are my personal responsibility.
