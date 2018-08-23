# Error Code For Futures

REST API Error Code

|Error Code |	 Description|
|---|---|
|20001	|User does not exist|
|20002	|Account frozen|
|20003	|Account frozen due to liquidation|
|20004	|Contract account frozen|
|20005	|User contract account does not exist|
|20006	|Required field missing|
|20007	|Illegal parameter|
|20008	|Contract account balance is too low|
|20009	|Contract status error|
|20010	|Risk rate ratio does not exist|
|20011	|Risk rate lower than 90%/80% before opening BTC position with 10x/20x leverage. or risk rate lower than 80%/60% before opening LTC position with 10x/20x leverage|
|20012	|Risk rate lower than 90%/80% after opening BTC position with 10x/20x leverage. or risk rate lower than 80%/60% after opening LTC position with 10x/20x leverage|
|20013	|Temporally no counter party price|
|20014	|System error|
|20015	|Order does not exist|
|20016	|Close amount bigger than your open positions|
|20017	|Not authorized/illegal operation|
|20018	|Order price cannot be more than 103% or less than 97% of the previous minute price|
|20019	|IP restricted from accessing the resource|
|20020	|secretKey does not exist|
|20021	|Index information does not exist|
|20022	|Wrong API interface (Cross margin mode shall call cross margin API, fixed margin mode shall call fixed margin API)|
|20023	|Account in fixed-margin mode|
|20024	|Signature does not match|
|20025	|Leverage rate error|
|20026	|API Permission Error|
|20027	|no transaction record|
|20028	|no such contract|
|20029	|Amount is large than available funds|
|20030	|Account still has debts|
|20038	|Due to regulation, this function is not availavle in the country/region your currently reside in.|
|20049  |Request frequency too high|
|21005	|Request interface failed, please retry|
|21020	|Contracts are being delivered, orders cannot be placed|
|21021	|Contracts are being settled, contracts cannot be placed|
|21023	|Current Holding Pos Exceeds the Max Allowed Limit (Cross Margin)|
|21024	|Current Holding Pos Exceeds the Max Allowed Limit (Fixed Margin)|
|21025	|Margin Ratio is lower than Min. Margin Requirment Upon Order Submission|
|21026	|Your account has been restricted from opening more positions|
|HTTP ERROR CODE 403|	Too many requests, IP is shielded.|
|Request Timed Out	|Too many requests, IP is shielded|

WebSocket API Error Code

|Error Code|	Description|
|---|---|
|10000	|Required parameter is empty|
|10001	|Illegal parameters|
|10002	|Authentication failure|
|10003	|This connection has requested other user data|
|10004	|This connection did not request this user data|
|10005	|api_key or sign is invalid|
|10008	|Parameter erorr|
|10009	|Order does not exist|
|10010	|Insufficient funds|
|10011	|Order quantity too low|
|10012	|Only support btc_usd ltc_usd|
|10014	|Order price must be between 0 - 1,000,000|
|10015	|Channel subscription temporally not available|
|10016	|Insufficient coins|
|10017	|WebSocket authorization error|
|10100	|user frozen|
|10216	|non-public API|
|10049	|User can not have more than 50 unfilled small orders (amount<0.15BTC)|
|20001	|user does not exist|
|20002	|user frozen|
|20003	|frozen due to force liquidation|
|20004	|contract account frozen|
|20005	|user contract account does not exist|
|20006	|required field can not be null|
|20007	|illegal parameter|
|20008	|contract account fund balance is zero|
|20009	|contract status error|
|20010	|risk rate information does not exist|
|20011	|risk rate bigger than 90% before opening position|
|20012	|risk rate bigger than 90% after opening position|
|20013	|temporally no counter party price|
|20014	|system error|
|20015	|order does not exist|
|20016	|liquidation quantity bigger than holding|
|20017	|not authorized/illegal order ID|
|20018	|order price higher than 105% or lower than 95% of the price of last minute|
|20019|	IP restrained to access the resource|
|20020|	secret key does not exist|
|20021|	index information does not exist|
|20022|	wrong API interface|
|20023|	fixed margin user|
|20024|	signature does not match|
|20025|	leverage rate error|
|20100|	request time out|
|20101|	the format of data is error|
|20102|	invalid login|
|20103|	event type error|
|20104|	subscription type error|
|20107|	JSON format error|
|20115|	The quote is not match|
|20116|	Param not match|
|1002|	The transaction amount exceed the balance|
|1003|	The transaction amount is less than the minimum requirement|
|1004|	The transaction amount is less than 0|
|1007|	No trading market information|
|1008|	No latest market information|
|1009|	No order|
|1010|	Different user of the cancelled order and the original order|
|1011|	No documented user|
|1013|	No order type|
|1014|	No login|
|1015|	No market depth information|
|1017|	Date error|
|1018|	Order failed|
|1019|	Undo order failed|
|1024|	Currency does not exist|
|1025|	No chart type|
|1026|	No base currency quantity|
|1027|	Incorrect parameter may exceeded limits|
|1028|	Reserved decimal failed|
|1029|	Preparing|
|1030|	Account has margin and futures, transactions can not be processed|
|1031|	Insufficient Transferring Balance|
|1032|	Transferring Not Allowed|
|1035|	Password incorrect|
|1036|	Google Verification code Invalid|
|1037|	Google Verification code incorrect|
|1038|	Google Verification replicated|
|1039|	Message Verification Input exceed the limit|
|1040|	Message Verification invalid|
|1041|	Message Verification incorrect|
|1042|	Wrong Google Verification Input exceed the limit|
|1043|	Login password cannot be same as the trading password|
|1044|	Old password incorrect|
|1045|	2nd Verification Needed|
|1046|	Please input old password|
|1048|	Account Blocked|
|1050|	Orders have been withdrawn or withdrawn|
|1051|	Order completed|
|1201|	Account Deleted at 00: 00|
|1202|	Account Not Exist|
|1203|	Insufficient Balance|
|1204|	Invalid currency|
|1205|	Invalid Account|
|1206|	Cash Withdrawal Blocked|
|1207|	Transfer Not Support|
|1208|	No designated account|
|1209|	Invalid api|
