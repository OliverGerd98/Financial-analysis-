import requests
import time

def get_bitcoin_price():
    try:
        url = "https://api.coinbase.com/v2/prices/BTC-USD/spot"
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            return round(float(data['data']['amount']), 2)
        else:
            print(f"Fehler beim Abrufen der Daten. Status Code: {response.status_code}")
            return None
    except Exception as e:
        print(f"Ein Fehler ist aufgetreten: {e}")
        return None

def monitor_bitcoin_price():
    print("Drücke Strg+C, um das Programm zu beenden.")
    while True:
        bitcoin_price = get_bitcoin_price()
        if bitcoin_price is not None:
            print(f"Der aktuelle Bitcoin-Kurs beträgt: ${bitcoin_price:.2f} USD")
        else:
            print("Konnte den Bitcoin-Kurs nicht abrufen.")
        time.sleep(10)

if __name__ == "__main__":
    monitor_bitcoin_price()
