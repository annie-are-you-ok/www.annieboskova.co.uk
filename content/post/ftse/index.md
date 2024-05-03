---
title: FTSE Market Summary & Portfolio Analysis
date: '2024-05-03'
---


KATE expects your code to define variables with specific names that correspond to certain things we are interested in.

KATE will run your notebook from top to bottom and check the latest value of those variables, so make sure you don't overwrite them.

* Remember to uncomment the line assigning the variable to your answer and don't change the variable or function names.
* Use copies of the original or previous DataFrames to make sure you do not overwrite them by mistake.

You will find instructions below about how to define each variable.

Once you're happy with your code, upload your notebook to KATE to check your feedback.

## About the Dataset

In the following section, you will be analysing **The Financial Times Stock Exchange 100 (FTSE 100) Index** data, pronounced, the 'Footsie hundred', is a share index of the 100 companies listed on the London Stock Exchange with the highest market capitalisation. 

The dataset includes information about:

- Company - Name of  the publicly traded company

- Ticker - A ticker or stock symbol is an abbreviation used to uniquely identify a stock on a particular stock market

- Sector - Industry in which the company operates in

- Mid-price (p) - Average between the 'buying' and the 'selling' price of a particular stock

- Change - Difference between the current price and the previous day's market price of a particular stock
    - A positive change is what allows investors to make a profit.

- Our view - View of the analyst or the brokerage firm collected this data

- Brokers - Total number of brokerage firms tracking and analysing the stock

- 'Strong Buy', 'Buy', 'Neutral', 'Sell', 'Strong Sell' - columns indicates the weighted verdict of all Brokers 

- Recommendation - Final verdict or recommendation for the stock
    - Overweight/Outperform means "buy", investors should assign a higher weighting to the stock in portfolios or funds 
    - Underweight/Underperform means "sell" or "don't buy" recommendation for a specific stock

## Importing Pandas Library

Make sure you run the following code cell import Pandas Library before you attempt any of the questions

```python
import pandas as pd
import numpy as np
```

## Importing the dataset

First use the `.read_csv()` function to import the file `FTSE100.csv` from the `data` folder, assigning it to `df` DataFrame. 

After using the `.head()` method to look at the first five rows of the `df` DataFrame. Also, consider using `.info()` method to further explore your data.

```python
df = pd.read_csv('data/FTSE100.csv')
df.head()
```

```python
df.info()
```

## 1. Tidy Data

The dataset has a column with only `n/a` values, and also 101 rows (you may have been expecting 100!). This is because one of the companies (Royal Dutch Shell) has two entries. We can get rid of the first instance of these (RDSA).

Starting from a copy of `df`, create a new DataFrame called `clean_df` with the following changes:
- Drop the row with a `Ticker` value of `RDSA`
- Drop the `Strong Buy` column

```python
#add your code below
#Make sure you call your new DataFrame clean_df
clean_df = df.copy() # clean_df is a copy of df

clean_df = clean_df.drop('Strong Buy', axis=1)
clean_df = clean_df.drop(clean_df[clean_df['Ticker'] == 'RDSA'].index)
```

```python
clean_df.head()
```

```python
clean_df.info()
```

## 2. Change Column Data Type

Look at the values in the `Mid-price (p)` column. At first glance they may look like floats but in fact they have been interpreted as text. We need to change them to floats for them to be more useful.

Starting from a copy of `clean_df`, create a new DataFrame called  `price_df` with the following change:

- Convert the values in the `Mid-price (p)` column to floats (keeping the column in the same place)

