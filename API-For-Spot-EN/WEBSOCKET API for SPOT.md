# WEBSOCKET API for SPOT

## GETTING Started

WebSocket protocol is a new HTML5 protocol, which provides full-duplex communication between web browsers and web servers. Connection can be established after one handshake. Web server can then push business logic data to web browsers.Advantages:

- Request header is small in size (around 2 bytes) during communication
- Web servers and clients can send data bi-directionaly
- Since there is no need to create and delete TCP connection repeatedly, it saves resources

We strongly recommend developers to use Websocket API to access market related information and trading depth.

Should you have any questions, feel free to contact our support team.

## Request Process

OKEx Spot WebSocket URL：`wss://real.okex.com:10441/websocket`  

### Send Request

Request Data Format: 

{'event':'addChannel','channel':'channelValue','parameters':{'api\_key':'value1','sign':'value2'}}.   
Note: 		

'event':'addChannel'(register request)/'removeChannel'(unregister request)

'channel': OKEX provided data type  	  

'parameters': optional field 	

'api\_key' and 'secret\_key' is apply 'apiKey' and 'secretKey' for users    	

Example:

websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_ticker' }").  
websocket.send("[{'event':'addChannel','channel':'ok_sub_spot_X_ticker'},{'event':'addChannel','channel':'ok_sub_spot_X_depth'},{'event':'addChannel','channel':'ok_sub_spot_X_deals'}]"); Support batch register 

#### Server Response
Return Data Format:

[{"channel":"channel","success":"","errorcode":"","data":{}},{"channel":"channel","success":"","errorcode":1,"data":{}}] 
channel: requested data type
result: true,false(applicable to WebSocket Trade API)
data: returned data
errorcode: error code(applicable to WebSocket Trade API)   

#### Data Push Process   
To reduce data size and ensure real-time data push, 'ticker' and 'depth' will only be pushed only when new data is available. 'trades' is newly completed transactions from previous data push to this data push.   

#### How to know whether connection is lost?
OKEX provides heartbeat verification process. Clients send: {'event':'ping'} per 30 seconds, the server returns heartbeats {"event":"pong"} to indicate connection between the clients and the server. If the clients stop receiving the heartbeats, then they will need to reconnect with the server.    

## API Reference  

### Spot Price API 
Receive the latest OKEx Spot data

1. ok\_sub\_spot\_X\_ticker   Subscribe Price Ticker

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_ticker'}");`	
①  value of X is: ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt  

