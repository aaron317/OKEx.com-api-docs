# REST API for SPOT    

## Getting Started    

REST , a.k.a Respresntational State Transfer, is one of the most common web services protocol. REST emphasis on its readability, standardization, interoperabilty and scalability.Advantages: 
   
- Each URL respresented one web resources in RESTful architecture;
- Act as a representation of resources between client and server;
- Client-end is enabled to operate server-side resources with 4 HTTP requests representational state transfer.   

We recommend developers to use REST API to proceed spot trading and withdrawals.

In case of any problem, please contact our support team.     
    
## Request Process    

The root url is:`https://www.okex.com/api/v1`     

All requests go over https protocol, The field **contentType** in request header should be:`application/x-www-form-urlencoded`    
	
There are four simple steps to the request process.  
  
1. Request parameter: construct the request parameters according to each API interface.
2. Submit request: use POST or GET method to send request to server.
3. Server respond: server processes request and returns data to users in JSON format after authentication check.
4. Data processing: the client processes the returned data.

## API Reference  

### Spot Price API 

Receive the latest OKEx spot market data  

1. Get /api/v1/ticker    Get Price Ticker

URL `https://www.okex.com/api/v1/ticker.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/ticker.do?symbol=ltc_btc
# Response
{
	"date":"1410431279",
	"ticker":{
		"buy":"33.15",
		"high":"34.15",
		"last":"33.15",
		"low":"32.05",
		"sell":"33.16",
		"vol":"10532696.39199642"
	}
}
```

Return Values	

```
date: server time for returned data
buy: best bid
high: highest price
last: latest price
low: lowest price
sell: best ask
vol: volume (in the last rolling 24 hours)
```

Request Parameters	

|Parameter|Description|
| :-----    | :-----   |
|symbol| Pairs like : ltc\_btc  etc\_btc|

2. Get /api/v1/depth   Get Market Depth

URL `https://www.okex.com/api/v1/depth.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/depth.do?symbol=ltc_btc
# Response
{
	"asks": [
		[792, 5],
		[789.68, 0.018],
		[788.99, 0.042],
		[788.43, 0.036],
		[787.27, 0.02]
	],
	"bids": [
		[787.1, 0.35],
		[787, 12.071],
		[786.5, 0.014],
		[786.2, 0.38],
		[786, 3.217],
		[785.3, 5.322],
		[785.04, 5.04]
	]
}
```

Return Values	

```
asks : ask depth
bids : bid depth
```

Request Parameters	

|Parameter|		Description|
| :-----     | :-----   |
|symbol|Pairs like : ltc\_btc  etc\_btc|
|size|value: must be between 1 - 200|

3. Get /api/v1/trades   Get Trade Recently 60

URL `https://www.okex.com/api/v1/trades.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/trades.do?symbol=ltc_btc&since=7622718804
# Response
[
	{
        "date": "1367130137",
		"date_ms": "1367130137000",
		"price": 787.71,
		"amount": 0.003,
		"tid": "230433",
		"type": "sell"
	},
	{
        "date": "1367130137",
		"date_ms": "1367130137000",
		"price": 787.65,
		"amount": 0.001,
		"tid": "230434",
		"type": "sell"
	},
	{
		"date": "1367130137",
		"date_ms": "1367130137000",
		"price": 787.5,
		"amount": 0.091,
		"tid": "230435",
		"type": "sell"
	}
]
```

Return Values	

```
date: transaction time
date_ms: transaction time in milliseconds
price: transaction price
amount: quantity in BTC (or LTC)
tid: transaction ID
type: buy/sell
```

Request Parameters	

|Parameter|		Description|
| :-----     | :-----   |
|symbol|Pairs like : ltc\_btc  etc\_btc|
|since|get recently 60 pieces of data starting from the given tid (optional)|

4. Get /api/v1/kline    Get Candlestick Data

URL `https://www.okex.com/api/v1/kline.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/kline.do?symbol=ltc_btc&type=1min
# Response
[
    [
        1417449600000,
        2339.11,
        2383.15,
        2322,
        2369.85,
        83850.06
    ],
    [
        1417536000000,
        2370.16,
        2380,
        2352,
        2367.37,
        17259.83
    ]
]
```

Return Values	

```
[
	1417564800000,	timestamp
	384.47,		open
	387.13,		high
	383.5,		low
	387.13,		close
	1062.04,	volume
]
```

