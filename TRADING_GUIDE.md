# Claude MCP Trading Routine Guide

A step-by-step daily routine for using Claude AI via MCP to control TradingView and execute the **"Liquidity Sniper 20x"** strategy.

---

## Allowed Trading Hours (Makkah Time UTC+3)
This strategy hunts liquidity sweeps — liquidity only appears during high-volume sessions.
- 🟢 **London Session:** `10:00 AM` to `1:00 PM` (clean moves, moderate volume)
- 🔥 **New York Session:** `4:30 PM` to `7:30 PM` (best session — highest liquidity)
- ❌ **Asian Session:** `No trading.` Random chop and dead ranges.
- ❌ **Weekends:** `No trading.` Low liquidity and erratic manipulation.

---

## Daily Routine (From boot to trade close)

### 1️⃣ Setup (First thing when you open your computer)
1. Open **PowerShell** and run this command to restart TradingView cleanly:
   ```powershell
   Stop-Process -Name TradingView -Force -ErrorAction SilentlyContinue; Start-Sleep 3;
   ```
   *(Then open the TradingView desktop app)*
2. Wait **10 seconds**, then launch Claude in the terminal:
   ```powershell
   cd "$HOME\tradingview-mcp"; claude
   ```
3. Paste this initialization prompt to Claude:
   > *"Run `tv_health_check` to ensure the MCP connection is fully stable. Next, read our '20x Liquidity Sweep Sniper' strategy from the `CLAUDE.md` file. Critically evaluate the strength of these rules and explicitly verify your capability to monitor these specific conditions (Volume spikes, ATR, EMA 200, Fakeouts) using your MCP tools. Confirm when loaded."*

### 2️⃣ Market Scan (Determine today's direction)
Let the AI do the analysis for you. Paste this command:
> *"Analyze the `BTCUSDT.P` chart on the 1H timeframe. Where is the price positioned relative to the EMA 200, and what is our directional bias (Long or Short) today? Based on that, drop down to the 15m timeframe and identify the top 3 high-probability liquidity pools (prominent highs/lows) closest to the current price."*

### 3️⃣ Set the Watchdog (Live monitoring)
After Claude identifies the target highs/lows, set your manual Alerts in TradingView at those prices.
Then give Claude this monitoring command:
> *"Excellent. Now monitor the 5m timeframe around those specific liquidity levels. Watch the OHLC data and Volume carefully. Once the price sweeps a level and prints a reversal candle (Fakeout) with a volume spike, notify me immediately. Ensure you calculate the ATR 14 to prepare for a dynamic Stop Loss."*

### 4️⃣ Execution (When the alert fires)
When the alert rings (make sure the 5m candle has closed), ask Claude for the precise analysis:
> *"The candle just closed! Evaluate this close on the 5m timeframe. Is it a valid Fakeout/Sweep according to our strict rules? Send me the entry report specifying: 1. Entry price. 2. Stop Loss size (using ATR, but strictly capping at 0.8% max risk). 3. Take profit targets (TP1 at VWAP, TP2 at opposing structure)."*

### 5️⃣ Open the Trade on Binance
- Go to Binance and open the trade exactly as Claude specified (use 25% of capital, 20x leverage).
- Set a **TP** order (close half at VWAP) and a **SL** order (stop loss).
- Once the trade reaches **+0.5%** profit, move the stop loss to entry (Breakeven) immediately.
- Close the screen. Your trade is now risk-free.

---

## Tips for the MCP Manager (You)
* **Don't trade emotions:** The biggest advantage is that YOU don't decide the entry — the machine does, based on cold math (ATR + Volume).
* **Watch the long wick:** If Bitcoin prints a huge fakeout wick that requires a stop loss of **1.2%**, Claude will tell you: "Cancel this trade" because it exceeds the 0.8% max risk. Obey immediately.
* **Fresh data matters:** When you ask Claude during execution time, make sure he's analyzing the latest live candles, not cached/stale data.

---

## Hidden Power Tools (Slash Commands)
These are pre-built commands inside the project you can type directly to Claude:

| Command | What it does | When to use it |
|---------|-------------|----------------|
| `/macro-dashboard` | Full macro snapshot: VIX, Dollar, Gold, Bitcoin, Yields | Before every session to check global market mood |
| `/market-regime` | Determines if we're in a Bull, Bear, or Correction market | Before trading to calibrate risk level |

### Advanced Prompts (For deeper analysis)
You can send these directly to Claude:
> *"Run `get_ta_summary` for `BINANCE:BTCUSDT.P` on timeframes 5, 15, 60. Then run `lookup_symbols` for the same symbol to get current price, volume, and daily change. Summarize the results."*

> *"Run `/macro-dashboard` and tell me: is Bitcoin in Risk-On or Risk-Off mode right now? Should I be cautious today?"*

## Economic News Sources (Check daily before trading)
- 🔴 [Forex Factory Calendar](https://www.forexfactory.com/calendar) — Red news = no trading
- 🪙 [CoinMarketCal](https://coinmarketcal.com/en/) — Crypto-specific events
- 📱 [Investing.com](https://www.investing.com/economic-calendar/) — Mobile app with push alerts
