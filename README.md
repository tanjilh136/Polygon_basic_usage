# Alpaca & Polygon.io API — Exploration & Integration Notebook

## Background

This notebook was developed during my time as a Python Developer at a remote trading platform startup (2020–2021). It served as the foundational exploration and prototyping layer for the trading system — the working space where Alpaca and Polygon.io API integrations were figured out, tested, and refined before being structured into the production codebase (AlpacaTradingBot and PolygonSMSBot).

Think of this as the laboratory. The production systems came after.

---

## What This Notebook Covers

### Alpaca API Integration
Full order lifecycle implementation against the Alpaca brokerage API:

- Account information retrieval (buying power, balance)
- Asset lookup by ticker symbol (company name, shortability status)
- Order placement — three order types:
  - **Market orders** — immediate execution at current price
  - **Limit orders** — execution only at specified price or better
  - **Stop-limit orders** — trigger + limit price combination
- Buy and sell implementations for all three order types
- Order management: list open orders, fetch order by ID, cancel single order, cancel all orders
- Activity history retrieval (filled orders)

### UNIX Nanosecond Timestamp Handling
Custom utility functions for working with Polygon's nanosecond timestamp format:
- Current time in nanoseconds
- Add/subtract N minutes from a nanosecond timestamp
- Convert nanosecond timestamp to human-readable datetime

### Polygon.io Market Data Integration
- `get_last_30_min_data()` — retrieves the last 30 minutes of live trade data for any ticker via Polygon's historical trades endpoint. Uses a custom pagination mechanism to collect all ticks within the window.
- `get_30_min_res_with_quantity_formula()` — splits 30-minute data into three 10-minute segments, computes weighted average (3x first, 2x second, 1x third), floors the result to produce a recommended buy quantity. This formula directly fed the automated order sizing in the trading bot.

### Barset Generation
- `generate_barset_forever()` — iterates through a list of company tickers, retrieves 1-minute OHLCV (Open, High, Low, Close, Volume) data from Polygon, and saves per-ticker CSV files for downstream analysis.

### Top Movers Detection
- `max3mover()` — reads all saved barset CSV files, computes average volume per ticker, sorts descending, and returns the top 3 tickers by average volume. Used as a signal for identifying high-activity stocks for potential trading.

---

## Relationship to Production Systems

This notebook directly preceded and informed two production-oriented repositories:

| Component | This Notebook | Production System |
|-----------|-------------|-------------------|
| Order execution | Explored and tested here | `AlpacaTradingBot` |
| Market data stream | REST API prototyped here | `PolygonSMSBot` (WebSocket) |
| Quantity formula | Developed here | Used in strategy layer |

---

## Tech Stack

- **Language:** Python
- **Brokerage:** Alpaca Trade API (paper trading environment)
- **Market Data:** Polygon.io REST API
- **Data:** pandas · CSV
- **Environment:** Google Colab / Google Drive

---

## Important Notes

**This repository is private and is maintained for personal reference only.**

All API credentials have been removed from this notebook. The original development used paper trading credentials (no real money involved). Any credentials referenced in earlier versions of this notebook should be considered expired and non-functional.

This code reflects exploratory and prototyping work — not production-grade software. For the structured, production-oriented implementations, see the AlpacaTradingBot and PolygonSMSBot repositories.

---

## Author

**Tanjil Hasan**
github.com/tanjilh136 · linkedin.com/in/tanjil-hasan-650246169
