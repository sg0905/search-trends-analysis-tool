

---

# Search Interest Analysis with Pytrends

This Python script leverages the `pytrends` library to analyze and visualize Google search interest over time for a set of keywords. It demonstrates how to fetch search interest data, process it, and visualize trends and correlations.

## Requirements

Before running this script, ensure you have the following Python packages installed:

- `pytrends`
- `matplotlib`
- `seaborn`
- `pandas`

You can install the necessary packages using pip:

```bash
pip install pytrends matplotlib seaborn pandas
```

## Script Overview

1. **Setup and Initialization:**
   - Imports the necessary libraries.
   - Initializes the `pytrends` object with specified language and timezone settings.

2. **Data Fetching:**
   - Defines a list of keywords and a timeframe for analysis.
   - Builds the payload and fetches the search interest data over time.

3. **Data Export:**
   - Saves the search interest data to CSV files.
   - Handles errors gracefully if data fetching fails.

4. **Data Visualization:**
   - Plots search interest trends over time for the specified keywords.
   - Plots monthly average search interest trends.
   - Calculates and visualizes the correlation matrix of search interest using a heatmap.

## How to Use

1. **Adjust Keywords and Timeframe:**
   Modify the `keywords` list and `timeframe` string according to your analysis needs. The current example includes keywords like 'Gemini', 'copilot', 'Chatgpt', and 'IBM Watson'.

2. **Run the Script:**
   Execute the script in your Python environment:

   ```bash
   python your_script_name.py
   ```

   Ensure that the script has access to the internet to fetch data from Google Trends.

3. **View Results:**
   - The script generates two CSV files:
     - `search_interest_trends.csv`: Contains the search interest data over time.
     - `search_interest_by_region.csv`: Intended for regional search interest data, but this code doesn't include the fetch for regional data.
   - Visualizations will be displayed for:
     - Search interest trends over time.
     - Monthly search interest trends.
     - Correlation matrix of search interest.

## Script Details

### Data Fetching and Export

```python
pytrends = TrendReq(hl='en-US', tz=360)
keywords = ['Gemini', 'copilot', 'Chatgpt', 'IBM Watson']
timeframe = '2022-01-01 2024-09-01'

pytrends.build_payload(keywords, timeframe=timeframe)
data = pytrends.interest_over_time()
data.to_csv('search_interest_trends.csv')
```

### Data Visualization

```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(14, 7))
for keyword in keywords:
    plt.plot(data.index, data[keyword], label=keyword)
plt.title('Search Interest Over Time')
plt.xlabel('Date')
plt.ylabel('Search Interest')
plt.legend()
plt.grid(True)
plt.show()

monthly_data = data.resample('M').mean()
plt.figure(figsize=(14, 7))
for keyword in keywords:
    plt.plot(monthly_data.index, monthly_data[keyword], label=keyword)
plt.title('Monthly Search Interest Trends')
plt.xlabel('Date')
plt.ylabel('Search Interest')
plt.legend()
plt.grid(True)
plt.show()

correlation_matrix = data.corr()
plt.figure(figsize=(10, 7))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt='.2f')
plt.title('Correlation Matrix of Search Interest')
plt.show()
```

