# WebSocket API for SPOT    

## 开始使用    

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：   
- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节；   
- 客户端和服务器皆可以主动地发送数据给对方；   
- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。    
    
## 请求交互    

**币币交易**WebSocket服务连接地址：`wss://real.okex.com:10441/websocket`         
	
#### 发送请求    
请求数据格式为：  
`{'event':'addChannel','channel':'channelValue','parameters':{'api_key':'value1','sign':'value2'}} `  
其中  
event: addChannel(注册请求数据)/removeChannel(注销请求数据)   
channel: OKEx提供请求数据类型   
parameters: 参数为选填参数，其中api_key为用户申请的APIKEY，sign为签名字符串，签名规则参照请求说明   
binary: 参数为选填参数，是否为压缩数据。1 压缩数据；0 原始数据；默认0     

例如： 
`websocket.send("{'event':'addChannel','channel':'ok_sub_spot_usd_btc_ticker','binary','1'}")`  
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
OKEx通过心跳机制解决这个问题。客户端每30秒发送一次心跳数据：{'event':'ping'}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。    

## API参考  

### 币币行情 API 

获取OKEx币币行情数据  

