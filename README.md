# 📊 pairs-trading-strategy

A beginner-to-intermediate quantitative trading strategy built in Python that exploits the **statistical relationship between two correlated stocks** — MSFT and AMZN. The strategy monitors the log-price spread between the pair, computes a rolling z-score, and enters long/short positions when the spread deviates significantly from its historical mean, betting on mean reversion. A full backtest engine simulates realistic P&L with commission costs, and four charts cover normalised prices, z-score signals, rolling correlation, and portfolio performance vs benchmark. This project demonstrates core quant concepts: cointegration intuition, z-score signal generation, market-neutral positioning, and statistical risk metrics.

---

## Strategy Logic

| Z-Score Condition | Action |
|---|---|
| z > +1.5 | **Short** MSFT, **Long** AMZN (spread too wide) |
| z < −1.5 | **Long** MSFT, **Short** AMZN (spread too narrow) |
| \|z\| < 0.5 | **Close** position (spread mean-reverted) |

The spread is defined as `log(MSFT price) − log(AMZN price)`. Rolling 30-day mean and standard deviation are used to normalise it into a z-score.

---

## Quickstart

**Google Colab (recommended)**

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

1. Open a new Colab notebook at [colab.new](https://colab.new)
2. Paste the contents of `Pairs_trading.ipynb` into a cell
3. Uncomment the first line: `!pip install yfinance -q`
4. Run with `Shift + Enter`

**Run locally**

```bash
git clone https://github.com/YOUR_USERNAME/pairs-trading-strategy
cd pairs-trading-strategy
pip install yfinance pandas numpy matplotlib
python Pairs_trading.ipynb
```

---

## Configuration

Edit the constants at the top of `Pairs_trading.ipynb`:

```python
STOCK_A    = "MSFT"    # first leg of the pair
STOCK_B    = "AMZN"    # second leg of the pair
START      = "2020-01-01"
END        = "2024-12-31"
ROLL_WIN   = 30        # rolling window for z-score calculation (days)
Z_ENTRY    = 1.5       # z-score threshold to open a position
Z_EXIT     = 0.5       # z-score threshold to close a position
CAPITAL    = 10_000    # starting portfolio value ($)
COMMISSION = 0.001     # 0.1% per trade
```

---

## Output

The script prints a performance summary and saves `pairs_trading_results.png` with four panels:

- **Normalised prices** — both stocks indexed to 100 for visual comparison
- **Z-Score** — spread deviation with entry/exit bands and shaded active positions
- **Rolling correlation** — 30-day window showing how closely the pair moves together
- **Portfolio vs benchmark** — strategy P&L indexed against MSFT buy-and-hold

---

## Key Limitations

- **No cointegration test** — the pair is assumed to be cointegrated; a real implementation would use the Engle-Granger or Johansen test to confirm this statistically
- **Fixed hedge ratio** — sizing assumes a 1:1 log-price relationship; a proper hedge ratio (via OLS regression) would improve accuracy
- **Lookahead bias risk** — z-score uses a rolling window which is correctly lagged here, but care is needed with any feature engineering additions
- **Transaction costs** — real pairs trades incur borrow costs for the short leg, which are not modelled
- **Regime sensitivity** — if the correlation between the pair breaks down (e.g. during a sector rotation), the strategy can suffer extended losses

---

## Stack

`Python` · `pandas` · `numpy` · `matplotlib` · `yfinance`

---

## License

MIT
