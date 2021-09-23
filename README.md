# Shrimpy-Python-Api-project 3.9.5
trying to get this running

# import the Shrimpy library for crypto exchange websockets
import shrimpy

# a sample error handler, it simply prints the incoming error
def error_handler(err):
    print(err)

exchanges_bbo = {}

# define the handler to manage the output stream
def handler(msg):
    bid_price = msg['content']['bids'][0]['price']
    ask_price = msg['content']['asks'][0]['price']
    exchanges_bbo[msg['exchange']] = {'bid': float(bid_price), 'ask': float(ask_price)}
    best_bid = 0.0
    best_ask = 100000.0
    best_bid_exchange = ''
    best_ask_exchange = ''
    for key, value in exchanges_bbo.items():
        if value['bid'] > best_bid:
            best_bid = value['bid']
            best_bid_exchange = key
        if value['ask'] < best_ask:
            best_ask = value['ask']
            best_ask_exchange = key
    if best_bid > best_ask:
        print("sell on " + best_bid_exchange + " for " + str(best_bid))
        print("buy on " + best_ask_exchange + " for " + str(best_ask))
    else:
        print("No Arbitrage Available")


# input your Shrimpy public and private key
public_key = '560dd21601968bdc51d01f2bf3c200cea7de7ba22bcde8f4c417114d76c426c4'
private_key = '94b9505918679dc83360e07b277fbca38155ebe7229407d68a379209211a68733125056876d0b31c8f3e62814fdc315a55ca6c4b3f832f05e04348a5b7de1851'

# create the Shrimpy websocket client
api_client = shrimpy.ShrimpyApiClient(public_key, private_key)
raw_token = api_client.get_token()
client = shrimpy.ShrimpyWsClient(error_handler, raw_token['token'])

# connect to the Shrimpy websocket and subscribe
client.connect()

# select exchanges to arbitrage
exchanges = ["coinbase", "binance", "gemini"]
pair = "btc-usdt"

# subscribe to the websockets for the given pair on each exchange
for exchange in exchanges:
    subscribe_data = {
        "type": "subscribe",
        "exchange": exchange,
        "pair": pair,
        "channel": "bbo"
    }
    client.subscribe(subscribe_data, handler)







result:

Exception in thread Thread-1:
Traceback (most recent call last):

line 954, in _bootstrap_inner
    self.run()
    
    line 892, in run  self._target(*self._args, **self._kwargs)
    
    in _run_socket_thread
    for task in asyncio.all_tasks():
    
    loop = asyncio.get_running_loop()
RuntimeError: no running event loop line 45
    
    