Example

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker'}
# Response
[
    {
        "binary": 0,
        "channel": "ok_sub_spot_bch_btc_ticker",
        "data": {
            "high": "10000",
            "vol": "185.03743858",
            "last": "111",
            "low": "0.00000001",
            "buy": "115",
            "change": "101",
            "sell": "115",
            "dayLow": "0.00000001",
            "dayHigh": "10000",
            "timestamp": 1500444626000
        }
    }
]
```

Returned Value Description	

```
buy(double): best bid
high(double): highest price
last(double): latest price
low(double): lowest price
sell(double): best ask
timestamp(long): timestamp
vol(double: volume(the most recent 24 hours)
```

2. ok\_sub\_spot\_X\_depth   Subscribe Market Depth(Increment)

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_depth'}");`	
①  the value of X is: ltc_btc eth_btc etc_btc bch_btc btc_usdt eth_usdt ltc_usdt etc_usdt bch_usdt etc_eth bt1_btc bt2_btc btg_btc qtum_btc hsr_btc neo_btc gas_btc qtum_usdt hsr_usdt neo_usdt gas_usdt 

Example 

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_depth'}
# Response
[
    {
        "binary": 0,
        "channel": "ok_sub_spot_bch_btc_depth",
        "data": {
            "asks": [],
            "bids": [
                [
                    "115",
                    "1"
                ],
                [
                    "114",
                    "1"
                ],
                [
                    "1E-8",
                    "0.0008792"
                ]
            ],
            "timestamp": 1504529236946
        }
    }
]
```

Returned Value Description	

```
Incremental data return (Return full data for the first query)
Delete (BTC/LTC amount is 0)
Edit (Same price, different amount)
Add (Price inexist)

bids([string, string]):bids depth
asks([string, string]):asks depth
timestamp(long): server timestamp
```

3. ok\_sub\_spot\_X\_depth\_Y   Subscribe Market Depth(Full)

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_depth_Y'}");`	

①  value of X is: ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt  
② value of Y is: 5, 10, 20(Amount) 

Example

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_depth_5'}
# Response
[
    {
        "binary": 0,
        "channel": "ok_sub_spot_bch_btc_depth_5",
        "data": {
            "asks": [],
            "bids": [
                [
                    "115",
                    "1"
                ],
                [
                    "114",
                    "1"
                ],
                [
                    "1E-8",
                    "0.0008792"
                ]
            ],
            "timestamp": 1504529432367
        }
    }
]
```

Returned Value Description	

```
bids[string, string]: bids depth
asks[string, string]: asks depth
timestamp(long): server timestamp
```

4. ok\_sub\_spot\_X\_deals   Subscribe Trade Records

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_deals'}");`	

①  value of X is: ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt  

Example

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_deals'}
# Response
[
   {
    "channel":"ok_sub_spot_bch_btc_deals",
    "data":[["1001","2463.86","0.052","16:34:07","ask"]]
}
]
```

Returned Value Description	

```
[tid, price, amount, time, type]
[string, string, string, string, string]
```

5. ok\_sub\_spot\_X\_kline\_Y   Subscribe Candlestick Chart Data

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_kline_Y'}");`	

① value of X is: ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt  
② value of Y is: 1min, 3min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, day, 3day, week

Example

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_kline_1min'}
# Response
[{
    "channel":"ok_sub_spot_bch_btc_kline_1min",
    "data":[
        ["1490337840000","995.37","996.75","995.36","996.75","9.112"],
        ["1490337840000","995.37","996.75","995.36","996.75","9.112"]
    ]
}]
```

Returned Value Description	

```
[time, open price, highest price, lowest price, close price, volume]
[string, string, string, string, string, string]
```



### Spot Trade API

Easily place spot orders on OKEx

1. login   Login(Personal Information)

Example

```
# Request
{"event":"login","parameters":{"api_key":"xxx","sign":"xxx"}}
# Response
[
    {
        "binary": 0,
        "channel": "login",
        "data": {
            "result": true
        }
    }
]
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|

Description	

```
After subscribing to login also need to subscribe ok_sub_spot_X_order 
transaction data interface, and ok_sub_spot_X_balance account 
information interface.
```


2. ok\_spot\_order   Place Orders

`websocket.send("{'event':'addChannel','channel':'ok_spot_order','parameters': {'api_key':'XXXX','sign':'XXXX','symbol':'bch_btc','type':'buy','price':'50','amount':'0.02'}}")`

Example

```
# Request
{
    'event':'addChannel',
    'channel':'ok_spot_order',
    'parameters':{
        'api_key':'XXXX',
        'sign':'XXXX',
        'symbol':'bch_btc',
        'type':'buy',
        'price':'50',
        'amount':'0.02'
    }
}

# Response
[
    {
        "binary": 0,
        "channel": "ok_spot_order",
        "data": {
            "result": true,
            "order_id": 6189
        }
    }
]
```

Returned Value Description	

```
order_id(long)：order id
result(boolean)：execute result
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt(Support all online currency pairs)|
|type|order Type: limit order (buy/sell) market order (buy\_market/sell\_market)|
|price|order price [limit order buy (required): price between 0~1,000,000 | market order buy (required): BTC:price bigger than (0.01 * best ask price) / LTC :price bigger than (0.1 * best ask price) / ETH :price bigger than (0.01 * best ask price)]|
|amount|order quantity:[limit order sell (required): BTC quantity not less than 0.01 / LTC quantity not less than 0.1 / ETH quantity not less than 0.01| market order sell (required): BTC quantity not less than 0.01 / LTC quantity not less than 0.1 / ETH quantity not less than 0.01]|

3. ok\_spot\_cancel\_order   Cancel Order

`websocket.send("{'event':'addChannel','channel':'ok_spot_cancel_order','parameters': {'api_key':'XXXX','sign':'XXXX','symbol':'bch_btc','order_id':'125433027'}}");`

Example

```
# Request
{
    'event':'addChannel',
    'channel':'ok_spot_cancel_order',
    'parameters':{
        'api_key':'XXXX',
        'sign':'XXXX',
        'symbol':'bch_btc',
        'order_id':'125433027'
    }
}

# Response
[
    {
        "binary": 0,
        "channel": "ok_spot_cancel_order",
        "data": {
            "result": true,
            "order_id": "125433027"
        }
    }
]
```

Returned Value Description	

```
order_id(long)：order id
result(boolean)：execute result
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt|
|order\_id|order ID|


4. ok\_spot\_userinfo   Get User Account Info

`websocket.send("{'event':'addChannel','channel':'ok_spot_userinfo','parameters': {'api_key':'XXXX','sign':'XXXX'}}");`

Example

```
# Request
{
    'event':'addChannel',
    'channel':'ok_spot_userinfo',
    'parameters':{
        'api_key':'XXXX',
        'sign':'XXXX'
    }
}

# Response
[
    {
        "binary": 0,
        "channel": "ok_spot_userinfo",
        "data": {
            "result": true,
            "info": {
                "funds": {
                    "free": {
                        "btc": "5927.8506057903954517",
                        "bch": "49982.615135298639",
                        "bcs": "60000",
                        "etc": "40000",
                        "eth": "30000",
                        "ltc": "20001.09989"
                    },
                    "freezed": {
                        "btc": "7228",
                        "bch": "0",
                        "bcs": "0",
                        "etc": "0",
                        "eth": "0",
                        "ltc": "0"
                    }
                }
            }
        }
    }
]
```

Returned Value Description	

```
free: available fund
freezed: frozen fund
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|


5. ok\_spot\_orderinfo  Get Order Info

`websocket.send("{'event':'addChannel','channel':'ok_spot_orderinfo','parameters': {'api_key':'XXXX','sign':'XXXX','symbol':'bch_btc','order_id':'20914907'}}");`

Example

```
# Request
{
    'event':'addChannel',
    'channel':'ok_spot_orderinfo',
    'parameters':{
        'api_key':'XXXX',
        'sign':'XXXX',
        'symbol':'bch_btc',
        'order_id':'20914907'
    }
}

# Response
[
    {
        "binary": 0,
        "channel": "ok_spot_orderinfo",
        "data": {
            "result": true,
            "orders": [
                {
                    "symbol": "bch_btc",
                    "amount": "0.10000000",
                    "price": "1.00000000",
                    "avg_price": 0,
                    "create_date": 1504529828000,
                    "type": "buy",
                    "deal_amount": 0,
                    "order_id": 6189,
                    "status": -1
                }
            ]
        }
    }
]
```

Returned Value Description	

```
amount(double): order quantity
create_date(double): order time
avg_price(double): average transaction price
deal_amount(double): filled quantity
order_id(long): order ID
orders_id(long): order ID (Deprecated)
price(double): order price
status(int): -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process,5 = cancel request in process
symbol(string):ltc_btc  eth_btc  etc_btc  bch_btc  btc_usdt  eth_usdt  ltc_usdt  etc_usdt  bch_usdt  etc_eth  bt1_btc  bt2_btc  btg_btc qtum_btc hsr_btc neo_btc gas_btc qtum_usdt hsr_usdt neo_usdt gas_usdt
type(string): buy_market = market buy order, sell_market = market sell order
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt|
|order\_id|order ID|

6. ok\_sub\_spot\_X_order  Trade Record

Example

```

# Response
[
    {
        "base": "bch",
        "binary": 0,
        "channel": "ok_sub_spot_bch_btc_order",
        "data": {
            "symbol": "bch_btc",
            "tradeAmount": "1.00000000",
            "createdDate": "1504530228987",
            "orderId": 6191,
            "completedTradeAmount": "0.00000000",
            "averagePrice": "0",
            "tradePrice": "0.00000000",
            "tradeType": "buy",
            "status": 0,
            "tradeUnitPrice": "113.00000000"
        },
        "product": "spot",
        "quote": "btc",
        "type": "balance"
    }
]
```

Returned Value Description	

```
createdDate(string): created date
orderId(long): order ID
tradeType(string): trade type (buy: buy;sell: sell;buy_market: buy at market price;sell_market: sell at market price)
sigTradeAmount(string):quantity in a single transaction
sigTradePrice(string):rate in a single transaction
tradeAmount(string):for market sell orders: total sell quantity, for limit orders: order quantity
tradeUnitPrice(string):for market buy orders: total buy amount, for limit orders: order rate
symbol(string):ltc_btc  eth_btc  etc_btc  bch_btc  btc_usdt  eth_usdt  ltc_usdt  etc_usdt  bch_usdt  etc_eth  bt1_btc  bt2_btc  btg_btc qtum_btc hsr_btc neo_btc gas_btc qtum_usdt hsr_usdt neo_usdt gas_usdt
completedTradeAmount(string): filled 'tradeAmount'
tradePrice(string): filled amount
averagePrice(string): average transaction price
unTrade(string): unfilled amount in usd for market order, unfilled coin quantity for limit order
status(int): -1: cancelled, 0: pending, 1: partially filled, 2: fully filled, 4: cancel request in process
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|symbol|ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt (Support all online currency pairs)|

7. ok\_sub\_spot\_X\_balance  User Account Info


Example

```

# Response
[
    {
        "base": "bch",
        "binary": 0,
        "channel": "ok_sub_spot_bch_btc_balance",
        "data": {
            "info": {
                "free": {
                    "btc": 5814.850605790395
                },
                "freezed": {
                    "btc": 7341
                }
            }
        },
        "product": "spot",
        "quote": "btc",
        "type": "order"
    }
]```

Returned Value Description	

```
free: available fund
freezed: frozen fund
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|symbol|ltc\_btc eth\_btc etc\_btc bch\_btc btc\_usdt eth\_usdt ltc\_usdt etc\_usdt bch\_usdt etc\_eth bt1\_btc bt2\_btc btg\_btc qtum\_btc hsr\_btc neo\_btc gas\_btc qtum\_usdt hsr\_usdt neo\_usdt gas\_usdt(Support all online currency pairs)|

