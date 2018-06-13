# FUTURES REST API    

## Getting Started    

REST , a.k.a Respresntational State Transfer, is one of the most common web services protocol. REST emphasis on its readability, standardization, interoperabilty and scalability.Advantages: 
   
- Each URL respresented one web resources in RESTful architecture;
- Act as a representation of resources between client and server;
- Client-end is enabled to operate server-side resources with 4 HTTP requests representational state transfer.   

We recommend developers to use REST API to proceed Contracts trading and withdrawals.

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

#### Contract Price API 

Contract Price API Receive the latest OKEX contract data  

1. Get /api/v1/future_ticker    Get OKEX Contract Price

URL `https://www.okex.com/api/v1/future_ticker.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_ticker.do?symbol=btc_usd&contract_type=this_week
# Response
{
	"date":"1411627632",
	"ticker":{
		"last":409.2, 
		"buy" :408.23, 
		"sell":409.18, 
		"high":432.0, 
		"low":405.71, 
		"vol":55168.0, 
		"contract_id":20140926012, 
		"unit_amount":100.0
	}
}
```

Return Values	

```
buy: best bid
contract_id: contract ID
high: highest price
last: latest price
low: lowest price
sell: best ask
unit_amount: contract par value
vol: volume (in last 24 hours)
```

Request Parameters	

|Parameter|Description|
| :-----    |:-----   |
|symbol|btc\_usd   ltc\_usd      eth\_usd      etc\_usd     bch\_usd|
|contract\_type|this\_week   next\_week   quarter|

2. Get /api/v1/future_depth   Get OKEX Contract Market Depth

URL `https://www.okex.com/api/v1/future_depth.do`	

Example

```
# Request
GET https://www.okex.com/api/v1/future_depth.do?symbol=btc_usd&contract_type=this_week
# Response
{
	"asks":[
		[411.8,6],
		[411.75,11],
		[411.6,22],
		[411.5,9],
		[411.3,16]
	],
	"bids":[
		[410.65,12],
		[410.64,3],
		[410.19,15],
		[410.18,40],
		[410.09,10]
	]
}
```

Return Values	

```
asks :ask depth
bids :bids depth
```

Request Parameters	

|Parameter|Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type| this\_week   next\_week  quarter|
|size|value：5-200|
|merge|value：1(merge depth)|

3. Get /api/v1/future_trades   Get Contract Trade History

URL `https://www.okex.com/api/v1/future_trades.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_trades.do?symbol=btc_usd&contract_type=this_week
# Response
[
	{
		"amount":11,
		"date_ms":140807646000,
		"date":140807646,
		"price":7.076,
		"tid":37,
		"type":"buy"
	},
	{
		"amount":100,
		"date_ms":1408076464000,
		"date":1408076464,
		"price":7.076,
		"tid":39,
		"type":"sell"
	}
]
```

Return Values	

```
amount: quantity, in number of contracts
date_ms: transaction time(ms)
date: transaction time
price: transaction price
tid: transaction ID
type: buy/sell
```

Request Parameters	

|Parameter|	Description|
| :-----   | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type| this\_week   next\_week  quarter|



 4. Get /api/v1/future_index    Get OKEX Contract Index Price

URL `https://www.okex.com/api/v1/future_index.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_index.do?symbol=btc_usd
# Response
{"future_index":471.0817}
```

Return Values	

