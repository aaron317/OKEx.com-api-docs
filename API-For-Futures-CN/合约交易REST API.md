# REST API for FUTURES    

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
- 在RESTful架构中，每一个URL代表一种资源；    
- 客户端和服务器之间，传递这种资源的某种表现层；    
- 客户端通过四个HTTP指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用REST API进行币币交易或者资产提现等操作。    
    
## 请求交互    

REST访问的根URL：`https://www.okex.com/api/v1` 

访问时需要科学上网

所有请求基于Https协议，请求头信息中contentType需要统一设置为：`application/x-www-form-urlencoded`   
	
请求交互说明    
1. 请求参数：根据接口请求参数规定进行参数封装。    
2. 提交请求参数：将封装好的请求参数通过POST 或GET 方式提交至服务器。    
3. 服务器响应：服务器首先对用户请求数据进行参数安全校验，通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。    
4. 数据处理：对服务器响应数据进行处理。 

## API参考  

### 合约行情 API 

获取OKEx合约行情数据  

1. Get /api/v1/future_ticker    获取OKEx合约行情

URL `https://www.okex.com/api/v1/future_ticker.do`	

示例	

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

返回值说明	

```
buy:买一价
contract_id:合约ID
high:最高价
last:最新成交价
low:最低价
sell:卖一价
unit_amount:合约面值
vol:成交量(最近的24小时)
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|

2. Get /api/v1/future_depth   获取OKEx合约深度信息

URL `https://www.okex.com/api/v1/future_depth.do`	

示例	

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

返回值说明	

```
asks :卖方深度
bids :买方深度
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|size|Integer|是|value：1-200|
|merge|Integer|否(默认0)|value：1(合并深度)|

3. Get /api/v1/future_trades   获取OKEx合约交易记录信息

URL `https://www.okex.com/api/v1/future_trades.do`	

示例	

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

返回值说明	

```
amount：交易数量
date_ms：交易时间(毫秒)
date：交易时间
price：交易价格
tid：交易ID
type：交易类型
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|



 4. Get /api/v1/future_index   获取OKEx合约指数信息

URL `https://www.okex.com/api/v1/future_index.do`	

示例	

```
# Request
GET https://www.okex.com/api/v1/future_index.do?symbol=btc_usd
# Response
{"future_index":471.0817}
```

返回值说明	

```
future_index :指数
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|

5. Get /api/v1/exchange_rate   获取美元人民币汇率

URL `https://www.okex.com/api/v1/exchange_rate.do`	

示例	

```
# Request
GET https://www.okex.com/api/v1/exchange_rate.do
# Response
{ "rate":6.1329 }
```

返回值说明	

```
rate：美元-人民币汇率
```

请求参数	
```
无
```

6. Get /api/v1/future_estimated_price   获取交割预估价

URL `https://www.okex.com/api/v1/future_estimated_price.do`	

示例	

```
# Request
GET https://www.okex.com/api/v1/future_estimated_price.do?symbol=btc_usd
# Response
{"forecast_price":5.4}
```

返回值说明	

```
forecast_price:交割预估价  注意：交割预估价只有交割前三小时返回
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|

7. Get /api/v1/future_kline   获取OKEx合约K线信息

URL `https://www.okex.com/api/v1/future_kline.do`	

示例	

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

返回值说明	

```
[
    1440308760000,	时间戳
    233.38,		开
    233.38,		高
    233.27,		低
    233.37,		收
    186,		交易量
    79.70234956		交易量转化BTC或LTC数量
]
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|type|String|是|1min/3min/5min/15min/30min/1day/3day/1week/1hour/2hour/4hour/6hour/12hour|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|size|Integer|否(默认0)|指定获取数据的条数|
|since|Long|否(默认0)|时间戳（eg：1417536000000）。 返回该时间戳以后的数据|

8. Get /api/v1/future_hold_amount   获取当前可用合约总持仓量

URL `https://www.okex.com/api/v1/future_hold_amount.do`	

示例	

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

返回值说明	

```
amount:总持仓量（张）
contract_name:合约名
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|

9. Get /api/v1/future_price_limit   获取合约最高限价和最低限价

URL `https://www.okex.com/api/v1/future_price_limit.do`	

示例	

```
# Request
GET https://www.okex.com/api/v1/future_price_limit.do?symbol=btc_usd&contract_type=this_week
# Response
{"high":443.07,"low":417.09}
```

返回值说明	

