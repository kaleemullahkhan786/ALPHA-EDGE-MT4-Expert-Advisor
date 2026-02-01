# ALPHA EDGE – MT4 Expert Advisor

Grid-based basket EA for MetaTrader 4 with H1 candle signal, group management, trailing stop, and safety guards.

## Features
- **Basket/Group Trading** – Manages up to 3 parallel groups with separate grid logic
- **H1 Signal** – Opens first trade on bullish/bearish closed H1 candle
- **Configurable Grid** – Pip step, lot booster, and max trades per group
- **Trailing Stop** – Optional trailing for single-trade baskets
- **Equity Guard** – Auto-closes all EA trades if drawdown exceeds limit
- **Day/Time Filters** – Per-day enable/disable and trading hours
- **Account Lock & Expiry** – Optional hardcoded security protection

---

## Requirements
- MetaTrader 4 (MT4)
- Broker account with trading enabled
- `ALPHA_EDGE.mq4` compiled to `ALPHA_EDGE.ex4`

---

## Installation
1. Open MT4.
2. Go to **File → Open Data Folder**.
3. Copy `ALPHA_EDGE.ex4` (or `ALPHA_EDGE.mq4`) into `MQL4/Experts/`.
4. Restart MT4 or right-click **Navigator** → **Refresh**.
5. Locate **ALPHA_EDGE** under **Expert Advisors**.

---

## How to Attach the EA
1. Open the chart for your symbol.
2. Drag **ALPHA_EDGE** onto the chart.
3. Enable:
   - **Allow live trading**
   - **Allow DLL imports** (only if required; EA does not use DLLs)
4. Click **OK**.

---

## Inputs Overview

### General Settings
| Input | Description |
|-------|-------------|
| Lots | Base lot size for the first trade in a group |
| Booster | Lot multiplier for each subsequent grid trade |
| TakeProfit | Basket TP distance in pips (when `UseTakeProfitPct=0`) |
| PipStarter | Grid step in pips to add next trade |
| GridGapSeconds | Minimum seconds between trades in the same group |
| Max_Trades_Allowed_Group1/2/3 | Max trades per group |
| MagicNumber | Unique ID for EA trades |

### Additional Safe Guards
| Input | Description |
|-------|-------------|
| UseStoplossPct | Enable SL by % (1=On, 0=Off) |
| StoplossPct | SL % from entry (when enabled) |
| UseTakeProfitPct | Enable TP by % (1=On, 0=Off) |
| TakeProfitPct | TP % from average basket price |

### Trailing Stop
| Input | Description |
|-------|-------------|
| UseTralling | Enable trailing for single-trade baskets |
| TrallingStart | Start trailing after this profit (pips) |
| LockProfit | Lock this many pips when trailing |
| UseEquityGuard | Close all EA trades if drawdown exceeds limit |
| Amountindollars | Max allowed drawdown (account currency) |

### Day/Time Filters
- **TradeonMonday** … **TradeonSunday**: 1 = allow, 0 = block.
- **InpStartHour** / **InpEndHour** per day: Trading window (e.g. `"00:00"` – `"23:59"`).  
  Same start/end = full 24h for that day.

---

## Security & Protection (Hardcoded)

Account lock and expiry are set in the source code and are **not** in MT4 inputs.

| Setting | Description |
|---------|-------------|
| `ALLOWED_ACCOUNT` | If not 0, EA works only on this account number |
| `EXPIRY_YEAR/MONTH/DAY` | EA stops trading after this date |

**Where to edit:**
1. Open `ALPHA_EDGE.mq4` in MetaEditor.
2. Find the block near the top:
   ```text
   // SECURITY & PROTECTION SETTINGS (HARDCODED - DO NOT MODIFY)
   #define ALLOWED_ACCOUNT      0
   #define EXPIRY_YEAR          0
   #define EXPIRY_MONTH         0
   #define EXPIRY_DAY           0
   ```
3. Update the values, save, and recompile to generate a new `ALPHA_EDGE.ex4`.

**Messages when blocked:**
- *"This account is not allowed. Contact developer."*
- *"EA license expired. Contact developer."*

---

## Notes
- EA uses group-based basket logic; one direction per group.
- Trading only occurs within the configured day/time windows.
- For reproducible results, use the same `.set` file as in backtests.

---

## Troubleshooting
- **No trades:**
  - Ensure AutoTrading is ON.
  - Confirm symbol allows trading.
  - Check day/time filters.
  - Verify account lock/expiry is not blocking.
- **Different results vs backtest:** Check spread, commission, and symbol settings.

---

## License
See repository license for terms of use.