*Hint: converting directly to a float may not work if there are commas in the strings; you may find the [str.replace](https://docs.python.org/3/library/stdtypes.html#str.replace) method useful for fixing this before conversion.*

```python
#add your code below
#Make sure you call your new DataFrame price_df

def convert_to_float(x):
    try: 
        return float(x.replace(',', '')) #if x has commas, replace with '' to remove them
    except:
        return np.nan #otherwise return a NaN value

price_df = clean_df.copy()
price_df['Mid-price (p)'] = price_df['Mid-price (p)'].apply(convert_to_float)
```

```python
price_df.head()
```

```python
price_df.info()
```

## 3. Format Change Values

Take a look at the values in the `Change` column. You'll see that they are in an inconsistent format, and stored as strings. The positive values need to be multiplied by 100. The negative values need to have the `%` sign removed.

**Part 1:** Create a function called `format_change` which takes a string such as those in the `Change` column and does the following:
1. If the last character is a % sign, remove it 
2. Convert the string to a float
3. If that float is posiive, multiply it by 100
4. Return the resulting float

*Hint: to convert string to a float you may find [float()](https://www.w3schools.com/python/ref_func_float.asp) function useful*

```python
price_df['Change']
```

- if negative, remove %
- convert to float
- if positive, *100
- return the float

```python
#add your code below
def format_change(string):
    
    if string[-1] == '%': 
        string = string.replace('%', '') #if %, remove it
   
    string = float(string)
    
    if string >= 0:
        string = string * 100
    
    return string
```

Uncomment and run the following code cell to test that your function works as expected:

```python
# def format_change(string):
#     if string[-1] == '%':
#         float(string.replace('%', ''))
#     else: 
#         float(string)
#         return string
```

```python
format_change('0.45%')
```

```python
format_change('45.45%')
```

```python
format_change('-0.59%')
```

**Part 2:** Starting from a copy of `price_df`, create a new DataFrame called  `change_df` with a new column called `Change (%)`:

- This should contain the result returned from the function created above when given the original `Change` column value as the argument
  

```python
#add your code below
#Make sure you call your new DataFrame change_df

change_df = price_df.copy()
change_df['Change %'] = change_df['Change'].apply(format_change)
```

```python
change_df.head()
```

## 4. Holding Summary

You are given the details of a share holding in a tuple, containing the company ticker code, number of shares, and price paid. Make sure you run the following code cell before attempting the question.

```python
holding = ('BP.', 500, 1535.21)

print(holding[0])  #ticker code
print(holding[1])  #number of shares
print(holding[2])  #price paid
```

Starting from the `holding` above and `change_df`, build a new dictionary containing the following keys and the appropriate values in the given data formats.

```
{    
    holding_cost: float,    
    # The cost (in £, not pence) of the holding (number of shares * price paid)
    holding_value: float,    
    # The value (in £, not pence) of the holding (number of shares * current mid-price) 
    change_in_value: float,    
    # The percentage change from the original cost of the holding to the current value  
    (e.g. if the holding_value is 1.2 * the holding_cost, the change_in_value should equal 20.0)
    
}
```

Call this dictionary `holding_dict`

*Hint: If you want to get the first item in a series of values (such as a column of a filtered DataFrame), you can use* `.values[0]` *on the column*

```python
row_number = change_df.index.get_loc(change_df[change_df['Ticker'] == holding[0]].index[0])
row_number
```

```python
#add your code below
#Make sure you call your new dictionary holding_dict

#number of shares * price paid, conv into £
holding_cost = (holding[1] * holding[2])/100 #number of shares *
# print(holding_cost)

#number of shares * current mid-price, conv into £
holding_value = (holding[1] * change_df['Mid-price (p)'].values[row_number]) / 100 
# print(holding_value)

change_in_value = holding_cost / holding_value *100
# print(change_in_value)

holding_dict = {
    'holding_cost(£)': holding_cost,
    'holding_value(£)': holding_value,
    'change_in_value(%)': change_in_value    
}

holding_dict
```

## 5. Market Comparison

Provided with the DataFrame you processed above, `change_df`, we would like to compare the % change in the mid-price for each company to the average % change for all companies in the market, along with a summary of the broker recommendations.

Create a DataFrame called `comparison_df` with the following columns added to a copy of `change_df`: 

- **'Beat Market'** - This should be a Boolean column with `True` for stocks where `Change (%)` exceeds that of the average market change
- **'Buy Ratio'** - This should equal the `Buy` column divided by the `Brokers` column

*Hint: Calculate the average market change % first then compare each value in the `Change (%)` column to that when creating the new column*

```python
change_df.head(2)
```

average market change = 'Change %'.mean
create new columns:
- 'Beat Market' = bool values, change % > ave_market_change
- 'Buy Ratio' = comparison_df['Buy'] / comparison_df['Brokers']

```python
#add your code below
#Make sure you call your new DataFrame comparison_df

comparison_df = change_df.copy()

ave_market_change = comparison_df['Change %'].mean()
print(ave_market_change)

comparison_df['Beat Market'] = comparison_df['Change %'] > ave_market_change
comparison_df['Buy Ratio'] = comparison_df['Buy'] / comparison_df['Brokers']
```

```python
comparison_df.head(7)
```

## 6. Investigate

We want to identify any companies which match a given set of rules, so that we can look into them further.   

We want to identify companies in `watchlist` that meet at least one of the following requirements:

i) Any company in `watchlist` whose prices is equal to or lower than the given target price.

ii) Any company in `watchlist` where `Buy Ratio` is 0.5 or greater.

Using the `watchlist` below and `comparison_df` you defined, create a list of companies meeting the requirements, call this list `companies_list`. The list should only have the company names, not the price.

Note: **A company meeting both requirements should only appear once in the list**.

*Hint: create an empty list to add company names to, using a loop to work through the watchlist. If you want to get the first item in a series of values (such as a column of a filtered DataFrame), you can use* `.values[0]` *on the column.*

- prices <= given target price
- comparison_df['Buy Ratio'] >= 0.5
- A company meeting both requirements should only appear once in the list

```python
watchlist = [('TUI', 820.0), ('Whitbread', 4300.0), ('AstraZeneca', 7500.0), 
             ('Standard Chartered', 920.0), ('Barclays', 135.5)]
watchlist
```

```python
# ticker_midprice = comparison_df[['Ticker', 'Mid-price (p)']].values
ticker_midprice = comparison_df[['Ticker', 'Mid-price (p)']].values[0]
ticker_midprice
```

```python
print(df.loc[6, 'Mid-price (p)'])

#7368.0
```

```python
#add your code below
#Make sure you call your new empty list companies_list

companies_list = [] #company names only

for comp in watchlist:
    
    ticker = comp[0]

#     row = comparison_df.loc[comparison_df['Ticker'] == 'TUI'].index[0]
    row_number = comparison_df.index.get_loc(comparison_df[comparison_df['Ticker'] == ticker].index[0])
#     print(comparison_df['Mid-price (p)'][row_number])

#     if comparison_df['Mid-price (p)'] <= comp[1]: #if mid_price <= target_price
#         companies_list.append(comp[0])
#     if 
# print(companies_list)
```

```python
# row_number = comparison_df.loc[comparison_df['Ticker'] == "Whitbread"]
# row_number
```

```python
# row_number = comparison_df.index.get_loc(change_df[change_df['Ticker'] == 'Whitbread'])
# row_number[0]
```
