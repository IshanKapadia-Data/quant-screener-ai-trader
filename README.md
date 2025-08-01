# 📈 Automated S&P 500 Pre-Market Screener & Trade Executor 

This project builds a pre-market screener for S&P 500 stocks based on **technical momentum criteria**, then applies a **rule-based trading strategy** for swing trading. Initially designed for manual trade review, it has evolved into a **fully automated AI trading agent** using **n8n**, saving over 4 hours of daily effort.

---

## 🚀 Project Purpose

The goal of this project is to streamline my **morning trading workflow** by identifying S&P 500 stocks that are **within a favorable price range relative to their 52-week high**, compute their **RSI (Relative Strength Index)**, and generate a focused watchlist before the U.S. market opens. The strategy targets strong momentum setups while accounting for **overnight price gaps** and is designed for **swing trade execution** with a multi-stage entry approach.

---

## 📊 Screener Overview

### ✅ Technical Filters Applied:
- **Universe:** S&P 500 stocks (scraped from Wikipedia)
- **Price Filter:** Current price between **85% and 92% of 52-week high**
  - This wider range accounts for **overnight volatility** to avoid missing trades at open
- **Indicators Displayed:**  
  - RSI (14-day)  
  - % Below 52-Week High  
  - P/E Ratio  
  - Current Price  
  - 52-Week High

### 🔎 Output:
- A sorted CSV file (`sp500_watchlist_YYYY-MM-DD.csv`) listing stocks ranked by proximity to their 52-week high.
- Example columns:
  - `Ticker`, `Current Price`, `52-Week High`, `% Below High`, `P/E Ratio`, `RSI`

---

## 📌 Original Workflow (Manual)

Originally, I ran this script manually each morning and followed this workflow:

1. Run the script before market open
2. Review the CSV output
3. For each stock in the list:
   - Analyze technical indicators and recent chart behavior
   - Make discretionary trade decisions
   - Manually place orders in my trading account

This approach was effective but **time-consuming**, taking roughly **4 hours each day**.

---

## 🔁 Trading Strategy Logic

### 📥 Entry Logic:

- **Entry 1:** Buy at market open if stock is **between 85% and 92% of its 52-week high**
- **Entry 2 (Re-buy):** Buy again if price moves up to within **5% of its 52-week high**

### 📤 Exit Conditions (per position):

Each position is exited based on the first rule hit:

- **Target Gain:** +20% from buy price
- **Stop Loss:** -15% from buy price

> Each entry is treated as an independent position with its own logic and tracking.

---

## 📈 Strategy Backtest Results (Jan 1, 2023 – Jul 31, 2025)

| Metric              | Value       |
|---------------------|-------------|
| **CAGR**            | 18.46%       |
| **Sharpe Ratio**    | 1.35        |
| **Win Rate**        | 67.86%      |
| **Avg Win/Loss**    | 1.8         |
| **Avg Holding Time**| 60 days |

---

## 🤖 AI-Based Automation with n8n

To eliminate manual steps, I built a **low-code AI agent** using **[n8n](https://n8n.io/)** to fully automate the process from screener execution to trade placement.

### 🔧 Automation Flow

- **Trigger:** Every morning before market open, n8n triggers the Python screener via cron.
- **File Handling:** The resulting CSV is passed through n8n’s internal file system or cloud connector.
- **Signal Evaluation:** Each row is evaluated using JavaScript logic and IF branches that enforce:
  - Entry eligibility
  - Re-entry triggers
  - Price-based exits
  - Time-based exits
- **Trade Execution:**  
  Trades are automatically executed through integration with a brokerage API.  
  Some supported or scriptable brokers include:

  - 🟢 **Alpaca** – Commission-free trading with a modern, well-documented REST API (ideal for retail algo trading)  
  - 🟢 **Interactive Brokers (IBKR)** – Widely used for both retail and institutional trading, via **IBKR API** or **IB Gateway**  
  - 🟢 **TD Ameritrade** – Offers a powerful API with authentication flow and trading endpoints (retail-friendly)  
- **Logging & Notifications:**
  - All trades are logged to Google Sheets
  - Alerts are sent via email when a trade is executed for transparency and tracking

### 💡 Benefits

- Saves **4+ hours of manual work daily**
- Enforces **consistent, rules-based trading discipline**
- Avoids emotional trading decisions
- Easily modifiable using n8n’s visual editor
- Fully extensible with additional filters or data sources

---

## ⚙️ Tech Stack

- **Python**: Screener, RSI calculations, price filters
- **n8n**: Automation engine (low-code)
- **yfinance**: Real-time market data
- **pandas, tqdm, datetime**: Data wrangling
- **Google Sheets / Email / Webhooks**: Trade logging and alerts

---


## 📌 Next Steps

- Implement **position sizing and portfolio-level risk control**
- Track trades in a structured database (PostgreSQL/MongoDB)
- Explore **RL or ML-enhanced decision making**

---


**Disclaimer:**  
This project is for **educational and research purposes only**. It is not financial advice or an offer to buy or sell any securities. Trade at your own risk.


