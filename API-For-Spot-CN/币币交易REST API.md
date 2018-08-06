# REST API for SPOT    

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

### 币币行情 API 

获取OKEx币币行情数据  

1. Get /api/v1/ticker    获取OKEx币币行情

URL `https://www.okex.com/api/v1/ticker.do`	

示例	

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

返回值说明	

```
date: 返回数据时服务器时间
buy: 买一价
high: 最高价
last: 最新成交价
low: 最低价
sell: 卖一价
vol: 成交量(最近的24小时)
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如ltc_btc|

2. Get /api/v1/depth   获取OKEx币币市场深度

URL `https://www.okex.com/api/v1/depth.do`	

示例	

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

返回值说明	

```
asks :卖方深度
bids :买方深度
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如ltc_btc|
|size|Integer|否(默认200)|value: 1-200|

3. Get /api/v1/trades   获取OKEx币币交易信息(60条)

URL `https://www.okex.com/api/v1/trades.do`	

示例	

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

返回值说明	

```
date:交易时间
date_ms:交易时间(ms)
price: 交易价格
amount: 交易数量
tid: 交易生成ID
type: buy/sell
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如ltc_btc|
|since|Long|否(默认返回最近成交60条)|tid:交易记录ID(返回数据不包括当前tid值,最多返回60条数据)|

4. Get /api/v1/kline    获取OKEx币币K线数据(每个周期数据条数2000左右)

URL `https://www.okex.com/api/v1/kline.do`	

示例	

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

返回值说明	

```
[
	1417536000000,	时间戳
	2370.16,	开
	2380,		高
	2352,		低
	2367.37,	收
	17259.83	交易量
]
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如ltc_btc|
|type|String|是|1min/3min/5min/15min/30min/1day/3day/1week/1hour/2hour/4hour/6hour/12hour|
|size|Integer|否(默认全部获取)|指定获取数据的条数|
|since|Long|否(默认全部获取)|时间戳，返回该时间戳以后的数据(例如1417536000000)|

### 币币交易 API 

用于OKEx币币交易  

1. POST /api/v1/userinfo    获取用户信息

URL `https://www.okex.com/api/v1/userinfo.do`	访问频率 6次/2秒    

示例	

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

返回值说明	

```
free:账户余额
freezed:账户冻结余额
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|

2. POST /api/v1/trade    下单交易

URL `https://www.okex.com/api/v1/trade.do`	访问频率 20次/2秒	

示例	

```
# Request
POST https://www.okex.com/api/v1/trade.do
# Response
{"result":true,"order_id":123456}
```

返回值说明	

```
result:true代表成功返回
order_id:订单ID
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_btc|
|type|String|是|买卖类型：限价单(buy/sell) 市价单(buy_market/sell_market)|
|price|Double|否|下单价格 市价卖单不传price|
|amount|Double|否|交易数量 市价买单不传amount,市价买单需传price作为买入总金额|
|sign|String|是|请求参数的签名|

3. POST /api/v1/batch_trade    批量下单

URL `https://www.okex.com/api/v1/batch_trade.do`	访问频率 20次/2秒	

示例	

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

返回值说明	

```
result:订单交易成功或失败
order_id:订单ID
备注:
     只要其中任何一单下单成功就返回true
     返回的订单信息和orders_data上传的订单顺序一致
     如果下单失败：order_id为-1，error_code为错误代码
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_btc|
|type|String|否|买卖类型：限价单(buy/sell)|
|orders_data|String(格式\[{price:3,amount:5,type:'sell'},{price:3,amount:3,type:'buy'}])|是|最大下单量为5， price和amount参数参考trade接口中的说明，最终买卖类型由orders_data 中type 为准，如orders_data不设定type 则由上面type设置为准。|
|sign|String|是|请求参数的签名|

4. POST /api/v1/cancel_order    撤销订单

URL `https://www.okex.com/api/v1/cancel_order.do`	访问频率 20次/2秒

示例	

```
# Request
POST https://www.okex.com/api/v1/cancel_order.do
# Response
#多笔订单返回结果(成功订单ID,失败订单ID)
{"success":"123456,123457","error":"123458,123459"}
```

返回值说明	

```
result:true撤单请求成功，等待系统执行撤单；false撤单失败(用于单笔订单)
order_id:订单ID(用于单笔订单)
success:撤单请求成功的订单ID，等待系统执行撤单(用于多笔订单)
error:撤单请求失败的订单ID(用户多笔订单)
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_btc|
|order_id|String|是|订单ID(多个订单ID中间以","分隔,一次最多允许撤消3个订单)|
|sign|String|是|请求参数的签名|

5. POST /api/v1/order_info    获取用户的订单信息

URL `https://www.okex.com/api/v1/order_info.do`	访问频率 20次/2秒(未成交)

示例	

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

返回值说明	

```
amount:委托数量
create_date: 委托时间
avg_price:平均成交价
deal_amount:成交数量
order_id:订单ID
orders_id:订单ID(不建议使用)
price:委托价格
status:-1:已撤销  0:未成交  1:部分成交  2:完全成交 3:撤单处理中
type:buy_market:市价买入 / sell_market:市价卖出
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_btc|
|order_id|Long|是|订单ID -1:未完成订单，否则查询相应订单号的订单|
|sign|String|是|请求参数的签名|

6. POST /api/v1/orders_info    批量获取用户订单

URL `https://www.okex.com/api/v1/orders_info.do`	访问频率 20次/2秒

示例	

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

返回值说明	