Request Parameters	

|Parameter|	Description|
| :-----    | :-----   |
|symbol|Pairs like : ltc\_btc  etc\_btc|
|type|1min/3min/5min/15min/30min/1day/3day/1week/1hour/2hour/4hour/6hour/12hour|
|size|specify data size to be acquired|
|since|timestamp(eg:1417536000000). data after the timestamp will be returned|

### Spot Trading API 

Place spot orders on OKEx 

1. POST /api/v1/userinfo    Get User Account Info

URL `https://www.okex.com/api/v1/userinfo.do`  
	Request frequency 6 times/2s    

Example	

```
# Request
POST https://www.okex.com/api/v1/userinfo.do
# Response
{
    "info": {
        "funds": {
            "free": {
                "btc": "0",
                "ltc": "0",
                "eth": "0"
            },
            "freezed": {
                "btc": "0",
                "ltc": "0",
                "eth": "0"
            }
        }
    },
    "result": true
}
```

Return Values	

```
free: available fund
freezed: frozen fund
```

Request Parameters	

|Parameter|Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|

2. POST /api/v1/trade    Place Orders

URL `https://www.okex.com/api/v1/trade.do`  
	Request frequency 20 times/2s	

Example	

```
# Request
POST https://www.okex.com/api/v1/trade.do
# Response
{"result":true,"order_id":123456}
```

Return Values	

```
result: true means order placed successfully
order_id: order ID of the newly placed order
```

Request Parameters	

|Parameter|		Description|
| :-----       | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|type|order type: limit order(buy/sell) market order(buy_market/sell_market)|
|price|order price. For limit orders, the price must be between 0~1,000,000. IMPORTANT: for market buy orders, the price is to total amount you want to buy, and it must be higher than the current price of 0.01 BTC (minimum buying unit), 0.1 LTC or 0.01 ETH. For market sell orders, the price is not required|
|amount|order quantity. Must be higher than 0.01 for BTC, 0.1 for LTC or 0.01 for ETH.For market buy roders, the amount is not required|
|sign|signature of request parameters|

3. POST /api/v1/batch_trade    Batch Trade

URL `https://www.okex.com/api/v1/batch_trade.do`  
	Request frequency 20 times/2s	

Example	

```
# Request
POST https://www.okex.com/api/v1/batch_trade.do
# Response
{
	"order_info":[
		{"order_id":41724206},
		{"error_code":10011,"order_id":-1},
		{"error_code":10014,"order_id":-1}
	],
	"result":true
}
```

Return Values	

```
result: true indicates order successfully placed
order_id: order ID
Note: return true if any one order is placed successfully
     returned orders info and 'orders_data' are in same sequence
     if fail to place order: order_id is -1
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|type|optional, order type for limit orders (buy/sell)|
|orders_data|JSON string Example: [{price:3,amount:5,type:'sell'},{price:3,amount:3,type:'buy'},{price:3,amount:3}] max order number is 5，for 'price' and 'amount' parameter, refer to trade/API. Final order type is decided primarily by 'type' field within 'orders\_data' and subsequently by 'type' field (if no 'type' is provided within 'orders\_data' field)|
|sign|signature of request parameters|

4. POST /api/v1/cancel_order    Cancel Orders

URL `https://www.okex.com/api/v1/cancel_order.do`  
	Request frequency 20 times/2s

Example	

```
# Request
POST https://www.okex.com/api/v1/cancel_order.do
# Response
# For batch orders, return (success order IDs, failed order IDs)
{"success":"123456,123457","error":"123458,123459"}
```

Return Values	

```
result: true: cancel order request successful, wait to be processed, false: cancel order failed(applicable to single order)
order_id: order ID (applicable to single order)
success: ID of orders whose cancel request are successful, wait to be processed.(applicable to batch orders)
error: ID of orders whose cancel request are unsuccessful.(applicable to batch orders)
```

Request Parameters	

|Parameter|		Description|
| :-----  | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|order_id|order ID (multiple orders are separated by a comma ',', Max of 3 orders are allowed per request)|
|sign|signature of request parameters|

5. POST /api/v1/order_info    Get Order Info

URL `https://www.okex.com/api/v1/order_info.do`  
	Request frequency 20 times/2s (unfilled orders)

Example	