```
future_index :current futures index price
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|

5. Get /api/v1/exchange_rate   Get USD-CNY Exchange Rate

URL `https://www.okex.com/api/v1/exchange_rate.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/exchange_rate.do
# Response
{ "rate":6.1329 }
```

Return Values	

```
rate： USD-CNY exchange rate used by OKEX, updated weekly
```

Request Parameters	
```
none
```

6. Get /api/v1/future_estimated_price   Get Estimated Delivery Price

URL `https://www.okex.com/api/v1/future_estimated_price.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_estimated_price.do?symbol=btc_usd
# Response
{"forecast_price":5.4}
```

Return Values	

```
forecast_price:estimated delivery price  
Note: only available within 3 hrs before delivery or settlement
```

Request Parameters	

|Parameter|	Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|

7. Get /api/v1/future_kline   Get Contract Candlestick Data

URL `https://www.okex.com/api/v1/future_kline.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_kline.do
# Response
[
    [
        1440308700000,
        233.37,
        233.48,
        233.37,
        233.48,
        52,
        22.2810015
    ],
    [
        1440308760000,
        233.38,
        233.38,
        233.27,
        233.37,
        186,
        79.70234956
    ]
]
```

Return Values	

```
[
   1440308760000,	timestamp
    233.38,		open
    233.38,		high
    233.27,		low
    233.37,		close
    186,		volume
    79.70234956		BTC or LTC amount
]
```

Request Parameters	

|Parameter|		Description|
| :-----      | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|type|1min : 1 minute candlestick data  <br>3min : 3 minutes candlestick data  <br> 5min : 5 minutes candlestick data  <br> 15min : 15 minutes candlestick data  <br> 30min : 30 minutes candlestick data  <br> 1day : 1 day candlestick data <br> 3day : 3 days candlestick data <br> 1week : 1 week candlestick data  <br> 1hour : 1 hour candlestick data <br>  2hour : 2 hours candlestick data  <br> 4hour : 4 hours candlestick data <br> 6hour : 6 hours candlestick data  <br> 12hour : 12 hours candlestick data|
|contract\_type| contract type: this\_week/next\_week/quarter|
|size|specify data size to be acquired|
|since|timestamp(eg:1417536000000). data after the timestamp will be returned|

8. Get /api/v1/future_hold_amount   Get Total Number Of Current Holding (cont)

URL `https://www.okex.com/api/v1/future_hold_amount.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_hold_amount.do?symbol=btc_usd&contract_type=this_week
# Response
[
    {
        "amount": 106856,
        "contract_name": "BTC0213"
    }
]
```

Return Values	

```
amount: total number (cont)
contract_name: contract name
```

Request Parameters	

|Parameter|	Description|
| :-----     | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|contract type: this\_week/next\_week/quarter|

9. Get /api/v1/future_price_limit   Get upper and lower price limit

URL `https://www.okex.com/api/v1/future_price_limit.do`	

Example	

```
# Request
GET https://www.okex.com/api/v1/future_price_limit.do?symbol=btc_usd&contract_type=this_week
# Response
{"high":443.07,"low":417.09}
```

Return Values	

```
high: limit high price
low: limit low price
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|contract type: this\_week/next\_week/quarter (required)|

#### Contract Trade API 

Contract Trading API for placing contract orders on OKEX

1. POST /api/v1/future_userinfo   Get OKEX Contract Account Info（Cross-Margin Mode）

URL `https://www.okex.com/api/v1/future_userinfo.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_userinfo.do
# Response
{
    "info": {
        "btc": {
            "account_rights": 1,
            "keep_deposit": 0,
            "profit_real": 3.33,
            "profit_unreal": 0,
            "risk_rate": 10000
        },
        "ltc": {
            "account_rights": 2,
            "keep_deposit": 2.22,
            "profit_real": 3.33,
            "profit_unreal": 2,
            "risk_rate": 10000
        }
    },
    "result": true
}
```

Return Values	

```
account_rights: account equity
keep_deposit: margin
profit_real: realized
profit_unreal: unrealized
risk_rate: current margin ratio

```

Request Parameters	

|Parameter|		Description|
| :-----     | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|

2. POST /api/v1/future_position   Get User Contract Positions （Cross-Margin Mode）

URL `https://www.okex.com/api/v1/future_position.do`	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_position.do
# Response
{
	"force_liqu_price": "0.07",
    "holding": [
        {
            "buy_amount": 1,
            "buy_available": 0,
            "buy_price_avg": 422.78,
            "buy_price_cost": 422.78,
            "buy_profit_real": -0.00007096,
            "contract_id": 20141219012,
            "contract_type": "this_week",
            "create_date": 1418113356000,
            "lever_rate": 10,
            "sell_amount": 0,
            "sell_available": 0,
            "sell_price_avg": 0,
            "sell_price_cost": 0,
            "sell_profit_real": 0,
            "symbol": "btc_usd"
        }
    ],
    "result": true
}
```

Return Values	

```
buy_amount(double): long position amount(usd)
buy_available: available long postions that can be closed
buy_price_avg(double): average open price
buy_price_cost(double): cost price
buy_profit_real(double): long position realized gains/losses
contract_id(long): contract ID
create_date(long): open date
lever_rate: lever rate
sell_amount(double): short position amount(usd)
sell_available: available short postions that can be closed
sell_price_avg(double): average open price
sell_price_cost(double): cost price
sell_profit_real(double): short position realized gains/losses
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
contract_type: contract type
force_liqu_price: estimated margin call price
```

Request Parameters	

|Parameter|	Description|
| :-----     | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter|
|api_key|apiKey of the user|
|sign|signature of request parameters|

3. POST /api/v1/future_trade   Place Orders

URL `https://www.okex.com/api/v1/future_trade.do`  
Request frequency 5 times/s(each coin has 5 times)	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_trade.do
# Response
{
	"order_id":986,
   "result":true
}
```

Return Values	

```
order_id: order ID
result: request result, true means successful
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter|
|api_key|apiKey of the user|
|sign|signature of request parameters|
|price|order price|
|amount|order quantity|
|type| 1: open long position    2: open short position    3:liquidate long position    4: liquidate short position|
|match_price|match best counter party price (BBO)? 0: No    1: Yes   If yes, the 'price' field is ignored|
|lever_rate|Leverage settings have to be adjusted on the trading page before creating an order. If you currently have 10x open order(s) or holding position(s), you cannot create a new 20x order.|

