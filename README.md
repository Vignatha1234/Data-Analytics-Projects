import requests
import pandas as pd
from datetime import datetime

def fetch_crypto_data(coin="bitcoin", vs_currency="usd", days=365):
    url = f"https://api.coingecko.com/api/v3/coins/{coin}/market_chart"
    params = {
        "vs_currency": vs_currency,
        "days": days,
        "interval": "daily"
    }
    response = requests.get(url, params=params)
    response.raise_for_status()
    data = response.json()

    prices = pd.DataFrame(data["prices"], columns=["timestamp", "price"])
    prices["timestamp"] = pd.to_datetime(prices["timestamp"], unit="ms")
    prices.set_index("timestamp", inplace=True)

    return prices

df = fetch_crypto_data("bitcoin")
print(df.head())
