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
1. Clone this repo: `git clone https://github.com/DKMALHI506/Stock-Trend-Analyzer.git`
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


# My stock analyzer - LIBARIES!
     import yfinance as yf            
     import pandas as pd                
     from datetime import datetime, timedelta  
     import matplotlib.pyplot as plt   

# Date setup - last 30 days
      today = datetime.today()           # Today’s date
      thirty_days_ago = today - timedelta(days=30)  # Rewind 30 days

# Fetch Tesla stock - TSLA’s a fun pick
tesla = yf.Ticker("TSLA")          # Tesla’s ticker
stock_data = tesla.history(start=thirty_days_ago, end=today)  # Last month’s data

# Moving averages - 5 and 20 days
stock_data['FiveDayAvg'] = stock_data['Close'].rolling(window=5).mean()    # 5-day trend
stock_data['TwentyDayAvg'] = stock_data['Close'].rolling(window=20).mean() # 20-day trend

# Buy/sell signals - took some trial
stock_data['TradeSignal'] = 0      # Default: hold (0)
stock_data.loc[stock_data['FiveDayAvg'] > stock_data['TwentyDayAvg'], 'TradeSignal'] = 1   # Buy signal
stock_data.loc[stock_data['FiveDayAvg'] < stock_data['TwentyDayAvg'], 'TradeSignal'] = -1  # Sell signal
stock_data['ChangePoint'] = stock_data['TradeSignal'].diff()  # Signal switches

# Show trades - where I’d act
print("Where I’d buy (signal to 1):")
print(stock_data[stock_data['ChangePoint'] == 1][['Close', 'FiveDayAvg', 'TwentyDayAvg']])
print("\nWhere I’d sell (signal to -1):")
print(stock_data[stock_data['ChangePoint'] == -1][['Close', 'FiveDayAvg', 'TwentyDayAvg']])

# Graph time - took time to polish
plt.figure(figsize=(10, 6))        # Wide size
plt.plot(stock_data.index, stock_data['Close'], color='blue')           # Price in blue
plt.plot(stock_data.index, stock_data['FiveDayAvg'], color='orange', label='5-Day Avg')    # 5-day in orange
plt.plot(stock_data.index, stock_data['TwentyDayAvg'], color='green', label='20-Day Avg')  # 20-day in green

# Markers for trades - triangles rock
plt.scatter(stock_data.index[stock_data['ChangePoint'] == 1], 
            stock_data['Close'][stock_data['ChangePoint'] == 1], 
            color='green', marker='^', label='Buy Here')  # Green up-arrows
plt.scatter(stock_data.index[stock_data['ChangePoint'] == -1], 
            stock_data['Close'][stock_data['ChangePoint'] == -1], 
            color='red', marker='v', label='Sell Here')   # Red down-arrows

# Labels and polish - looks legit
plt.title("Tesla Stock Price - Last 30 Days")  # Clear title
plt.xlabel("Date")                  # X: dates
plt.ylabel("Price in USD")          # Y: cash
plt.grid(True)                      # Grid for clarity
plt.xticks(rotation=45)             # Tilt dates
plt.legend()                        # Line labels

# Save and show - for GitHub
plt.savefig('tesla_price_chart.png')  # Save as PNG
plt.show()                          # Show locally
