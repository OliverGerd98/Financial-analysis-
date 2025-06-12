--------------------------------

DCFs (V11) and DCF-Comps for Payment Provider online  

---------------------------------





----------------------------------------
Bitget Code -> 

import hmac
import base64
import json
import time
import requests

api_key = ""
secret_key = ""
passphrase = ""

symbol = "BTCUSDT_UMCBL"
margin_coin = "USDT"
limit_price = "96000"
leverage = 20
invested_amount = 5

order_size = round((invested_amount * leverage) / float(limit_price), 6)
tp_price = "94000"  # Take Profit für Short (unterhalb von 96000)
sl_price = "98000"  # Stop Loss für Short (oberhalb von 96000)

base_url = "https://api.bitget.com"
place_order_url = "/api/mix/v1/order/placeOrder"

def generate_signature(secret_key, method, url, timestamp, params):
    data_to_sign = f"{timestamp}{method.upper()}{url}{json.dumps(params)}"
    signature = hmac.new(secret_key.encode('utf-8'), data_to_sign.encode('utf-8'), digestmod='sha256').digest()
    return base64.b64encode(signature).decode()

def place_order():
    timestamp = str(int(time.time() * 1000))
    headers = {
        "ACCESS-KEY": api_key,
        "ACCESS-SIGN": "",
        "ACCESS-TIMESTAMP": timestamp,
        "ACCESS-PASSPHRASE": passphrase,
        "Content-Type": "application/json"
    }
    params = {
        "symbol": symbol,
        "marginCoin": margin_coin,
        "side": "open_short",  # Short-Position
        "orderType": "limit",
        "price": limit_price,
        "size": str(order_size),
        "timeInForceValue": "normal",
        "presetTakeProfitPrice": tp_price,
        "presetStopLossPrice": sl_price
    }
    headers["ACCESS-SIGN"] = generate_signature(secret_key, "POST", place_order_url, timestamp, params)
    try:
        response = requests.post(base_url + place_order_url, headers=headers, json=params)
        if response.status_code == 200:
            print("Order erfolgreich:", response.json())
        else:
            print("Fehler beim Platzieren der Order:", response.status_code, response.text)
    except Exception as e:
        print("Exception aufgetreten:", str(e))

place_order()

------------------------------

