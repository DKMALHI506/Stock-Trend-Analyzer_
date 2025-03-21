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


# My stock trend analyzer script - took me a while to get this working!
  
   # Yahoo Finance API is super handy for stock data
      import yfinance as yf
import pandas as pd  # Pandas is my go-to for messing with tables
from datetime import datetime, timedelta  # Needed these for the date stuff
import matplotlib.pyplot as plt  # Plotting’s the fun part!

# Setting up the date range - just the last 30 days to keep it simple
today = datetime.today()  # Grabs today’s date
thirty_days_ago = today - timedelta(days=30)  # Backtracks 30 days

# Pulling Tesla’s stock data - TSLA seemed like a cool choice
tesla = yf.Ticker("TSLA")  # Ticker object for Tesla
stock_data = tesla.history(start=thirty_days_ago, end=today)  # Gets the last month’s prices

# Adding moving averages - 5 and 20 days felt like a good combo
stock_data['FiveDayAvg'] = stock_data['Close'].rolling(window=5).mean()  # Short-term trend
stock_data['TwentyDayAvg'] = stock_data['Close'].rolling(window=20).mean()  # Longer-term trend

# Figuring out buy/sell signals - took some trial and error here!
stock_data['TradeSignal'] = 0  # Start with neutral (0)
stock_data.loc[stock_data['FiveDayAvg'] > stock_data['TwentyDayAvg'], 'TradeSignal'] = 1  # Buy when short crosses over long
stock_data.loc[stock_data['FiveDayAvg'] < stock_data['TwentyDayAvg'], 'TradeSignal'] = -1  # Sell when it dips below
stock_data['ChangePoint'] = stock_data['TradeSignal'].diff()  # Spots where signals flip

# Printing out the key moments - wanted to see where I’d trade
print("Here’s where I’d buy (signal jumps to 1):")
print(stock_data[stock_data['ChangePoint'] == 1][['Close', 'FiveDayAvg', 'TwentyDayAvg']])
print("\nAnd here’s where I’d sell (signal drops to -1):")
print(stock_data[stock_data['ChangePoint'] == -1][['Close', 'FiveDayAvg', 'TwentyDayAvg']])

# Plotting it all - spent way too long making this look decent
plt.figure(figsize=(10, 6))  # Bigger size so it’s not squished
plt.plot(stock_data.index, stock_data['Close'], color='blue')  # The raw price line
plt.plot(stock_data.index, stock_data['FiveDayAvg'], color='orange', label='5-Day Avg')  # Orange for the short avg
plt.plot(stock_data.index, stock_data['TwentyDayAvg'], color='green', label='20-Day Avg')  # Green for the long avg

# Adding buy/sell markers - these triangles make it pop!
plt.scatter(stock_data.index[stock_data['ChangePoint'] == 1], 
            stock_data['Close'][stock_data['ChangePoint'] == 1], 
            color='green', marker='^', label='Buy Here')  # Green up-arrows for buys
plt.scatter(stock_data.index[stock_data['ChangePoint'] == -1], 
            stock_data['Close'][stock_data['ChangePoint'] == -1], 
            color='red', marker='v', label='Sell Here')  # Red down-arrows for sells

# Making it readable - titles and labels are a must
plt.title("Tesla Stock Price - Last 30 Days")  # Simple title
plt.xlabel("Date")  # X-axis is time
plt.ylabel("Price in USD")  # Y-axis is money
plt.grid(True)  # Grid helps track the numbers
plt.xticks(rotation=45)  # Tilted dates so they don’t overlap
plt.legend()  # Legend to tell what’s what

# Saving the plot - nice to have for my GitHub
plt.savefig('tesla_price_chart.png')  # Saves it as a PNG
plt.show()  # Pops up the graph on my screen
