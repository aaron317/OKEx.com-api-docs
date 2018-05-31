# WebSocket API for FUTURES    

## Getting Started    

WebSocket protocol is a new HTML5 protocol, which provides full-duplex communication between web browsers and web servers. Connection can be established after one handshake. Web server can then push business logic data to web browsers.Advantages:

- Request header is small in size (around 2 bytes) during communication
- Web servers and clients can send data bi-directionaly
- Since there is no need to create and delete TCP connection repeatedly, it saves resources
We strongly recommend developers to use Websocket API to access market related information and trading depth.

Should you have any questions, feel free to contact our support team.    
    
## Request Process    

OKEx Contract WebSocket URL：`wss://real.okex.com:10440/websocket/okexapi`         
	
#### Send Request    
Request Data Format: 

```
{'event':'addChannel','channel':'channelValue','parameters':{'api\_key':'value1','sign':'value2'}}.   
```

Note: 		

```
'event':'addChannel'(register request)/'removeChannel'(unregister request)

'channel': OKEX provided data type  	  

'parameters': optional field 	

'api\_key' and 'secret\_key' is apply 'apiKey' and 'secretKey' for users    	
```

Example:

```
websocket.send("{'event':'addChannel','channel':'ok\_btcusd\_ticker' }")	

websocket.send("[{'event':'addChannel','channel':'ok\_btcusd\_ticker'},{'event':'addChannel','channel':'ok\_btcusd\_depth'},{'event':'addChannel','channel':'ok\_btcusd\_trades'}]"); Support batch register 
```
   
#### Server Response
Return Data Format:
```
[{"channel":"channel","success":"","errorcode":"","data":{}},{"channel":"channel","success":"","errorcode":1,"data":{}}] 
```
channel: requested data type
result: true,false(applicable to WebSocket Trade API)
data: returned data
errorcode: error code(applicable to WebSocket Trade API)   

#### Data Push Process   
To reduce data size and ensure real-time data push, 'ticker' and 'depth' will only be pushed only when new data is available. 'trades' is newly completed transactions from previous data push to this data push.   

#### How to know whether connection is lost?
OKEX provides heartbeat verification process. Clients send: {'event':'ping'} per 30 seconds, the server returns heartbeats {"event":"pong"} to indicate connection between the clients and the server. If the clients stop receiving the heartbeats, then they will need to reconnect with the server.    

## API Reference  

### Contract Price API 

Receive the latest OKEX contract data
  
1. ok\_sub\_futureusd\_X\_ticker\_Y   Subscribe Contract Market Price

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_ticker_Y'}");`	
① value of X is: btc, ltc, eth, etc, bch  
② value of Y is: this_week, next_week, quarter
Example	

```
# Request Example
{'event':'addChannel','channel':'ok_sub_futureusd_btc_ticker_this_week'}
# Response
[
    {
        "data": {
            "limitHigh": "1030.3",
            "vol": 276406,
            "last": 998.05,
            "sell": 998.05,
            "buy": 997.61,
            "unitAmount": 100,
            "hold_amount": 180178,
            "contractId": 20170324034,
            "high": 1049.18,
            "low": 973.15,
            "limitLow": "968.1"
        },
        "channel": "ok_sub_futureusd_btc_ticker_this_week"
    }
]
```

Returned Value Description	

```
limitHigh(string):maximum buy price
limitLow(string):minimum sell price
vol(double):trade volumes within 24 hours
sell(double):sell one price
unitAmount(double):union amount
hold_amount(double):position
contractId(long):contract id
high(double):the highest price within 24 hours
low(double):the lowest price within 24 hours
```


2. ok\_sub\_futureusd\_X\_kline\_Y\_Z   Subscribe Contract Candlestick Chart Data

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_kline_Y_Z'}");`	
① value of X is:btc, ltc, eth, etc, bch  
② value of Y is:this\_week, next\_week, quarter  
③ value of Z is:1min, 3min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, day, 3day, week

Example	

