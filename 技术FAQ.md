# 常见问题    

## REST API

- 访问限制
- 答：一个ip五分钟之内，最多发送3000个https请求。若超出，系统会自动封ip一个小时，一个小时后自动解除。部分接口还有针对同一apikey的访问频次限制，具体限制见相关接口，超出后降低访问频次即恢复。    
- 服务器返回10000错误码
- 答：请检查接口所需参数是否正确无误提交给服务器，然后检查请求头信息contentType是否被设置成'application/x-www-form-urlencoded'
- 服务器返回10006错误码
- 答：请检查请求域名，如果是国际站用户请访问www.okcoin.com，如果是国内账号请求访问www.okcoin.cn      
    
## WebSocket API    

- 如何判断服务端和客户端连接是否正常连接？
- 答：OKEX通过心跳机制解决这个问题。客户端发送的心跳数据：{'event':'ping'}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。
- 如何请求国际站与国内站的数据？
- 答：国内站api地址为:wss://real.okcoin.cn:10440/websocket/okcoinapi 
- 国际站api地址为:wss://real.okcoin.com:10440/websocket/okcoinapi
- OKEX api地址为:wss://real.okex.com:10440/websocket/okexapi
- 如何区分当前数据为国内站还是国际站的？
- 答：国内站数据类型名包含cny，例如：ok_btccny_ticker
- 国际站数据类型名包含usd，例如：ok_btcusd_ticker
