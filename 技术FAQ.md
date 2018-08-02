# 常见问题    

## REST API

1. 访问限制     
答：一个IP五分钟之内，最多发送3000个https请求。若超出，系统会自动封IP一个小时，一个小时后自动解除。部分接口还有针对同一APIKEY的访问频次限制，具体限制见相关接口，超出后降低访问频次即恢复。       

2. 服务器返回10000错误码    
答：请检查接口所需参数是否正确无误提交给服务器，然后检查请求头信息content-type是否被设置成'application/x-www-form-urlencoded'

3. 服务器返回10006错误码    
答：请检查请求域名，OKEx用户请访问[www.okex.com](www.okex.com) 国际站用户请访问[www.okcoin.com](www.okcoin.com)      
    
## WebSocket API    

1. 如何判断服务端和客户端连接是否正常连接？     
答：OKEX通过心跳机制解决这个问题。客户端发送的心跳数据：{'event':'ping'}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。


## 其他相关

1. 如何创建APIKEY？  
 答：个人中心——>我的API——>创建API（勾选权限）

2. 申请的国际站APIKEY能在OKEx使用么？
 答：不能，国际站申请的APIKEY 只适用于https://www.okcoin.com OKEx站申请的APIKEY 仅适用于 https://www.okex.com 
 
3. API有示例demo么？  
 答：有，请访问https://github.com/okcoin-okex/API-docs-OKEx.com/blob/master/%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B.md
 
4. okex的wss://real.okex.com:10440/websocket/okexapi 地址调不通？  
 答：国内用户使用香港阿里云服务器或者国外服务器
 
5. 有测试环境提供给使用者吗？
 答：目前不提供测试环境，建议采取下单不成交的方式，进行测试。

6. 经常断线或者数据丢失，请求接口时提示“time out”？  
 答：解决办法国内用户使用香港阿里云服务器或者国外服务器

7. 生成sign签名失败？  
 答：首先比较所有参数名的第一个字母，按abcd顺序排列，若遇到相同首字母，则看第二个字母，以此类推
 
8. 有没有获取所有币种对的API接口？  
 答：https://www.okex.com/v1/tickers.do
 
9. https://www.okex.com/api/v1/userinfo.do 和 https://www.okex.com/api/v1/wallet_info.do 的区别？  
 答：userinfo 是现货账户信息，wallet_info是钱包账户信息。
 
10. 合约下单的时候报错“20007”？  
 答：API合约下单参数 amount字段  表示合约下单单位为”张“，value最小值是1。
 
11. 现货下单接口；“市价买单不传amount”这句话如何理解，系统如何知道我要买进多少？  
 答：市价买单情况下，由于价格不确定，所以需要传买入的总金额而不是数量。总金额数／市价 =买到的数量

12. "提币时为什么返回“10017（API鉴权失败）”？  
 答：创建APIKEY的时候需要勾选提币权限。
 
13. 当使用API提现带标签的币种时，如何添加标签？  
 答：使用「提币地址：tag」这种格式如xrp的提币地址   rUzWJkXyEtT8ekSSxkBYPqCvHpngcy6Fks:53780
 
14. 使用API下单，可以使用策略委托吗？  
 答：不可以，目前只支持限价单和市价单两种下单模式。将会在后续版本支持

15. 用ws订阅了合约的接口数据，为什么会与服务器断开？  
 答：OKEx通过心跳机制解决这个问题。客户端每30秒发送一次心跳数据：{"event":"ping"}，服务器会响应客户端：{"event":"pong"}以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。
