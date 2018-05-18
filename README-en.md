# Getting Started

## API Overview

OKEx provides users with a set of simple and powerful development tools designed to help users quickly and efficiently integrate OKEx trading functions into their applications.

The OKEx interface is the basis for providing services. After the developer creates an account on the OKEx website, he can create APIs with different permissions according to his own needs and use the API to perform automatic transactions or cash withdrawals.

The following functions can be quickly implemented through the API:
- Get market updates
- Get sales depth information
- Query available and frozen amounts
- Check your current pending orders
- Quick Buy and Sell
- Batch withdrawal
- Quick withdrawal to your certification address

After obtaining interface permissions, you can help develop by reading this interface document.
    
## Description of interface call mode

OKEx provides users with three ways to call the interface. Developers can choose their own way to inquire about the market, make transactions or withdraw cash according to their own usage scenarios and preferences.

#### REST API

REST, short for Representational State Transfer, is the most popular Internet software architecture at present. It has a clear structure, conforms to standards, is easy to understand, and is easy to expand. It is being adopted by more and more websites. Its advantages are as follows:
- In a RESTful architecture, each URL represents a resource;
- Between the client and the server, a certain presentation layer of such resources is passed;
- The client uses the four HTTP commands to operate on the server-side resources to achieve "state transition of the presentation layer."

Developers are advised to use the REST API for currency exchange transactions or asset withdrawals.

#### WebSocket API

WebSocket is a new protocol for HTML5. It implements full-duplex communication between the client and the server, allowing data to be transmitted in both directions quickly. A simple handshake can establish a connection between the client and the server. The server can push information to the client based on the business rules. Its advantages are as follows:
- When the client and server perform data transmission, the request header information is relatively small, about 2 bytes;
- Both client and server can send data to the other party actively;
- No need to create TCP requests and destroy multiple times, saving bandwidth and server resources.

Developers are strongly advised to use the WebSocket API to obtain information such as market conditions and trading depth.

## Open API Permissions

The user's API permissions are obtained from the website's basic settings -> My API. Click Apply API to get, where apiKey is the access key that OKEx provides to API users and secretKey is the private key used to sign request parameters.

**_Note: Do not reveal these two parameters to anyone. These two parameters are related to the security of your account. _**
     
## parameter signature

User submitted parameters must be signed in addition to sign.

First, the string to be signed is ordered according to the parameter name (first compares the first letter of all parameter names, in abcd order, if you encounter the same first letter, then look at the second letter, and so on).

For example: sign the following parameters

String[] parameters={"api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c","symbol=btc_usdt","type=buy","price=680","amount=1.0"};

Generate a string to be signed

Amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=btc_usdt&type=buy

Then, the character to be signed is added with the private key parameter to generate the final to-be-signed character string. E.g:

Amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=btc_usdt&type=buy&secret_key=secretKey

Note that `&secret\_key=secretKey` is a mandatory parameter for the signature.

Finally, a 32-bit MD5 algorithm is used to perform a signature operation on the final string to be signed, thereby obtaining a signature result string (this string is assigned to the parameter sign). In the MD5 calculation result, letters are all capitalized.