```
high :最高买价
low :最低卖价
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|

### 合约交易 API 

用于OKEX快速进行合约交易

1. POST /api/v1/future_userinfo   获取OKEx合约账户信息(全仓)

URL `https://www.okex.com/api/v1/future_userinfo.do`  访问频率 10次/2秒  

示例	

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

返回值说明	

```
account_rights:账户权益
keep_deposit：保证金
profit_real：已实现盈亏
profit_unreal：未实现盈亏
risk_rate：保证金率

```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|

2. POST /api/v1/future_position   获取用户持仓获取OKEX合约账户信息 （全仓）

URL `https://www.okex.com/api/v1/future_position.do`  访问频率 10次/2秒 

示例	

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

返回值说明	

```
buy_amount(double):多仓数量
buy_available:多仓可平仓数量
buy_price_avg(double):开仓平均价
buy_price_cost(double):结算基准价
buy_profit_real(double):多仓已实现盈余
contract_id(long):合约id
create_date(long):创建日期
lever_rate:杠杆倍数
sell_amount(double):空仓数量
sell_available:空仓可平仓数量
sell_price_avg(double):开仓平均价
sell_price_cost(double):结算基准价
sell_profit_real(double):空仓已实现盈余
symbol:btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
contract_type:合约类型
force_liqu_price:预估爆仓价
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|

3. POST /api/v1/future_trade   合约下单

URL `https://www.okex.com/api/v1/future_trade.do`  访问频率 5次/1秒(按币种单独计算) 	

示例	

```
# Request
POST https://www.okex.com/api/v1/future_trade.do
# Response
{
	"order_id":986,
   "result":true
}
```

返回值说明	

```
order_id ： 订单ID
result ： true代表成功返回
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|price|String|是|价格|
|amount|String|是|委托数量（张）|
|type|String|是|1:开多 2:开空 3:平多 4:平空|
|match_price|String|否|是否为对手价 0:不是    1:是   ,当取值为1时,price无效|
|lever_rate|String|否|杠杆倍数，下单时无需传送，系统取用户在页面上设置的杠杆倍数。且“开仓”若有10倍多单，就不能再下20倍多单|

4. POST /api/v1/future_trades_history    获取OKEX合约交易历史（非个人）访问频率 

URL `https://www.okex.com/api/v1/future_trades_history`   访问频率 2次/2秒 

示例	

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

返回值说明	

```
amount：交易数量
date：交易时间(毫秒)
price：交易价格
tid：交易ID
type：交易类型（buy/sell）
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|date|String|是|合约交割时间，格式yyyy-MM-dd|
|since|Long|是|交易Id起始位置|

5. POST /api/v1/future\_batch_trade   批量下单

URL `https://www.okex.com/api/v1/future_batch_trade.do`  访问频率 3次/1秒 最多一次下1-5个订单（按币种单独计算）	

示例	

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

返回值说明	

```
result:订单交易成功或失败
order_id:订单ID
备注:
     只要其中任何一单下单成功就返回true
      返回的结果数据和orders_data提交订单数据顺序一致
     如果下单失败：order_id为-1，error_code为错误代码
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|orders_data|String|是|JSON类型的字符串 例：[{price:5,amount:2,type:1,match\_price:1},{price:2,amount:3,type:1,match\_price:1}] 最大下单量为5，price,amount,type,match\_price参数参考future_trade接口中的说明|
|sign|String|是|请求参数的签名|
|lever_rate|String|否|杠杆倍数，下单时无需传送，系统取用户在页面上设置的杠杆倍数。且“开仓”若有10倍多单，就不能再下20倍多单|

6. POST /api/v1/future_cancel   取消合约订单

URL `https://www.okex.com/api/v1/future_cancel.do`  访问频率 2次/1秒，最多一次撤1-5个订单（按币种单独计算） 	

示例	

```
# Request
POST https://www.okex.com/api/v1/future_cancel.do
# Response
#多笔订单返回结果(成功订单ID,失败订单ID)
{
    "error":"161251:20015", //161251订单id 20015错误id
    "success":"161256"
}
#单笔订单返回结果
{
    "order_id":"161277",
    "result":true
}
```

返回值说明	

```
result:订单交易成功或失败(用于单笔订单)
order_id:订单ID(用于单笔订单)
success:成功的订单ID(用于多笔订单)
error:失败的订单ID后跟失败错误码(用户多笔订单)
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|order_id|String|是|订单ID(多个订单ID中间以","分隔,一次最多允许撤消5个订单)|
|sign|String|是|请求参数的签名|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|

7. POST /api/v1/future\_order\_info   获取合约订单信息

URL `https://www.okex.com/api/v1/future_order_info.do`  	

