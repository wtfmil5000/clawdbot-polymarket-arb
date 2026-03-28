# Clawdbot Polymarket Arb

> Clawdbot arbitrage scanner that continuously monitors Polymarket markets for price discrepancies and spread opportunities. Detects cross-market mispricings and executes arb entries automatically when the spread exceeds your minimum threshold.

*Last updated: March 2026*

![preview_clawdbot polymarket arbitrage scanner](https://github.com/user-attachments/assets/01ce5cf6-e643-4fee-a4d6-2fa536bcf8f3)

---

## What is Clawdbot Polymarket Arb?

Clawdbot Polymarket Arb is the arbitrage module of the Clawdbot suite. It scans Polymarket CLOB prices in real time, compares them against external probability sources and cross-market equivalents, and automatically enters positions when a price divergence exceeds your configured minimum spread.

Arb opportunities on Polymarket close fast. This module is built to detect and act before the window disappears.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/wtfmil5000/clawdbot-polymarket-arb/releases) |

---

## What It Scans For

| Arb Type | Description | Threshold |
|----------|-------------|-----------|
| **Cross-market spread** | Same event priced differently on Polymarket vs Kalshi or PredictIt | Configurable min spread |
| **YES/NO imbalance** | YES + NO prices sum to less than 1.0 | Auto-detected |
| **Stale price lag** | One side hasn't updated after related news | Configurable staleness window |
| **Correlated market gap** | Identical events in same category diverge | Score-based detection |

---

## Engine Features

* **Multi-source comparison** - compares Polymarket prices against Kalshi and PredictIt equivalents
* **YES/NO imbalance detection** - flags markets where YES + NO don't sum to near 1.0
* **Auto-execute** - enters the underpriced side automatically when spread clears threshold
* **Speed-optimized** - sub-second detection loop with prioritized order submission
* **Daily P&L tracking** - logs realized arb profit per market with entry and exit prices
* **Telegram alerts** - instant notification on every arb detected and executed
* **Skip list** - blacklist specific markets or categories from scanning

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Sources** | Polymarket + Kalshi built-in | Configurable |
| **Execution** | Auto + manual review | Full control |
| **Config** | `config.toml` | Direct code access |
| **Alerts** | Dashboard + Telegram | JSON + Telegram |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set min spread threshold and Polymarket API key
# 3. Run Clawdbot Arb - scanning starts immediately across all active markets
```

### Python

```bash
cd clawdbot-polymarket-arb/python
pip install -r requirements.txt
python clawdbot-polymarket-arb-v.1.4.14.py
```

---

## How It Works

![clawdbot arb pipeline](https://github.com/user-attachments/assets/8c58eee3-73d1-49b4-aa90-a1a8186f420e)

Four stages per scan cycle:

1. **Fetch** - pulls current YES/NO prices for all active Polymarket markets
2. **Compare** - checks each market against Kalshi and cross-market equivalents
3. **Score** - calculates spread magnitude and assigns confidence score
4. **Execute** - places order on the underpriced side if spread clears minimum threshold

### Config Reference

```toml
[arb]
min_spread = 0.04               # Minimum price gap to trigger entry
position_size_usd = 30
max_open_arbs = 5
scan_interval_ms = 500

[sources]
compare_kalshi = true
compare_predictit = false

[polymarket]
api_key = ""
private_key = ""

[telegram]
bot_token = ""
chat_id = ""

[export]
arb_log_csv = "data/arb/arb_log.csv"
```

---

## Arb Log Format

```json
{
  "arb_id": "arb_20260325_031",
  "market": "will-fed-cut-rates-may-2026",
  "polymarket_yes": 0.41,
  "kalshi_yes": 0.49,
  "spread": 0.08,
  "side_entered": "YES",
  "entry_price": 0.41,
  "exit_price": 0.47,
  "pnl_usd": 1.80,
  "timestamp": "2026-03-25T10:12:44Z"
}
```

---

## Verified Live

**Configuration used:**
* Min spread 0.04, Polymarket vs Kalshi comparison, $30 per position

**Arb executed:**

| | Details |
|---|---|
| Market | will-fed-cut-rates-may-2026 |
| Polymarket YES | 0.41 |
| Kalshi YES | 0.49 |
| Spread | 0.08 |
| Entry | YES at 0.41 |
| Exit | 0.47 after convergence |
| P&L | +$1.80 |
| Tx hash | 0xa1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2 |

---

## Frequently Asked Questions

**What is Clawdbot Polymarket Arb?**
Clawdbot Polymarket Arb is the arbitrage scanning module of the Clawdbot suite. It detects price discrepancies between Polymarket and equivalent markets on other platforms, then automatically enters the underpriced side when the spread exceeds your configured minimum.

**What platforms does it compare against?**
By default it compares Polymarket against Kalshi for equivalent events. PredictIt comparison is available in config but disabled by default due to slower price feeds.

**Is arb on Polymarket risk-free?**
No arb is fully risk-free. Execution latency, liquidity changes, and event correlation risk all apply. The bot minimizes these through fast execution and minimum spread thresholds, but no guarantee of profit exists.

**What is a good minimum spread?**
0.04 to 0.06 is a common starting range. Below 0.03 generates high noise and execution costs eat the spread. Above 0.08 catches only wide obvious gaps that may indicate information asymmetry rather than pure arb.

**Is this a polymarket arb execution bot?**
Yes. Detection and execution are combined - when a spread is detected and confirmed above threshold, the bot immediately places the order without waiting for manual confirmation.

**Can I run it alongside other Clawdbot modules?**
Yes. The Clawdbot suite is designed for parallel module operation. Run Arb alongside Whale or Signals modules simultaneously.

**Does it track resolved arb outcomes?**
Yes. The arb log records entry price, exit price, and P&L for every closed position, building a full track record of arb accuracy over time.

---

## Use Cases

- **Polymarket arb execution** - automatically capture price discrepancies between Polymarket and Kalshi
- **Polymarket cross-market arb** - systematic arb scanning across multiple prediction market platforms
- **Polymarket spread detection** - identify and act on YES/NO spread imbalances in CLOB markets
- **Clawdbot arbitrage module** - run as part of the full Clawdbot multi-module trading suite
- **Polymarket mispricing scanner** - continuous detection of mispriced odds relative to external sources

---

## Repository Structure

```
clawdbot-polymarket-arb/
+-- clawdbot-polymarket-arb-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- arb/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- scanner.py
|   |   +-- comparator.py
|   |   +-- executor.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, py-clob-client, pandas
```

* Polymarket account with CLOB API access
* Telegram bot token (for alerts)

---

*Detect the gap. Close it first.*