4. POST /api/v1/future_trades_history    Get OKEX Contract Trade History (Not for Personal)

URL `https://www.okex.com/api/v1/future_trades_history` 

Example	

```
# Request
POST https://www.okex.com/api/v1/future_trades_history.do
# Response
[
    {
        "amount": 11,
        "date": 140807646000,
        "price": 7.076,
        "tid": 37,
        "type": "buy"
    },
    {
        "amount": 100,
        "date": 1408076464000,
        "price": 7.076,
        "tid": 39,
        "type": "sell"
    }
]
```

Return Values	

```
amount： quantity, in number of contracts
date：transaction time(ms)
price：transaction price
tid：transaction ID
type：buy/sell
```

Request Parameters	

|Parameter|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|date|contract delivery date, format :yyyy-MM-dd|
|since|the start of transaction id|

5. POST /api/v1/future\_batch_trade   Batch Trade

URL `https://www.okex.com/api/v1/future_batch_trade.do`  
Request frequency 3 times/s  (each coin has 3 times).you can place 5 orders at monst once	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_batch_trade.do
# Response
{
	"order_info":[
        {"error_code":20013,"order_id":-1},
        {"error_code":20013,"order_id":-1},
        {"order_id":161256},
        {"order_id":161257}
	],
	"result":true
}
```

Return Values	

```
result: order successfully placed or not
order_id: order ID
Note:
     return true if any one order is placed successfully
     returned orders info and 'orders_data' are in same sequence
     if fail to place a specific order, the order_id is -1
```

Request Parameters	

|Parameter|	Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter|
|orders\_data|JSON string example: [{price:5,amount:2,type:1,match\_price:1},{price:2,amount:3,type:1,match\_price:1}]. Max 5 orders per request. For 'price', 'amount', 'type' and 'match\_price' parameters, refer to future\_trade/API|
|sign|signature of request parameters|
|lever_rate|Leverage settings have to be adjusted on the trading page before creating an order. If you currently have 10x open order(s) or holding position(s), you cannot create a new 20x order.|

6. POST /api/v1/future_cancel   Cancel Orders

URL `https://www.okex.com/api/v1/future_cancel.do`  
Request frequency 2 times/s  (each coin has 2 times).you can cancel 5 orders at monst once	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_cancel.do
# Response
{
    "order_id":986,
    "result":true
}
```

Return Values	

```
order_id: order ID
result: request result, true is successful
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|api_key|apiKey of the user|
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|order_id|order ID (multiple orders are separated by a comma ',', Max of 5 orders are allowed per request)|
|sign|signature of request parameters|
|contract\_type|this\_week   next\_week   quarter|

7. POST /api/v1/future\_order\_info   Get User Contract Order