```
# Request 
{'event':'addChannel','channel':'ok_sub_futureusd_btc_kline_this_week_1min'}
# Response
[
    {
        "data":[
            [
                "1490340480000",
                "998.96",
                "998.96",
                "998.96",
                "998.96",
                "12.0",
                "1.201249299271"
            ]
        ],
        "channel":"ok_sub_futureusd_btc_kline_this_week_1min"
    }
]
```

Returned Value Description	

```
[time, open price, highest price, lowest price, close price, volume]
[string, string, string, string, string, string]
```

3. ok\_sub\_futureusd\_X\_depth_Y   Subscribe Contract Market Depth(Incremental)

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_depth_Y'}");`	
① the value of X is: btc, ltc, eth, etc, bch  
② the value of Y is: this\_week, next\_week, quarter

Example	

```
# Request 
{'event':'addChannel','channel':'ok_sub_futureusd_btc_depth_this_week'}
# Response
[
    {
        "data": {
            "timestamp": 1490337551299,
            "asks": [
                [
                    "996.72",
                    "20.0",
                    "2.0065",
                    "85.654",
                    "852.0"
                ]
            ],
            "bids": [
                [
                    "991.67",
                    "6.0",
                    "0.605",
                    "0.605",
                    "6.0"
                ]
        },
        "channel": "ok_sub_futureusd_btc_depth_this_week"
    }
]
```

Returned Value Description	

```
timestamp(long): Server timestamp
asks(array): Ask depth Array index(string) [Price, Amount(Contract), Amount(Coin),Cumulant(Coin),Cumulant(Contract)]
bids(array): Bid depth Array index(string) [Price, Amount(Contract), Amount(Coin),Cumulant(Coin),Cumulant(Contract)]

Incremental data return (Return full data for the first query)
Delete (BTC/LTC amount is 0)
Edit (Same price, different amount)
Add (Price inexist)
bids([double, double]): Bid depth
asks([double, double]): Ask depth
timestamp: Server timestamp
```


4. ok\_sub\_futureusd\_X\_depth\_Y\_Z   Subscribe Contract Market Depth(Full)

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_depth_Y_Z'}");`	
① value of X is:btc, ltc, eth, etc, bch  
② Yvalue of Y is:this\_week, next\_week, quarter  
③ value of Z is:5, 10, 20(Amount) 

Example	

```
# Request
{'event':'addChannel','channel':'ok_sub_futureusd_btc_depth_this_week_20'}
# Response
[
    {
        "data": {
            "timestamp": 1490337551299,
            "asks": [
                [
                    "996.72",
                    "20.0",
                    "2.0065",
                    "85.654",
                    "852.0"
                ]
            ],
            "bids": [
                [
                    "991.67",
                    "6.0",
                    "0.605",
                    "0.605",
                    "6.0"
                ]
        },
        "channel": "ok_sub_futureusd_btc_depth_this_week_20"
    }
]
```

Returned Value Description	

```
timestamp(long): Server timestamp
asks(array): Ask depth Array index(string) [Price, Amount(Contract), Amount(Coin),Cumulant(Coin),Cumulant(Contract)]
bids(array): Bid depth Array index(string) [Price, Amount(Contract), Amount(Coin),Cumulant(Coin),Cumulant(Contract)]
```


5. ok\_sub\_futureusd\_X\_trade\_Y   Subscribe Contract Trade Record

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_trade_Y'}");`	
① X值为：btc, ltc, eth, etc, bch  
② Y值为：this\_week, next\_week, quarter   

Example	

```
# Request 
{'event':'addChannel','channel':'ok_sub_futureusd_btc_trade_this_week'}
# Response
[
    {
        "data": [
            [
                "732916869",
                "999.49",
                "12.0",
                "15:25:03",
                "ask",
                "1.2006"
            ],
            [
                "732916871",
                "999.49",
                "2.0",
                "15:25:03",
                "ask",
                "0.2001"
            ],
            [
                "732916899",
                "999.49",
                "2.0",
                "15:25:04",
                "ask",
                "0.2001"
            ]
        "channel": "ok_sub_futureusd_btc_trade_this_week"
    }
]
```

