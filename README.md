# Stock-Trend-Analyzer_
# Stock Trend Analyzer

A Python project to analyze Tesla's stock price trends using real-time data. I built this to dig into stock market patterns, experimenting with moving averages and trading signals. It’s my first stab at a data science project, and I’m pretty stoked with how it turned out!

## What It Does
- Fetches the last 30 days of Tesla (TSLA) stock data from Yahoo Finance.
- Calculates 5-day and 20-day moving averages to smooth out price fluctuations.
- Generates buy/sell signals based on MA crossovers (buy when 5-day MA > 20-day MA, sell when it dips below).
- Plots everything with a clean graph, complete with signal markers.

## Tech Stack
- **Python**: Core language.
- **yfinance**: Grabs live stock data.
- **pandas**: Handles the data crunching.
- **matplotlib**: Makes the visuals pop.

## How to Run It
1. Clone this repo: `git clone https://github.com/your-username/Stock-Trend-Analyzer.git`
2. Install dependencies: `pip install yfinance pandas matplotlib`
3. Fire it up: `python stock_fetch.py`
   - You’ll see a table of buy/sell signals in the terminal and a graph window with the results.

## Sample Output
Here’s what I got from my latest run (March 21, 2025):
- **Buy Signals**: None in the last 30 days—price didn’t spike enough.
- **Sell Signal**: Hit one on March 18, 2025 (Close: 225.31, MA5: 240.41, MA20: 278.43). Check `tesla_stock_plot.png` for the visual!

![image](https://github.com/user-attachments/assets/ca6939ab-773c-4178-98ab-f5eb561724d3)


## Challenges & Lessons
- Took me a bit to figure out `pandas` warnings about chained assignments—switched to `.loc` and it’s smooth now.
- Only 30 days of data means fewer signals. Longer periods might show more action.

## What’s Next?
- Maybe add RSI or MACD indicators for deeper analysis.
- Test it on other stocks like AAPL or GOOGL.
- Could turn it into a live dashboard someday.

## Why I Built This
I wanted a project that mixes real-world data with some basic prediction logic—something I could show off for admissions and actually explain. Hope you find it useful or at least kinda cool!

---
Feel free to poke around, fork it, or drop me a note if you’ve got ideas!
