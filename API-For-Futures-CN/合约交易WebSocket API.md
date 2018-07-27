# WebSocket API for FUTURES    

## 开始使用    

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：   
- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节；   
- 客户端和服务器皆可以主动地发送数据给对方；   
- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。    
    
## 请求交互    



**合约交易**WebSocket服务连接地址：`wss://real.okex.com:10440/websocket/okexapi`   

访问时需要科学上网        
	
#### 发送请求    
请求数据格式为：  
`{'event':'addChannel','channel':'channelValue','parameters':{'api_key':'value1','sign':'value2'}} `  
其中  
event: addChannel(注册请求数据)/removeChannel(注销请求数据)   
channel: OKEx提供请求数据类型   
parameters: 参数为选填参数，其中api_key为用户申请的APIKEY，sign为签名字符串，签名规则参照请求说明   
    

例如： 
`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_usd_btc_ticker'}")`  
`websocket.send("[{'event':'addChannel','channel':'ok_sub_spot_usd_btc_ticker'},{'event':'addChannel','channel':'ok_sub_spot_usd_btc_depth'},{'event':'addChannel','channel':'ok_sub_spot_usd_btc_trades'}]")`,支持批量注册 

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
OKEx通过心跳机制解决这个问题。客户端每30秒发送一次心跳数据：{"event":"ping"}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。    

## API参考  

### 合约行情 API 

获取OKEx合约行情数据
  
1. ok\_sub\_futureusd\_X\_ticker\_Y   订阅合约行情

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_ticker_Y'}");`  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg  
② Y值为：this\_week, next\_week, quarter
示例	

```
# Request 
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

返回值说明	

```
limitHigh(string):最高买入限制价格
limitLow(string):最低卖出限制价格
vol(double):24小时成交量
sell(double):卖一价格
buy(double): 买一价格
unitAmount(double):合约价值
hold_amount(double):当前持仓量
contractId(long):合约ID
high(double):24小时最高价格
low(double):24小时最低价格
```


2. ok\_sub\_futureusd\_X\_kline\_Y\_Z   订阅合约K线数据

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_kline_Y_Z'}");`  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg 
② Y值为：this\_week, next\_week, quarter  
③ Z值为：1min, 3min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, day, 3day, week
示例	

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

返回值说明	

```
[时间 ,开盘价,最高价,最低价,收盘价,成交量(张),成交量(币)]
[string, string, string, string, string, string]
```

3. ok\_sub\_futureusd\_X\_depth_Y   订阅合约市场深度(200增量数据返回)

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_depth_Y'}");`  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg 
② Y值为：this\_week, next\_week, quarter

示例	

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

返回值说明	

```
timestamp(long): 服务器时间戳
asks(array):卖单深度 数组索引(string) 0 价格, 1 量(张), 2 量(币) 3, 累计量(币) 
bids(array):买单深度 数组索引(string) 0 价格, 1 量(张), 2 量(币) 3, 累计量(币)
使用描述:
	1，第一次返回全量数据
	2，根据接下来数据对第一次返回数据进行，如下操作
	删除（量为0时）
	修改（价格相同量不同）
	增加（价格不存在）
```


4. ok\_sub\_futureusd\_X\_depth\_Y\_Z   订阅合约市场深度(全量返回)

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_depth_Y_Z'}");`	  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg 
② Y值为：this\_week, next\_week, quarter  
③ Z值为：5, 10, 20(获取深度条数) 

示例	

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

返回值说明	

```
timestamp(long): 服务器时间戳
asks(array):卖单深度 数组索引(string) [价格, 量(张), 量(币),累计量(币)]
bids(array):买单深度 数组索引(string) [价格, 量(张), 量(币),累计量(币)]
```


5. ok\_sub\_futureusd\_X\_trade\_Y   订阅合约交易信息

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_trade_Y'}");`  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg 
② Y值为：this\_week, next\_week, quarter   

示例	

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

返回值说明	

```
[交易序号, 价格, 成交量(张), 时间, 买卖类型，成交量(币-新增)]
[string, string, string, string, string, string]
```

6. ok\_sub\_futureusd\_X\_index   订阅合约指数

`websocket.send("{'event':'addChannel','channel':'ok_sub_futureusd_X_index'}");`  

① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg   
  

示例	

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