Returned Value Description	

```
[tid, price, amount, time, type,amountbtc]
[string, string, string, string, string, string,string]
```

6. ok\_sub\_futureusd\_X\_index   Subscribe Contract Index Price

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_index'}");`	
① value of X is:btc, ltc, eth, etc, bch  
  

Example	

```
# Request
{'event':'addChannel','channel':'ok_sub_futureusd_btc_index'}
# Response
[
    {
        "data": {
            "timestamp": "1490341322021",
            "futureIndex": "998.0"
        },
        "channel": "ok_sub_futureusd_btc_index"
    }
]
```

Returned Value Description	

```
futureIndex: current future index price
timestamp: server timestamp
```

7. X\_forecast\_price   Estimated futures delivery price

	
① value of X is：btc, ltc, eth, etc, bch  
  

Example	

```
# Response
[{
    "channel": "btc_forecast_price",
    "timestamp":"1490341322021",
    "data": "998.8"
}]
```

Returned Value Description	

```
Estimated futures delivery price
data(string): Estimated delivery price
timestamp(string): timestamp
Hint: This does not need to be subscribed and will be automatically returned 1 hour prior to delivery.

```

### Contract Trade API 

Easily place contract orders on OKEX

1. login   Login(Personal Information)  

Example	

```
# Request
{"event":"login","parameters":{"api_key":"xxx","sign":"xxx"}}
# Response
[
    {
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
The data will return when it changed. These types need not be subscribed.
eg:ok_sub_futureusd_trades,ok_sub_futureusd_userinfo,ok_sub_futureusd_positions
```

2. ok\_sub\_futureusd\_trades   Subscribe Contract Trade Records  

Example	

```
# Response
[{
        "data":{
            amount:1
            contract_id:20170331115,
            contract_name:"LTC0331",
            contract_type:"this_week",
            create_date:1490583736324,
            create_date_str:"2017-03-27 11:02:16",
            deal_amount:0,
            fee:0,
            lever_rate:20,
            orderid:5058491146,
            price:0.145,
            price_avg:0,
            status:0,
            system_type:0,
            type:1,
            unit_amount:10,
            user_id:101
        },
        "channel":"ok_sub_futureusd_trades"
    }
]
```


Returned Value Description	

```
amount (double): order quantity
contract_id (long): contract ID
create_date_str(string): create date
create_date(long):	create date
contract_name (string): contract name
contract_type(string): contract type
deal_amount(string) : filled quantity
fee (string): transaction fee
orderid (long) : order ID
price(double) : order price
price_avg (double) : average price
status (int):  -1 = cancelled, 0 = unfilled, 1 = partially filled, 2 = fully filled, 4 = cancel request in process
type (int): order type 1: open long, 2: open short, 3: close long, 4: close short
unit_amount(double) : contract par value
lever_rate (double): leverage rate
```

Request Parameters	

|Parameter Name|	Description|
| :-----   | :-----   |
|api_key|apiKey of the user|
|sign|signature of request parameters|


3. ok\_sub\_futureusd\_userinfo   Contract User Info
  

Example	

```
# Response
Fix Margin Model Account Info
 [{
        "data":{
            "balance":10.16491751,
            "symbol":"ltc_usd",
            "contracts":[
                {
                    "bond":0.50922987,
                    "balance":0.50922987,
                    "profit":0,
                    "freeze":1.72413792,
                    "contract_id":20170331115,
                    "available":6.51526374
                },
                {
                    "bond":0,
                    "balance":0,
                    "profit":0,
                    "freeze":1.64942529,
                    "contract_id":20170407135,
                    "available":6.51526374
                },
                {
                    "bond":0,
                    "balance":0,
                    "profit":0,
                    "freeze":0.27609056,
                    "contract_id":20170630116,
                    "available":6.51526374
                }
            ]
        },
        "channel":"ok_sub_futureusd_userinfo"
    }]
The Cross Margin Account Info
[{
	"data":{
		"balance":0.2567337,
		"symbol":"btc_usd",
		"keep_deposit":0.0025,
		"profit_real":-0.00139596,
		"unit_amount":100
	},
	"channel":"ok_sub_futureusd_userinfo"
}]
```


