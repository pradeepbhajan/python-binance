# python-binance


import logging
from binance.client import Client
from binance.enums import SIDE_BUY, SIDE_SELL, ORDER_TYPE_MARKET, ORDER_TYPE_LIMIT, TIME_IN_FORCE_GTC
import sys

# 🔹 Logging Setup
logging.basicConfig(filename='trading_bot.log', level=logging.INFO,
                    format='%(asctime)s:%(levelname)s:%(message)s')

class BasicBot:
    def __init__(self, api_key, api_secret, testnet=True):
        self.client = Client(api_key, api_secret, testnet=testnet)
        logging.info("Bot Initialized with Testnet Mode")

    def place_market_order(self, symbol, side, quantity):
        try:
            order = self.client.futures_create_order(
                symbol=symbol,
                side=side,
                type=ORDER_TYPE_MARKET,
                quantity=quantity
            )
            logging.info(f"Market Order Placed: {order}")
            print(f"✅ Market Order Successful: {order}")
        except Exception as e:
            logging.error(f"Market Order Failed: {e}")
            print(f"❌ Error placing market order: {e}")

    def place_limit_order(self, symbol, side, quantity, price):
        try:
            order = self.client.futures_create_order(
                symbol=symbol,
                side=side,
                type=ORDER_TYPE_LIMIT,
                timeInForce=TIME_IN_FORCE_GTC,
                quantity=quantity,
                price=price
            )
            logging.info(f"Limit Order Placed: {order}")
            print(f"✅ Limit Order Successful: {order}")
        except Exception as e:
            logging.error(f"Limit Order Failed: {e}")
            print(f"❌ Error placing limit order: {e}")

# --------- CLI इंटरफ़ेस ---------
if __name__ == "__main__":
    api_key = input("Enter API Key: ")
    api_secret = input("Enter API Secret: ")
    
    bot = BasicBot(api_key, api_secret)

    print("\nSelect Order Type:\n1. Market Order\n2. Limit Order")
    order_type = input("Enter choice (1/2): ")

    symbol = input("Enter Trading Pair (e.g. BTCUSDT): ")
    side = input("Enter Order Side (buy/sell): ").upper()
    quantity = float(input("Enter Quantity: "))

    if order_type == "1":
        bot.place_market_order(symbol, SIDE_BUY if side == "BUY" else SIDE_SELL, quantity)
    else:
        price = float(input("Enter Limit Price: "))
        bot.place_limit_order(symbol, SIDE_BUY if side == "BUY" else SIDE_SELL, quantity, price)


"""Install Dependencies
pip install python-binance
"""
