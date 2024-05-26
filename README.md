# -Exploring-the-Bitcoin-Cryptocurrency-Market-DataCamp-project

This project involves fetching the latest cryptocurrency data from the CoinMarketCap API, processing the data using Pandas, and visualizing various aspects of the market using Matplotlib.

## Table of Contents
1. [Installation](#installation)
2. [Usage](#usage)
3. [Features](#features)
4. [License](#license)

## Installation

To get started with this project, clone the repository and install the necessary dependencies.

```bash
git clone https://github.com/yourusername/crypto-market-analysis.git
cd crypto-market-analysis
pip install -r requirements.txt
```

## Usage

1. **Fetch Cryptocurrency Data**: The script fetches the latest cryptocurrency listings from the CoinMarketCap API.

2. **Process Data with Pandas**: The data is then processed to create a DataFrame that contains information about various cryptocurrencies, including market capitalization, circulating supply, and price changes.

3. **Visualize Data with Matplotlib**: The processed data is visualized using bar charts to show the top cryptocurrencies by market capitalization, the most volatile cryptocurrencies, and more.

### Example

Here's a simple example of how to run the script:

```python
import pandas as pd
import matplotlib.pyplot as plt
from requests import Session, Request
from requests.exceptions import ConnectionError, Timeout, TooManyRedirects
import json

# API URL and parameters
url = 'https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest'
parameters = {
    'start': '1',
    'limit': '5000',
    'convert': 'USD'
}
headers = {
    'Accepts': 'application/json',
    'X-CMC_PRO_API_KEY': 'your_api_key',
}

# Fetch data
session = Session()
session.headers.update(headers)

try:
    response = session.get(url, params=parameters)
    data = json.loads(response.text)
    current = pd.DataFrame(data['data'])
    print(current.head())
except (ConnectionError, Timeout, TooManyRedirects) as e:
    print(e)

# Load historical data
dec6 = pd.read_csv('datasets/coinmarketcap_06122017.csv')
market_cap_raw = dec6[['id', 'market_cap_usd']]

# Process data
market_cap_raw = market_cap_raw.query('market_cap_usd > 0')
market_cap_raw = market_cap_raw.assign(market_cap_perc=lambda x: (x.market_cap_usd / market_cap_raw.market_cap_usd.sum()) * 100)

# Plot data
cap10 = market_cap_raw.head(10).set_index('id')
cap10.plot.bar(y='market_cap_usd', logy=True, title='Top 10 Cryptocurrencies by Market Cap')
plt.show()
```

## Features

- **Fetch Latest Cryptocurrency Data**: Get up-to-date information on the top 5000 cryptocurrencies from the CoinMarketCap API.
- **Process Data with Pandas**: Clean and process the data to make it ready for analysis and visualization.
- **Visualize Data with Matplotlib**: Create various visualizations to better understand the cryptocurrency market, including:
  - Top 10 cryptocurrencies by market capitalization
  - Most volatile cryptocurrencies over 24 hours and 7 days
  - Distribution of cryptocurrencies based on market cap size

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Feel free to contribute to this project by creating issues or submitting pull requests on GitHub. For any questions or feedback, please contact [your email address].