Returned Value Description	

```
The Cross Margin Account Info

balance(double): Balance
symbol(double)：Currency
keep_deposit(double)：Margin
profit_real(double)：RPL
unit_amount(double)：Par value

Fix Margin Model Account Info

balance: account balance
available(double):available fund
balance(double): contract balance
bond(double):fixed margin
contract_id(long): contract id
contract_type(string): contract type
freeze(double):frozen
profit(double):realized
unprofit(double):Un Realized
```

Description	

```
There is no need to subscribe to the login, compatible with the old subscription, the old subscription is equivalent to the login operation.
Response:
[{"data":{"result":"true"},"channel":"login"}]
```


4. ok\_sub\_futureusd\_positions   Contract User Position Info
  

Example	

```
# Response
The Cross Margin Account Info
[{
        "data":{
            "positions":[
                {
                    "position":"1",
                    "contract_name":"BTC0630",
                    "costprice":"994.89453079",
                    "bondfreez":"0.0025",
                    "avgprice":"994.89453079",
                    "contract_id":20170630013,
                    "position_id":27782857,
                    "eveningup":"0",
                    "hold_amount":"0",
                    "margin":0,
                    "realized":0
                },
                {
                    "position":"2",
                    "contract_name":"BTC0630",
                    "costprice":"1073.56",
                    "bondfreez":"0.0025",
                    "avgprice":"1073.56",
                    "contract_id":20170630013,
                    "position_id":27782857,
                    "eveningup":"0",
                    "hold_amount":"0",
                    "margin":0,
                    "realized":0
                }
            ],
            "symbol":"btc_usd",
            "user_id":101
        },
        "channel":"ok_sub_futureusd_positions"
}]
Fix Margin Model Account Info
[{
	"data":{
		"positions":[
			{
				"position":"1",
				"profitreal":"0.0",
				"contract_name":"LTC0407",
				"costprice":"0.0",
				"bondfreez":"1.64942529",
				"forcedprice":"0.0",
				"avgprice":"0.0",
				"lever_rate":10,
				"fixmargin":0,
				"contract_id":20170407135,
				"balance":"0.0",
				"position_id":27864057,
				"eveningup":"0.0",
				"hold_amount":"0.0"
			},
			{
				"position":"2",
				"profitreal":"0.0",
				"contract_name":"LTC0407",
				"costprice":"0.0",
				"bondfreez":"1.64942529",
				"forcedprice":"0.0",
				"avgprice":"0.0",
				"lever_rate":10,
				"fixmargin":0,
				"contract_id":20170407135,
				"balance":"0.0",
				"position_id":27864057,
				"eveningup":"0.0",
				"hold_amount":"0.0"
			}
		"symbol":"ltc_usd",
		"user_id":101
	}]
```


Returned Value Description	

```
The Cross Margin Account Info

position(string): position 1 long 2 short
contract_name(string): contract name
costprice(string): cost price
bondfreez(string): freezed bond
avgprice(string): average open price
contract_id(long): contract id
position_id(long): position id
hold_amount(string): hold amount
eveningup(string): evening up
margin(double): margin
realized(double):real profit

Fix Margin Model Account Info

contract_id(long): contract id
contract_name(string): contract name
avgprice(string): average open price
balance(string): account (contract) balance
bondfreez(string): freezed bond
costprice(string): cost price
eveningup(string): evening up
forcedprice(string):  forced price
position(string): position 1 long 2 short
profitreal(string):  real profit
fixmargin(double): fixed margin
hold_amount(string): hold amount
lever_rate(double): lever rate
position_id(long): position id
symbol(string): btc_usd   ltc_usd   eth_usd   etc_usd   bch_usd
user_id(long):user_id
```

Description	

```
There is no need to subscribe to the login, compatible with the old subscription, the old subscription is equivalent to the login operation.
Response:
[{"data":{"result":"true"},"channel":"login"}]
```