URL `https://www.okex.com/api/v1/future_order_info.do`  	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_order_info.do
# Response
{
  "orders":
     [
        {
            "amount":111,
            "contract_name":"LTC0815",
            "create_date":1408076414000,
            "deal_amount":1,
            "fee":0,
            "order_id":106837,
            "price":1111,
            "price_avg":0,
            "status":"0",
            "symbol":"ltc_usd",
            "type":"1",
            "unit_amount":100,
            "lever_rate":10
        }
     ],
   "result":true
}
```

Return Values	

```
amount: order quantity
contract_name: contract name
create_date: order time
deal_amount: filled quantity
fee: transaction fee
order_id: order ID
price: order price
price_avg: average price
status: -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process, 5 = cancelled
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
type: order type 1: open long, 2: open short, 3: close long, 4: close short
unit_amount: contract par value
lever_rate: leverage rate value, 10 or 20
```

Request Parameters	

|Parameter|		Description|
| :-----    | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter|
|api_key|apiKey of the user|
|sign|signature of request parameters|
|status|query by order status 1: unfilled  2: filled|
|order_id|order ID -1: return the orders of the specified status, otherwise return the order that the ID specified|
|current\_page|current page number|
|page_length|number of orders per page, maximum 50|

8. POST /api/v1/future\_orders\_info   Get User Contract Order in 



URL `https://www.okex.com/api/v1/future_orders_info.do`  


Example	

```
# Request
POST https://www.okex.com/api/v1/future_orders_info.do
# Response
{
    "orders": [
        {
            "amount": 1,
            "contract_name": "BTC0213",
            "create_date": 1424932853000,
            "deal_amount": 0,
            "fee": 0,
            "lever_rate": 20,
            "order_id": 200144,
            "price": 1,
            "price_avg": 0,
            "status": 0,
            "symbol": "btc_usd",
            "type": 1,
            "unit_amount": 100
        },
        {
            "amount": 1,
            "contract_name": "BTC0213",
            "create_date": 1424932903000,
            "deal_amount": 0,
            "fee": 0,
            "lever_rate": 20,
            "order_id": 200146,
            "price": 1,
            "price_avg": 0,
            "status": 0,
            "symbol": "btc_usd",
            "type": 1,
            "unit_amount": 100
        },
        {
            "amount": 1,
            "contract_name": "BTC0213",
            "create_date": 1424932917000,
            "deal_amount": 0,
            "fee": 0,
            "lever_rate": 20,
            "order_id": 200147,
            "price": 1,
            "price_avg": 0,
            "status": 0,
            "symbol": "btc_usd",
            "type": 1,
            "unit_amount": 100
        }
    ],
    "result": true
}
```

Return Values	

```
amount: order quantity
contract_name: contract name
created_date: order time
deal_amount: filled quantity
fee: transaction fee
order_id: order ID
price: order price
price_avg: average price
status: -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
type: order type 1: open long, 2: open short, 3: close long, 4: close short
unit_amount: contract par value
lever_rate: leverage rate value, 10 or 20
```

Request Parameters	

|Parameter|Description|
| :-----     | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter|
|api_key|apiKey of the user|
|sign|signature of request parameters|
|order_id|order ID (multiple orders are separated by a comma ',', Max of 50 orders are allowed per request)|

9. POST /api/v1/future\_userinfo\_4fix   Get User Account Info (Fixed-Margin Mode)

URL `https://www.okex.com/api/v1/future_userinfo_4fix.do`  


Example	

```
# Request
POST https://www.okex.com/api/v1/future_userinfo_4fix.do
# Response
{
	"info": {
        "btc": {
            "balance": 99.95468925,
            "contracts": [{
                "available": 99.95468925,
                "balance": 0.03779061,
                "bond": 0,
                "contract_id": 20140815012,
                "contratct_type": "this_week",
                "freeze": 0,
                "profit": -0.03779061,
                "unprofit": 0
            }],
            "rights": 99.95468925
        },
        "ltc": {
            "balance": 77,
            "contracts": [{
                "available": 99.95468925,
                "balance": 0.03779061,
                "bond": 0,
                "contract_id": 20140815012,
                "contract_type": "this_week",
                "freeze": 0,
                "profit": -0.03779061,
                "unprofit": 0
            }],
            "rights": 100
        }
    },
    "result": true
}
```

Return Values	

```
balance: account balance
available: available fund
balance: account (contract) balance
bond: fixed margin
contract_id: contract ID
contract_type: contract Type
freeze: frozen
profit: realized
unprofit: unrealized
rights: account equity
```

Request Parameters	

|Parameter|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|

10. POST /api/v1/future\_position\_4fix   Get User Positions (Fixed-Margin Mode)

URL `https://www.okex.com/api/v1/future_position_4fix.do`  
Example	