返回值说明	

```
futureIndex(string): 指数
timestamp(string): 时间戳
```

7. X\_forecast\_price   合约预估交割价格

	
① X值为：btc, ltc, eth, etc, bch,eos,xrp,btg  
  

示例	

```
# Response
[{
    "channel": "btc_forecast_price",
    "timestamp":"1490341322021",
    "data": "998.8"
}]
```

返回值说明	

```
data(string): 预估交割价格
timestamp(string): 时间戳
操作说明
无需订阅，交割前一小时自动返回

```

### 合约交易 API 

获取OKEx合约交易数据

1. login   登录事件(个人信息推送)  

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
| :-----   | :-----   |
|api_key|用户申请的apiKey|
|sign|请求参数的签名|

说明	

```
个人信息推送，个人数据有变化时会自动推送，其它旧的个人数据订阅类型可不订阅，
如:ok_sub_futureusd_trades,ok_sub_futureusd_userinfo,ok_sub_futureusd_positions
```

2. ok\_sub\_futureusd\_trades   交易信息  

示例	

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


返回值说明	

```
amount(double): 委托数量
contract_name(string): 合约名称
created_date(long): 委托时间
create_date_str(string):委托时间字符串
deal_amount(double): 成交数量
fee(double): 手续费
order_id(long): 订单ID
price(double): 订单价格
price_avg(double): 平均价格
status(int): 订单状态(0等待成交 1部分成交 2全部成交 -1撤单 4撤单处理中)
symbol(string): btc_usd   ltc_usd   eth_usd   etc_usd   bch_usd
type(int): 订单类型 1：开多 2：开空 3：平多 4：平空
unit_amount(double):合约面值
lever_rate(double):杠杆倍数  value:10/20  默认10
system_type(int):订单类型 0:普通 1:交割 2:强平 4:全平 5:系统反单
```

请求参数	

|参数名|	描述|
| :-----   | :-----   |
|api_key|用户申请的apiKey|
|sign|请求参数的签名|


3. ok\_sub\_futureusd\_userinfo   合约账户信息
  

示例	

```
# Response
逐仓信息
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
全仓信息
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


返回值说明	

```
全仓信息
balance(double): 账户余额
symbol(string)：币种
keep_deposit(double)：保证金
profit_real(double)：已实现盈亏
unit_amount(int)：合约价值
逐仓信息
balance(double):账户余额
available(double):合约可用
balance(double):合约余额
bond(double):固定保证金
contract_id(long):合约ID
contract_type(string):合约类别
freeze(double):冻结
profit(double):已实现盈亏
unprofit(double):未实现盈亏
rights(double):账户权益
```

说明	

```
登录后无需订阅，兼容旧的订阅方式，旧的订阅方式相当于登录操作。
Response:
[{"data":{"result":"true"},"channel":"login"}]
```


4. ok\_sub\_futureusd\_positions   合约持仓信息
  

示例	

```
# Response
全仓返回
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
逐仓返回
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


返回值说明	

```
全仓说明
position(string): 仓位 1多仓 2空仓
contract_name(string): 合约名称
costprice(string): 开仓价格
bondfreez(string): 当前合约冻结保证金
avgprice(string): 开仓均价
contract_id(long): 合约id
position_id(long): 仓位id
hold_amount(string): 持仓量
eveningup(string): 可平仓量
margin(double): 固定保证金
realized(double):已实现盈亏

逐仓说明
contract_id(long): 合约id
contract_name(string): 合约名称
avgprice(string): 开仓均价
balance(string): 合约账户余额
bondfreez(string): 当前合约冻结保证金
costprice(string): 开仓价格
eveningup(string): 可平仓量
forcedprice(string): 强平价格
position(string): 仓位 1多仓 2空仓
profitreal(string): 已实现盈亏
fixmargin(double): 固定保证金
hold_amount(string): 持仓量
lever_rate(double): 杠杆倍数
position_id(long): 仓位id
symbol(string): btc_usd   ltc_usd   eth_usd   etc_usd   bch_usd  eos_usd  xrp_usd btg_usd 
user_id(long):用户ID
```

说明	

```
登录后无需订阅，兼容旧的订阅方式，旧的订阅方式相当于登录操作。
Response:
[{"data":{"result":"true"},"channel":"login"}]
```