```
amount：限价单请求：下单数量 /市价单请求：卖出的btc/ltc数量
deal_amount：成交数量
avg_price：平均成交价
create_date：委托时间
order_id：订单ID
price：限价单请求：委托价格 / 市价单请求：买入的usd金额
status： -1：已撤销  0：未成交 1：部分成交 2：完全成交 4:撤单处理中
type:buy_market：市价买入 /sell_market：市价卖出
result：结果信息
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|type|Integer|是|查询类型 0:未完成的订单 1:已经完成的订单|
|symbol|String|是|币对如ltc_btc|
|order_id|String|是|订单ID(多个订单ID中间以","分隔,一次最多允许查询50个订单)|
|sign|String|是|请求参数的签名|

7. POST /api/v1/order_history    获取历史订单信息，只返回最近两天的信息

URL `https://www.okex.com/api/v1/order_history.do`	

示例	

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

返回值说明	

```
current_page:当前页码
orders:委托详细信息
amount:委托数量
avg_price:平均成交价
create_date:委托时间
deal_amount:成交数量
order_id:订单ID
price:委托价格
status:-1:已撤销   0:未成交 1:部分成交 2:完全成交 4:撤单处理中
type:buy_market:市价买入 / sell_market:市价卖出
page_length:每页数据条数
result:true代表成功返回
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_btc|
|status|Integer|是|查询状态 0：未完成的订单 1：已经完成的订单(最近两天的数据)|
|current_page|Integer|是|当前页数|
|page_length|Integer|是|每页数据条数，最多不超过200|
|sign|String|是|请求参数的签名|

8. POST /api/v1/withdraw  	 提币BTC/LTC/ETH/ETC/BCH

URL `https://www.okex.com/api/v1/withdraw.do`	

示例	

```
# Request
POST https://www.okex.com/api/v1/withdraw.do
# Response
{
    "withdraw_id":301,
    "result":true
}
```

返回值说明	

```
withdraw_id:提币申请Id
result:true表示请求成功
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_usd|
|chargefee|Double|是|网络手续费 >=0 BTC范围 \[0.002，0.005] LTC范围 \[0.001，0.2] ETH范围 \[0.01] ETC范围 \[0.0001，0.2] BCH范围 \[0.0005，0.002] 手续费越高，网络确认越快，向OKCoin提币设置为0|
|trade_pwd|String|是|交易密码|
|withdraw_address|String|是|认证的地址、邮箱或手机号码|
|withdraw_amount|Double|是|提币数量 BTC>=0.01 LTC>=0.1 ETH>=0.1 ETC>=0.1 BCH>=0.1|
|target|String|是|地址类型 okcn：国内站 okcom：国际站 okex：OKEX address：外部地址|
|sign|String|是|请求参数的签名|

9. POST /api/v1/cancel_withdraw   取消提币BTC/LTC/ETH/ETC/BCH

URL `https://www.okex.com/api/v1/cancel_withdraw.do`	

示例	

```
# Request
POST https://www.okex.com/api/v1/cancel_withdraw.do
# Response
{
    "result":true,
    "withdraw_id":301
}
```

返回值说明	

```
withdraw_id:提币申请Id
result:true表示请求成功
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_usd|
|withdraw_id|String|是|提币申请Id|
|sign|String|是|请求参数的签名|

10. POST /api/v1/withdraw_info    查询提币BTC/LTC/ETH/ETC/BCH信息

URL `https://www.okex.com/api/v1/withdraw_info.do`	

示例	

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

返回值说明	

```
result:true表示请求成功
address:提现地址
amount:提现金额
created_date:提现时间
chargefee:网络手续费
status:提现状态（-3:撤销中;-2:已撤销;-1:失败;0:等待提现;1:提现中;2:已汇出;3:邮箱确认;4:人工审核中5:等待身份认证）
withdraw_id:提币申请Id
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币对如ltc_usd|
|withdraw_id|String|是|提币申请Id|
|sign|String|是|请求参数的签名|

11. POST /api/v1/account_records    获取用户提现/充值记录

URL `https://www.okex.com/api/v1/account_records.do`	

示例	

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
            "bank": "中国银行",
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

返回值说明	

```
addr: 地址
account: 账户名称
amount: 金额
bank: 银行
benificiary_addr: 收款地址
transaction_value: 提现扣除手续费后金额
fee: 手续费
date: 时间
symbol: btc, ltc, eth, etc, bch, usdt
status: 记录状态,如果查询充值记录:(-1:充值失败;0:等待确认;1:充值成功),如果查询提现记录:(-3:撤销中;-2:已撤销;-1:失败;0:等待提现;1:提现中;2:已汇出;3:邮箱确认;4:人工审核中;5:等待身份认证)
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|symbol|String|是|币种如btc_usd, ltc_usd, eth_usd, etc_usd, bch_usd, usdt_usd|
|type|Integer|是|0：充值 1 ：提现|
|current_page|Integer|是|当前页数|
|page_length|Integer|是|每页数据条数，最多不超过50|
|sign|String|是|请求参数的签名|

12. POST /api/v1/funds_transfer    资金划转

URL `https://www.okex.com/api/v1/funds_transfer.do`	

示例	

```
# Request
POST https://www.okex.com/api/v1/funds_transfer.do
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
|symbol|String|是|btc_usd ltc_usd eth_usd etc_usd bch_usd|
|amount|Number|是|划转数量|
|from|Number|是|转出账户(1：币币账户 3：合约账户 6：我的钱包)|
|to|Number|是|转入账户(1：币币账户 3：合约账户 6：我的钱包)|
|sign|String|是|请求参数的签名|
	
13. POST /api/v1/wallet_info    获取用户钱包账户信息

URL `https://www.okex.com/api/v1/wallet_info.do`	访问频率 6次/2秒    

示例	

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

返回值说明	

```
free:账户余额
holds:账户锁定余额
```

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的apiKey|
|sign|String|是|请求参数的签名|

