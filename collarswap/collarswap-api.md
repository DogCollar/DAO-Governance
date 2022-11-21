---
description: DRAFT COLLARSWAP DOCS - SUBJECT TO CHANGE WITHOUT NOTICE.
---

# CollarSwap API

CollarSwap.io General API

You’ll find some of the general requirements from CollarSwap.io to ensure smooth integration and data availability.  We provide publicly accessible, no-authentication API endpoints, with reasonable rate limits to ensure tickers/pairs list, market data, orderbook data can be queried on a minute basis. Data is available in JSON format.  A whitelist IP address may be granted to connecting partners where necessary.\
\
Spot Exchanges - Endpoints Overview \
CollarSwap offers the following 4 separate endpoints:\


| No. |   Endpoint  |                                             Description                                             |
| :-: | :---------: | :-------------------------------------------------------------------------------------------------: |
|  1  |    /pairs   |                                   Details on crypto assets traded                                   |
|  2  |   /tickers  |                   Market related statistics for all markets for the last 24 hours.                  |
|  3  |  /orderbook | Order book depth of any given trading pair, split into two different arrays for bid and ask orders. |
|  4  | /historical |                          Historical trade data for any given trading pair.                          |

**Endpoint 1 - /pairs**

The /pairs endpoint provides a summary of cryptoasset trading pairs available on the exchange. For example, for Dog Collar (COLLAR):

{ \
&#x20;  “ticker\_id”: “COLLAR\_ETH”, \
&#x20;  "base": "COLLAR", \
&#x20;  "target": "ETH", \
}\
\
/pairs endpoint response description:

| Name       | Data Type | Category              | Description                                                                    |
| ---------- | --------- | --------------------- | ------------------------------------------------------------------------------ |
| ticker\_id | string    | Mandatory             | Identifier of a ticker with delimiter to separate base/target, eg. COLLAR\_ETH |
| base       | string    | Mandatory             | Symbol/currency code of a the base cryptoasset, eg. COLLAR                     |
| Target     | string    | Mandatory             | Symbol/currency code of the target cryptoasset, eg. ETH                        |
| pool\_id   | string    | Recommended/Mandatory | pool/pair address or unique ID (Mandatory for DEX)                             |

Reference: [https://api.collarswap.io/api/stake/pairs](https://api.collarswap.io/api/stake/pairs)



**Endpoint 2 - /tickers (Market Info)**

The /tickers endpoint provides 24-hour pricing and volume information on each market pair available on an exchange.

{ \
&#x20;  "ticker\_id": "COLLAR\_ETH",\
&#x20;  "base\_currency": "COLLAR",\
&#x20;  "target\_currency": "ETH",\
&#x20;  "last\_price":"50.0", \
&#x20;  "base\_volume":"10", \
&#x20;  "target\_volume":"500", \
&#x20;  "bid":"49.9", "ask":"50.1",\
&#x20;  "high":”51.3”, “low”:”49.2”,\
}

/tickers endpoint response description:

| Name             | Data Type | Category              | Description                                                                                                                                                              |
| ---------------- | --------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ticker\_id       | string    | Mandatory             | Identifier of a ticker with delimiter to separate base/target, eg. COLLAR\_ETH                                                                                           |
| base\_currency   | string    | Mandatory             | Symbol/currency code of base pair, eg. COLLAR                                                                                                                            |
| target\_currency | string    | Mandatory             | Symbol/currency code of target pair, eg. ETH                                                                                                                             |
| last\_price      | decimal   | Mandatory             | <p>Last transacted price of base currency based on given target currency (unit in base or target)<br></p><p>eg.<br>X = ? <br>1 base = X target <br>X base = 1 target</p> |
| base\_volume     | decimal   | Mandatory             | 24 hour trading volume in base pair volume (unit in base)                                                                                                                |
| target\_volume   | decimal   | Mandatory             | 24 hour trading volume in target pair volume (unit in target)                                                                                                            |
| pool\_id         | string    | Recommended/Mandatory | pool/pair address or unique ID (Mandatory for DEX)                                                                                                                       |
| bid              | decimal   | Recommended           | Current **highest bid price**                                                                                                                                            |
| ask              | decimal   | Recommended           | Current lowest ask price                                                                                                                                                 |
| high             | decimal   | Recommended           | Rolling 24-hours highest transaction price                                                                                                                               |
| low              | decimal   | Recommended           | Rolling 24-hours lowest transaction price                                                                                                                                |

Reference: (Nov 21, 2022 will be added soon)\
\
Endpoint 3 - /orderbook (Order book depth details)&#x20;

The /orderbook/ticker\_id endpoint is to provide order book information with at least depth = 100 (50 each side) returned for a given market pair/ticker.

Endpoint parameters:

| Name       | Data Type   | Category        | Description                                                                                                                                                                                                                                             |
| ---------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ticker\_id | String      | **Mandatory**   | A ticker such as "COLLAR\_ETH", with delimiter between different cryptoassets                                                                                                                                                                           |
| depth      | **integer** | **Recommended** | <p>Orders depth quantity: [0, 100, 200, 500...]. 0 returns full depth. Depth = 100 means 50 for each bid/ask side.</p><p>Note that for more liquid or closely priced pairs, the lack of order depth may result in a miscalculation of depth/spread.</p> |

Example query: .../api/orderbook?ticker\_id=COLLAR\_ETH\&depth=200

{\
"ticker\_id": "COLLAR\_ETH", "timestamp":"1700050000", "bids":\[\
\[\
"49.8", "0.50000000" ], \[\
"49.9", "6.40000000" ] ], "asks":\[\
\[\
"50.1", "9.20000000" ], \[\
"50.2", "7.9000000" ] ] }\
\
Order book response descriptions:\


| Name       | Data Type | Category    | Description                                                                     |
| ---------- | --------- | ----------- | ------------------------------------------------------------------------------- |
| ticker\_id | String    | Mandatory   | A pair such as "COLLAR\_ETH", with delimiter between different cryptoassets     |
| timestamp  | timestamp | Recommended | Unix timestamp in milliseconds for when the last updated time occurred.         |
| bids       | decimal   | Mandatory   | An array containing 2 elements. The offer price and quantity for each bid order |
| asks       | decimal   | Mandatory   | An array containing 2 elements. The ask price and quantity for each ask order   |

Reference: (Nov 21, 2022 will be added soon)
