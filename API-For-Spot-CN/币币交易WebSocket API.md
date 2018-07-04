# WebSocket API for SPOT    

## 开始使用    

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：   
- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节；   
- 客户端和服务器皆可以主动地发送数据给对方；   
- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。    
    
## 请求交互    

**币币交易**WebSocket服务连接地址：`wss://real.okex.com:10441/websocket`         

访问时需要科学上网
	
#### 发送请求    
请求数据格式为：  
`{'event':'addChannel','channel':'channelValue','parameters':{'api_key':'value1','sign':'value2'}} `  
其中  
event: addChannel(注册请求数据)/removeChannel(注销请求数据)   
channel: OKEx提供请求数据类型   
parameters: 参数为选填参数，其中api_key为用户申请的APIKEY，sign为签名字符串，签名规则参照请求说明   
    

例如： 
`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker' }")`  
`websocket.send("[{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker'},{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker'},{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker'}]")`,支持批量注册 

#### 服务器响应    
返回数据格式为： 
`[{"channel":"channel","success":"","errorcode":"","data":{}}, {"channel":"channel","success":"","errorcode":1,"data":{}}]`   
其中  
channel: 请求的数据类型  
result: true成功，false失败(用于WebSocket 交易API) 
data: 返回结果数据  
errorcode: 错误码(用于WebSocket 交易API)     

#### 推送过程说明   
为保证推送的及时性及减少流量，行情数据（ticker）和委托深度（depth）这两种数据类型只会在数据发生变化的情况下才会推送数据，交易记录（trades）是推送从上次推送到本次推送产生的增量数据。   

#### 如何判断连接是否断开
OKEx通过心跳机制解决这个问题。客户端每30秒发送一次心跳数据：{'event':'ping'}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。    

## API参考  

### 币币行情 API 

获取OKEx币币行情数据  

1. ok_sub_spot_X_ticker    订阅行情数据

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_ticker'}");`	

X值为币对，如ltc_btc

示例	

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_ticker'}
# Response
[
    {
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

返回值说明	

```
buy(double): 买一价
high(double): 最高价
last(double): 最新成交价
low(double): 最低价
sell(double): 卖一价
timestamp(long)：时间戳
vol(double): 成交量(最近的24小时)
```
	
2. ok_sub_spot_X_depth   订阅币币市场深度(200增量数据返回)

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_depth'}");`	

X值为币对，如ltc_btc

示例	

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_depth'}
# Response
[
    {
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

返回值说明	

```
bids([string, string]):买方深度
asks([string, string]):卖方深度
timestamp(string):服务器时间戳
```

使用描述    

第一次返回全量数据，根据接下来数据对第一次返回数据进行如下操作：删除（量为0时）；修改（价格相同量不同）；增加（价格不存在）。		


3. ok_sub_spot_X_depth_Y   订阅市场深度

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_depth_Y'}");`	

X值为币对，如ltc_btc	

Y值为获取深度条数，如5，10，20		

示例	

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_depth_5'}
# Response
[
    {
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

返回值说明	

```
bids([string, string]):买方深度
asks([string, string]):卖方深度
timestamp(long):服务器时间戳
```


4. ok_sub_spot_X_deals   订阅成交记录

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_deals'}");`	

X值为币对，如ltc_btc				

示例	

```
# Request
{'event':'addChannel','channel':'ok_sub_spot_bch_btc_deals'}
# Response
[{
    "channel":"ok_sub_spot_bch_btc_deals",
    "data":[["1001","2463.86","0.052","16:34:07","ask"]]
}]
```

返回值说明	

```
增量数据返回
[交易序号, 价格, 成交量, 时间, 买卖类型]
[string, string, string, string, string]
```


5. ok_sub_spot_X_kline_Y    订阅K线数据

`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_X_kline_Y'}");`	

X值为币对，如ltc_btc	

Y值为K线时间周期，如1min, 3min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, day, 3day, week				

示例	

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

返回值说明	

```
[时间,开盘价,最高价,最低价,收盘价,成交量]
[string, string, string, string, string, string]
```


### 币币交易 API 

获取OKEx币币交易数据  

1. login    登录事件(个人信息推送)

示例	

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

请求参数	

|参数名|	描述|
| :-----    | :-----   |
|api_key|用户申请的APIKEY|
|sign|请求参数的签名|

说明    	

订阅login后还需要订阅 ok_sub_spot_X_order 交易数据接口，和ok_sub_spot_X_balance账户信息接口。	


2. ok_sub_spot_X_order   交易数据

示例	

```
# Response
[
    {
        "base": "bch",
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

返回值说明	

```
createdDate(string):创建日期
orderId(long):订单id
tradeType(string):交易类型（buy:买入；sell:卖出；buy_market:按市价买入；sell_market:按市价卖出）
sigTradeAmount(string):单笔成交数量
sigTradePrice(string):单笔成交价格
tradeAmount(string):委托数量（市价卖代表要卖总数量；限价单代表委托数量）
tradeUnitPrice(string):委托价格（市价买单代表购买总金额； 限价单代表委托价格）
symbol(string):交易币对，如ltc_btc
completedTradeAmount(string):已完成成交量
tradePrice(string):成交金额
averagePrice(string):平均成交价
unTrade(string):当按市场价买币时表示剩余金额，其他情况表示此笔交易剩余买/卖币的数量
status(int):-1已撤销,0等待成交,1部分成交,2完全成交,4撤单处理中
```

请求参数	

|参数名|	描述|
| :-----    | :-----   |
|symbol|交易币对，如ltc_btc|	


3. ok_sub_spot_X_balance   账户信息		

示例	

```
# Response
[
    {
        "base": "bch",
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
]
```

返回值说明	

```
free:账户余额
freezed:账户冻结余额
```

请求参数	

|参数名|	描述|
| :-----    | :-----   |
|symbol|交易币对，如ltc_btc|	



