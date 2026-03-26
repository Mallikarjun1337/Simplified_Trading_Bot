# Simplified_Trading_Bot
# Binance Futures Testnet Trading Bot

A Python CLI application for placing MARKET and LIMIT orders on Binance Futures Testnet (USDT-M).
Built with `python-binance`, clean modular structure, and production-level error handling.

---

## Features

- MARKET and LIMIT orders — BUY and SELL
- CLI via `argparse` with input validation and clear error messages
- `--dry-run` flag to preview orders without submitting
- Structured file logging with timestamps (`logs/trading_bot.log`)
- Typed exception handling: API errors, validation failures, network errors
- Modular layout: client / orders / validators / logging / CLI fully separated

---

## Setup

**1. Install dependencies**
```bash
pip install -r requirements.txt
```

**2. Set API credentials**
```bash
# Linux / macOS
export BINANCE_API_KEY="your_api_key"
export BINANCE_API_SECRET="your_api_secret"

# Windows (PowerShell)
$env:BINANCE_API_KEY="your_api_key"
$env:BINANCE_API_SECRET="your_api_secret"
```

> Get testnet credentials at [testnet.binancefuture.com](https://testnet.binancefuture.com) — sign in with GitHub and generate an API key pair.

---

## Example Commands

```bash
# MARKET BUY
python -m bot.cli --symbol BTCUSDT --side BUY --type MARKET --quantity 0.01

# LIMIT SELL
python -m bot.cli --symbol BTCUSDT --side SELL --type LIMIT --quantity 0.01 --price 80000

# Dry run (preview only, no order submitted)
python -m bot.cli --symbol BTCUSDT --side BUY --type MARKET --quantity 0.01 --dry-run

# Help
python -m bot.cli --help
```

---

## Usage

Run all commands from inside the `trading_bot/` directory.

All orders require `--symbol`, `--side`, `--type`, and `--quantity`.
`--price` is required for LIMIT orders.

---

## Sample Output

```
  Order Request
  ──────────────────────────────────
  Symbol      : BTCUSDT
  Side        : BUY
  Type        : MARKET
  Quantity    : 0.01
  ──────────────────────────────────

  Order Confirmed
  ──────────────────────────────────
  Order ID    : 12995468953
  Status      : NEW
  Executed    : 0.000
  Avg Price   : 0.00
  ──────────────────────────────────

  [OK] Order placed successfully.
```

Log written to `logs/trading_bot.log`:
```
2026-03-26 17:46:17 | INFO  | bot.orders | Placing BUY MARKET order | symbol=BTCUSDT qty=0.01
2026-03-26 17:46:18 | INFO  | bot.orders | Order accepted | id=12995468953 status=NEW executedQty=0.000 avgPrice=0.00
```

---

## Assumptions

- USDT-M Futures only (not COIN-M)
- Credentials via environment variables — never hardcoded
- Minimum order notional is $100 (testnet rule) — use `0.01` for BTCUSDT
- LIMIT orders default to `timeInForce=GTC`
- Requires Python 3.10+