```
# Request
POST https://www.okex.com/api/v1/order_info.do
# Response
{
    "result": true,
    "orders": [
        {
            "amount": 0.1,
            "avg_price": 0,
            "create_date": 1418008467000,
            "deal_amount": 0,
            "order_id": 10000591,
            "orders_id": 10000591,
            "price": 500,
            "status": 0,
            "symbol": "btc_usd",
            "type": "sell"
        },
        {
            "amount": 0.2,
            "avg_price": 0,
            "create_date": 1417417957000,
            "deal_amount": 0,
            "order_id": 10000724,
            "orders_id": 10000724,
            "price": 0.1,
            "status": 0,
            "symbol": "btc_usd",
            "type": "buy"
        }
    ]
}
```

Return Values	

```
amount: order quantity
avg_price: average transaction price
create_date: order time
deal_amount: filled quantity
order_id: order ID
orders_id: order ID (Deprecated)
price: order price
status: -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 3 = cancel request in process
type: buy_market = market buy order, sell_market = market sell order
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|order_id|if order\_id is -1, then return all unfilled orders, otherwise return the order specified|
|sign|signature of request parameters|

6. POST /api/v1/orders_info    Get Order Information in Batch

URL `https://www.okex.com/api/v1/orders_info.do`   
	Request frequency 20 times/2s

Example	

```
# Request
POST https://www.okex.com/api/v1/orders_info.do
# Response
{
	"result":true,
	"orders":[
		{
			"order_id":15088,
			"status":0,
			"symbol":"btc_usd",
			"type":"sell",
			"price":811,
			"amount":1.39901357,
			"deal_amount":1,
			"avg_price":811
		} ,
		{
			"order_id":15088,
			"status":-1,
			"symbol":"btc_usd",
			"type":"sell",
			"price":811,
			"amount":1.39901357,
			"deal_amount":1,
			"avg_price":811
		}
	]
}
```

Return Values	

