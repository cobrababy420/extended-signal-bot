# Extended Signal Bot

> Real-time trading signal bot for Extended perp DEX on Starknet. Monitors funding rates, open interest, TradFi correlation signals (gold/SPX/EUR), and order book pressure across 50+ markets and delivers structured LONG/SHORT alerts to Telegram.

*Last updated: March 2026*

## What is Extended Signal Bot?

Extended Signal Bot is a market intelligence tool for Extended DEX traders on Starknet. It monitors all Extended markets - including unique TradFi perpetuals like XAU (gold), SPX (S&P 500), and EUR/USD - for high-conviction setups and sends structured signal cards to Telegram with entry zone, stop, target, and the specific triggers that fired.

![preview_extended-signal-bot](https://github.com/user-attachments/assets/0a3d93c9-736e-4966-a62e-13d3c449c63a)

Extended Signal Bot's unique advantage is its TradFi correlation engine: it detects when traditional market movements (gold rising, SPX dropping) historically lead crypto perp moves on Extended, and fires early-entry signals before the cross-asset ripple reaches the crypto order books.

No auto-execution - signals are for your review. You trade on Extended using the signal as context.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github-zip.com/extended) |

---

## Signal Triggers

| Signal Type | Trigger Condition | Description |
|-------------|------------------|-------------|
| Funding Extreme | Funding rate > 0.04% or < -0.02% | Extreme funding may precede reversal |
| OI Acceleration | OI increases > 6% in 15 min | New money entering the market |
| TradFi Correlation | Gold +1.5% or SPX -2% triggers crypto signal | Cross-asset leading indicator |
| Maker Order Surge | Large maker liquidity added on one side | Informed passive order book positioning |
| Momentum Break | Price breaks 4h high/low with OI confirmation | Breakout with open interest validation |

---

## Engine Features

* **TradFi correlation engine** - maps gold, SPX, and EUR moves to Extended crypto market signals
* **50+ market monitor** - scans all Extended markets continuously
* **Funding rate feed** - live funding data from Extended API
* **OI acceleration detector** - detects open interest acceleration per market
* **Maker order analyzer** - identifies large passive order book positioning
* **Multi-signal combinator** - requires configurable minimum signals before alert fires
* **Telegram delivery** - formatted signal cards with market, direction, entry, SL, TP
* **Signal history log** - stores all alerts with subsequent price performance

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **TradFi signals** | Config toggle | Full code access |
| **Delivery** | Telegram | Telegram + JSON |
| **Config** | `config.toml` | Direct code access |
| **Logs** | Dashboard | JSON per signal |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set Telegram token, thresholds, and TradFi correlation ON/OFF
# 3. Run Signal Bot - alerts arrive within seconds of trigger conditions
```

### Python

```bash
cd extended-signal-bot/python
pip install -r requirements.txt
python extended-signal-bot-raw-v.1.4.14.py
```

---

## How It Works

1. **Poll** - fetches funding, OI, price, and maker depth for all Extended markets plus TradFi feeds
2. **Correlate** - checks if TradFi moves (gold, SPX) match historical leading patterns
3. **Evaluate** - tests each market against all signal triggers
4. **Combine** - fires only when minimum trigger count is met
5. **Deliver** - sends Telegram signal card with actionable trade data

### Config Reference

```toml
[extended]
api_base = "https://api.extended.exchange"
poll_interval_seconds = 10
markets = []   # empty = all 50+ markets

[tradfi]
correlation_enabled = true
gold_threshold_pct = 1.5
spx_threshold_pct = 2.0
eur_threshold_pct = 0.8

[signals]
min_triggers_to_fire = 2
funding_extreme_threshold = 0.04
oi_acceleration_pct = 6.0
momentum_window_hours = 4

[alerts]
telegram_bot_token = ""
telegram_chat_id = ""
signal_cooldown_min = 25
```

---

## Signal Card Format (Telegram)

```
EXTENDED SIGNAL
Market: ETH-PERP
Direction: SHORT
Entry zone: 3,180 - 3,210
Stop-loss: 3,290
Take-profit: 2,980
Triggers: funding_extreme (0.051%) + tradfi_correlation (SPX -2.3%)
Confidence: HIGH (2/2 triggers)
Time: 2026-03-19 07:18 UTC
```

---

![extended signal bot feed](https://github.com/user-attachments/assets/b086d604-1784-4239-9106-2891fbbe4115)

## Verified Live

**Configuration used:**
* All markets, TradFi correlation ON, 2-trigger minimum

| | Details |
|---|---|
| Market | ETH-PERP |
| Signal | SHORT |
| Triggers | Funding 0.051% + SPX -2.3% correlation |
| Entry zone | 3,180 - 3,210 |
| Delivered | 2026-03-19 07:18 UTC |

| | Result |
|---|---|
| Low after signal | 2,964 (TP zone hit) |
| Signal accuracy | Target reached within 6h |

---

## Frequently Asked Questions

**What is the TradFi correlation signal?**
Extended is one of few DEXes with gold (XAU), SPX, and EUR perp markets. The bot monitors these alongside traditional finance data feeds. When SPX drops sharply, crypto markets on Extended historically follow within hours - the bot fires an early signal before the crypto books reprice.

**Does the bot execute trades?**
No. Extended Signal Bot is purely for signal delivery. You receive alerts in Telegram and execute trades manually on Extended.

**Can I disable TradFi signals and use only crypto signals?**
Yes. Set `correlation_enabled = false` in config to use only crypto-native signals (funding, OI, momentum).

**Can I backtest TradFi correlation accuracy?**
Yes. `scripts/backtest.py` replays historical Extended and TradFi feed data to evaluate signal performance over any date range.

**How many markets does it monitor?**
All 50+ Extended markets by default. Restrict to a subset via the `markets` field in config.

**Is Starknet data latency a concern?**
Starknet ZK-rollup blocks are fast. API data latency is typically under 500ms. The bot's poll interval (default 10s) is well within signal-relevant timeframes.

---

## Use Cases

- **Extended signal alerts** - receive Telegram alerts for high-conviction Extended DEX setups
- **TradFi crypto correlation bot** - detect gold and SPX correlation signals before crypto reprices
- **Starknet perp signal bot** - monitor 50+ Starknet perpetual markets for multi-factor setups
- **Funding extreme signal** - alert when funding rate exceeds thresholds on Extended markets
- **Extended OI acceleration tracker** - detect open interest surges on Starknet perp markets

---

## Repository Structure

```
extended-signal-bot/
+-- extended-signal-bot-raw-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- signals/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- monitor.py
|   |   +-- tradfi_feed.py
|   |   +-- correlator.py
|   |   +-- telegram_sender.py
|   +-- scripts/
|   |   +-- backtest.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, starknet-py, httpx, typer[all], python-telegram-bot, devtools
```

* Extended API access (public market data)
* Telegram bot token for signal delivery
* Python 3.10+

---

*TradFi moves. Crypto follows. Signal first.*