```
# Request
POST https://www.okex.com/api/v1/future_position_4fix.do
# Response
{
	"holding": [{
        "buy_amount": 10,
        "buy_available": 2,
        "buy_bond": 1.27832803,
        "buy_flatprice": "338.97",
        "buy_price_avg": 555.67966869,
        "buy_price_cost": 555.67966869,
        "buy_profit_lossratio": "13.52",
        "buy_profit_real": 0,
        "contract_id": 20140815012,
        "contract_type": "this_week",
        "create_date": 1408594176000,
        "sell_amount": 8,
        "sell_available": 2,
        "sell_bond": 0.24315591,
        "sell_flatprice": "671.15",
        "sell_price_avg": 567.04644056,
        "sell_price_cost": 567.04644056,
        "sell_profit_lossratio": "-45.04",
        "sell_profit_real": 0,
        "symbol": "btc_usd",
        "lever_rate": 10
    }],
    "result": true
}
```

Return Values	

```
buy_amount: long position quantity
buy_available: available long postions that can be closed
buy_bond: long position margin
buy_flatprice: long position force liquidation price
buy_profit_lossratio: long position profit ratio
buy_price_avg: average open price
buy_price_cost: cost price
buy_profit_real: long position realized gain/loss
contract_id: contract ID
contract_type: contract Type
create_date: position opened time
sell_amount: short position quantity
sell_available: available short postions that can be closed
sell_bond: short position margin
sell_flatprice: short position force liquidation price
sell_profit_lossratio: short position profit ratio
sell_price_avg: average open price
sell_price_cost: cost price
sell_profit_real: short position realized gain/loss
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
lever_rate: leverage rate 
```

Request Parameters	

|Parameter|		Description|
| :-----   | :-----   |
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|this\_week   next\_week   quarter   (if left null, all contracts are returned)|
|api_key|apiKey of the user|
|sign|signature of request parameters|
|type|by default, futures positions with leverage rate 10 are returned. If type = 1, all futures positions are returned|

11. POST /api/v1/future_explosive   Get Forced Liquidation Orders

URL `https://www.okex.com/api/v1/future_explosive.do` 

Example	

```
# Request
POST https://www.okex.com/api/v1/future_explosive.do
# Response
[
    {
        "data": [
            {
                "amount": "42",
                "create_date": "2015-02-27 11:48:07",
                "loss": "-0.54275722",
                "price": "249.02",
                "type": 4
            },
            {
                "amount": "12",
                "create_date": "2015-02-27 11:48:05",
                "loss": "-0.15507349",
                "price": "249.02",
                "type": 4
            },
            {
                "amount": "3",
                "create_date": "2015-02-27 11:48:01",
                "loss": "-0.03896192",
                "price": "248.98",
                "type": 4
            },
            {
                "amount": "1811",
                "create_date": "2015-02-27 11:48:01",
                "loss": "-23.40317457",
                "price": "249.02",
                "type": 4
            },
            {
                "amount": "2",
                "create_date": "2015-02-27 11:48:00",
                "loss": "-0.02655576",
                "price": "248.80",
                "type": 4
            }
        ]
    }
]
```

Return Values	

```
amount: order quantity(unit: contract)
create_date: create date
loss: user loss due to forced liquidation
price: order price
type：1:open long position; 2:open short position; 3:liquidate long position; 4:liquidate short position

```

Request Parameters	

|Parameter|	Description|
| :-----     | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|contract type: this\_week/next\_week/quarter|
|status|0: open liquidation orders of last 7 days; 1: filled liquidation orders of last 7 days|
|current\_page|current page index|
|page_number|current page number(current\_page is invalid when using page\_number，don't send current\_page )|
|page_length|number of orders per page, maximum 50|

12. POST /api/v1/future_devolve   Account Fund Transfer

URL `https://www.okex.com/api/v1/future_devolve.do`  	

Example	

```
# Request
POST https://www.okex.com/api/v1/future_devolve.do
# Response
{
    "result":true
}
或
{
    "error_code":20029,
    "result":false
}
```

Return Values	

```
result: Transfer result. In case of error, returns the error code.
```

Request Parameters	

|Parameter|		Description|
| :-----     | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|
|symbol|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|type|1: Spot to futures, 2: Futures to spot|
|amount| Amount in coins|













