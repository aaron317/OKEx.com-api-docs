# SPOT REST API    

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
- 在RESTful架构中，每一个URL代表一种资源；    
- 客户端和服务器之间，传递这种资源的某种表现层；    
- 客户端通过四个HTTP指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用REST API进行币币交易或者资产提现等操作。    
    
## 请求交互    

REST访问的根URL：`https://www.okex.com/api/v1`     

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

3. Get /api/v1/trades   获取OKEx币币交易信息(600条)

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
|since|Long|否(默认返回最近成交600条)|tid:交易记录ID(返回数据不包括当前tid值,最多返回600条数据)|

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
                "usd": "0",
                "ltc": "0",
                "eth": "0"
            },
            "freezed": {
                "btc": "0",
                "usd": "0",
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
|sign|String|是|请求参数的签名|
|symbol|String|是|币对如ltc_btc|
|type|String|是|买卖类型：限价单(buy/sell) 市价单(buy_market/sell_market)|
|price|Double|否|下单价格 市价卖单不传price|
|amount|Double|否|交易数量 市价买单不传amount|

3. Get /api/v1/trades   获取OKEx币币交易信息(600条)

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
|since|Long|否(默认返回最近成交600条)|tid:交易记录ID(返回数据不包括当前tid值,最多返回600条数据)|

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