示例	

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

返回值说明	

```
amount: 委托数量
contract_name: 合约名称
create_date: 委托时间
deal_amount: 成交数量
fee: 手续费
order_id: 订单ID
price: 订单价格
price_avg: 平均价格
status: 订单状态(0等待成交 1部分成交 2全部成交 -1撤单 4撤单处理中 5撤单中)
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
type: 订单类型 1：开多 2：开空 3：平多 4： 平空
unit_amount:合约面值
lever_rate: 杠杆倍数  value:10\20  默认10 
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|status|String|否|查询状态 1:未完成的订单 2:已经完成的订单|
|order_id|String|是|订单ID -1:查询指定状态的订单，否则查询相应订单号的订单|
|current\_page|String|否|当前页数|
|page_length|String|否|每页获取条数，最多不超过50|

8. POST /api/v1/future\_orders\_info   批量获取合约订单信息

URL `https://www.okex.com/api/v1/future_orders_info.do`  


示例	

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

返回值说明	

```
amount: 委托数量
contract_name: 合约名称
created_date: 委托时间
deal_amount: 成交数量
fee: 手续费
order_id: 订单ID
price: 订单价格
price_avg: 平均价格
status: 订单状态(0等待成交 1部分成交 2全部成交 -1撤单 4撤单处理中)
symbol: btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
type: 订单类型 1：开多 2：开空 3：平多 4： 平空
unit_amount:合约面值
lever_rate: 杠杆倍数  value:10\20  默认10  
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|order_id|String|是|订单ID(多个订单ID中间以","分隔,一次最多允许查询50个订单)|

9. POST /api/v1/future\_userinfo\_4fix   获取逐仓合约账户信息

URL `https://www.okex.com/api/v1/future_userinfo_4fix.do`  

示例	

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

返回值说明	

```
balance:账户余额
available:合约可用
balance:账户(合约)余额
bond:固定保证金
contract_id:合约ID
contract_type:合约类别
freeze:冻结
profit:已实现盈亏
unprofit:未实现盈亏
rights:账户权益
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|

10. POST /api/v1/future\_position\_4fix   逐仓用户持仓查询

URL `https://www.okex.com/api/v1/future_position_4fix.do`  访问频率 10次/2秒 

示例	

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

返回值说明	

```
buy_amount:多仓数量
buy_available:多仓可平仓数量 
buy_bond:多仓保证金
buy_flatprice:多仓强平价格
buy_profit_lossratio:多仓盈亏比
buy_price_avg:开仓平均价
buy_price_cost:结算基准价
buy_profit_real:多仓已实现盈余
contract_id:合约id
contract_type:合约类型
create_date:创建日期
sell_amount:空仓数量
sell_available:空仓可平仓数量 
sell_bond:空仓保证金
sell_flatprice:空仓强平价格
sell_profit_lossratio:空仓盈亏比
sell_price_avg:开仓平均价
sell_price_cost:结算基准价
sell_profit_real:空仓已实现盈余
symbol:btc_usd   ltc_usd    eth_usd    etc_usd    bch_usd
lever_rate: 杠杆倍数
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|type|String|否|默认返回10倍杠杆持仓 type=1 返回全部持仓数据|

11. POST /api/v1/future_explosive   获取合约爆仓单

URL `https://www.okex.com/api/v1/future_explosive.do` 

示例	

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

返回值说明	

```
amount:委托数量（张）
create_date:创建时间
loss:穿仓用户亏损
price:委托价格
type：交易类型 1：买入开多 2：卖出开空 3：卖出平多 4：买入平空
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|contract\_type|String|是|合约类型: this\_week:当周   next\_week:下周   quarter:季度|
|status|String|是|状态 0：最近7天未成交 1:最近7天已成交|
|current\_page|Integer|否|当前页数索引值|
|page_number|Integer|否|当前页数(使用page\_number时current\_page失效，current\_page无需传)|
|page_length|Integer|否|每页获取条数，最多不超过50|

12. POST /api/v1/future_devolve   个人账户资金划转

URL `https://www.okex.com/api/v1/future_devolve.do`  	

示例	

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

返回值说明	

```
result:划转结果。若是划转失败，将给出错误码提示。
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|
|symbol|String|是|btc\_usd   ltc\_usd    eth\_usd    etc\_usd    bch\_usd|
|type|String|是|划转类型。1：币币转合约 2：合约转币币|
|amount|String|是| 划转币的数量|