```
amount: for limit orders, returns the order quantity.  For market orders, returns the filled quantity
deal_amount: filled quantity
avg_price: average transaction price
create_date: order time
order_id: order ID
price: for limit orders, return the order price.  For market orders, returns the filled price
status: -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process
type: buy_market = market buy order, sell_market = market sell order
result: request result
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|type|query type: 0 for unfilled (open) orders, 1 for filled orders|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|order_id|order ID (multiple orders are separated by ',', 50 orders at most are allowed per request)|
|sign|signature of request parameters|

7. POST /api/v1/order_history    Only the most recent 2 days are returned

URL `https://www.okex.com/api/v1/order_history.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/order_history.do
# Response
{
	"current_page": 1,
	"orders": [
		{
			"amount": 0,
			"avg_price": 0,
			"create_date": 1405562100000,
			"deal_amount": 0,
			"order_id": 0,
			"price": 0,
			"status": 2,
			"symbol": "btc_usd",
			"type": "sell”
		}
	],
	"page_length": 1,
	"result": true,
	"total": 3
}
```

Return Values	

```
current_page: current page number
orders: detailed order information
amount: order quantity
avg_price: average transaction price
create_date: order time
deal_amount: filled quantity
order_id: order ID
price: order price
status: -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process
type: buy_market = market buy order, sell_market = market sell order
page_length: number of orders per page
result: true means request successfully handled
```

Request Parameters	

|Parameter|		Description|
| :-----       | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|status|query type: 0 for unfilled (open) orders, 1 for filled orders|
|current_page|current page number|
|page_length|number of orders returned per page, maximum 200|
|sign|signature of request parameters|

8. POST /api/v1/withdraw  	 BTC/LTC/ETH/ETC/BCH Withdraw

URL `https://www.okex.com/api/v1/withdraw.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/withdraw.do
# Response
{
    "withdraw_id":301,
    "result":true
}
```

Return Values	

```
withdraw_id: withdrawal request ID
result: true means request successful
```

Request Parameters	

|Parameter|		Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|chargefee| network transaction fee. By default, fee is between 0.002 - 0.005 for BTC, and 0.001 - 0.2 for LTC, and ETH is 0.01, and 0.0001 - 0.2 for ETC, and 0.0005 - 0.002 for BCH transaction gets confirmed faster with higher fees. For withdraws to another OKCoin address, chargefee can be 0 and the withdraw will be 0 confirmation as well.|
|trade_pwd|trade/admin password|
|withdraw_address|withdraw address|
|withdraw_amount|BTC>=0.01 LTC>=0.1 ETH>=0.01 ETC>=0.1 BCH>=0.01|
|target|withdraw address type. okcoin.cn:"okcn" okcoin.com:"okcom" okes.com："okex" outer address:"address"|
|sign|signature of request parameters|

9. POST /api/v1/cancel_withdraw   Withdrawal Cancellation Request

URL `https://www.okex.com/api/v1/cancel_withdraw.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/cancel_withdraw.do
# Response
{
    "result":true,
    "withdraw_id":301
}
```

Return Values	

```
result: true for success, false for error
withdraw_id: withdrawal request ID
```

Request Parameters	

|Parameter|	Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|withdraw_id|withdrawal request ID|
|sign|signature of request parameters|

10. POST /api/v1/withdraw_info    Get Withdrawal Information

URL `https://www.okex.com/api/v1/withdraw_info.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/withdraw_info.do
# Response
{
    "result": true,
    "withdraw": [
        {
            "address": "15KGp……",
            "amount": 2.5,
            "created_date": 1447312756190,
            "chargefee": 0.1,
            "status": 0,
            "withdraw_id": 45001
        }
    ]
}
```

Return Values	

```
result: true for success, false for error
address: withdrawal address
amount: withdrawal amount
created_date: withdrawal time
chargefee: network transaction fee
status: withdrawal status (-3:Revoked;-2:Cancelled;-1:Failure;0:Pending;1:Pending;2:Complete;3:Email Confirmation;4:Verifying5:Wait Confirmation)
withdraw_id: withdrawal request ID
```

Request Parameters	

|Parameter|		Description|
| :-----| :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|withdraw_id|withdrawal request ID|
|sign|signature of request parameters|

11. POST /api/v1/account_records    Get User Deposits or Withdraw Record

URL `https://www.okex.com/api/v1/account_records.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/account_records.do
# Response
{
    "records": [
        {
            "addr": "1CWKbfwxSkEWP8W45D76j3BX8vPieoSCyL",
            "account": "12312",
            "amount": 0,
            "bank": "HSBC Hongkong",
            "benificiary_addr": "350541545",
            "transaction_value": 111,
            "fee": 0,
            "date": 1418008467000,
			"status": 1
        }
    ],
    "symbol": "btc"
}
```

Return Values	

```
addr: address
account: account name
amount: amount
bank: bank
benificiary_addr: benificiary address
transaction_value: withdraw amount after fee deduction
fee: commission fee
date: date
symbol: btc, ltc, eth, etc, bch, usdt
status: recharge status (-1:Failure;0:Wait Confirmation;1:Complete), withdrawal status (-3:Revoked;-2:Cancelled;-1:Failure;0:Pending;1:Pending;2:Complete;3:Email Confirmation;4:Verifying5:Wait Confirmation)
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_usd  etc\_usd|
|type|0：deposits 1 ：withdraw|
|current_page|current page number|
|page_length|data entries number per page, maximum 50|
|sign|signature of request parameters|

12. POST /api/v1/funds_transfer    Funds transfer

URL `https://www.okex.com/api/v1/funds_transfer.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/funds_transfer.do
# Response
{
    "result":true
}
or
{
    "error_code":20029,
    "result":false
}
```

Return Values	

```
result:true for success, false for error
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|Pairs like : ltc\_btc  etc\_btc|
|amount|The amount to transfer|
|from|Remitting Account(1:spot 3:futures 6:my wallet)|
|to|Beneficiary Account(1:spot 3:futures 6:my wallet)|
|sign|signature of request parameters|

13. POST /api/v1/wallet_info    Get My Wallet Info

URL `https://www.okex.com/api/v1/wallet_info.do`  
	Request frequency 6 times/2s    

Example	

```
# Request
POST https://www.okex.com/api/v1/wallet_info.do
# Response
{
    "info": {
        "funds": {
            "free": {
                "btc": "0",
                "ltc": "0",
                "eth": "0"
            },
            "holds": {
                "btc": "0",
                "ltc": "0",
                "eth": "0"
            }
        }
    },
    "result": true
}
```

Return Values	

```
free: available fund
holds: frozen fund
```

Request Parameters	

|Parameter|Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
