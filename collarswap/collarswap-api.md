---
description: DRAFT COLLARSWAP DOCS - SUBJECT TO CHANGE WITHOUT NOTICE.
---

# CollarSwap API

CollarSwap.io General API

You’ll find some of the general requirements from CollarSwap.io to ensure smooth integration and data availability.  We provide publicly accessible, no-authentication API endpoints, with reasonable rate limits to ensure tickers/pairs list, market data, orderbook data can be queried on a minute basis. Data is available in JSON format.  A whitelist IP address may be granted to connecting partners where necessary.\
\
Spot Exchanges - Endpoints Overview \
CollarSwap offers the following 4 separate endpoints:\


<table><thead><tr><th width="109" align="center">No.</th><th width="216" align="center">Endpoint</th><th align="center">Description</th></tr></thead><tbody><tr><td align="center">1</td><td align="center">/pairs</td><td align="center">Details on crypto assets traded</td></tr><tr><td align="center">2</td><td align="center">/tickers</td><td align="center">Market related statistics for all markets for the last 24 hours.</td></tr><tr><td align="center">3</td><td align="center">/orderbook</td><td align="center">Order book depth of any given trading pair, split into two different arrays for bid and ask orders.</td></tr><tr><td align="center">4</td><td align="center">/historical</td><td align="center">Historical trade data for any given trading pair.</td></tr></tbody></table>

**Endpoint 1 - /pairs**

The /pairs endpoint provides a summary of cryptoasset trading pairs available on the exchange. For example, for Dog Collar (COLLAR):

{ \
&#x20;  “ticker\_id”: “COLLAR\_ETH”, \
&#x20;  "base": "COLLAR", \
&#x20;  "target": "ETH", \
}\
\
/pairs endpoint response description:

<table><thead><tr><th width="122">Name</th><th width="118">Data Type</th><th width="163">Category</th><th>Description</th></tr></thead><tbody><tr><td>ticker_id</td><td>string</td><td>Mandatory</td><td>Identifier of a ticker with delimiter to separate base/target, eg. COLLAR_ETH</td></tr><tr><td>base</td><td>string</td><td>Mandatory</td><td>Symbol/currency code of a the base cryptoasset, eg. COLLAR</td></tr><tr><td>Target</td><td>string</td><td>Mandatory</td><td>Symbol/currency code of the target cryptoasset, eg. ETH</td></tr><tr><td>pool_id</td><td>string</td><td>Recommended/Mandatory</td><td>pool/pair address or unique ID (Mandatory for DEX)</td></tr></tbody></table>

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

<table><thead><tr><th width="181">Name</th><th width="129">Data Type</th><th width="117">Category</th><th>Description</th></tr></thead><tbody><tr><td>ticker_id</td><td>string</td><td>Mandatory</td><td>Identifier of a ticker with delimiter to separate base/target, eg. COLLAR_ETH</td></tr><tr><td>base_currency</td><td>string</td><td>Mandatory</td><td>Symbol/currency code of base pair, eg. COLLAR</td></tr><tr><td>target_currency</td><td>string</td><td>Mandatory</td><td>Symbol/currency code of target pair, eg. ETH</td></tr><tr><td>last_price</td><td>decimal</td><td>Mandatory</td><td><p>Last transacted price of base currency based on given target currency (unit in base or target)<br></p><p>eg.<br>X = ? <br>1 base = X target <br>X base = 1 target</p></td></tr><tr><td>base_volume</td><td>decimal</td><td>Mandatory</td><td>24 hour trading volume in base pair volume (unit in base)</td></tr><tr><td>target_volume</td><td>decimal</td><td>Mandatory</td><td>24 hour trading volume in target pair volume (unit in target)</td></tr><tr><td>pool_id</td><td>string</td><td>Recommended/Mandatory</td><td>pool/pair address or unique ID (Mandatory for DEX)</td></tr><tr><td>bid</td><td>decimal</td><td>Recommended</td><td>Current <strong>highest bid price</strong></td></tr><tr><td>ask</td><td>decimal</td><td>Recommended</td><td>Current lowest ask price</td></tr><tr><td>high</td><td>decimal</td><td>Recommended</td><td>Rolling 24-hours highest transaction price</td></tr><tr><td>low</td><td>decimal</td><td>Recommended</td><td>Rolling 24-hours lowest transaction price</td></tr></tbody></table>

Reference: (Nov 21, 2022 will be added soon)\
\
Endpoint 3 - /orderbook (Order book depth details)&#x20;

The /orderbook/ticker\_id endpoint is to provide order book information with at least depth = 100 (50 each side) returned for a given market pair/ticker.

Endpoint parameters:

<table><thead><tr><th width="132">Name</th><th width="116">Data Type</th><th width="160">Category</th><th>Description</th></tr></thead><tbody><tr><td>ticker_id</td><td>String</td><td><strong>Mandatory</strong></td><td>A ticker such as "COLLAR_ETH", with delimiter between different cryptoassets</td></tr><tr><td>depth</td><td><strong>integer</strong></td><td><strong>Recommended</strong></td><td><p>Orders depth quantity: [0, 100, 200, 500...]. 0 returns full depth. Depth = 100 means 50 for each bid/ask side.</p><p>Note that for more liquid or closely priced pairs, the lack of order depth may result in a miscalculation of depth/spread.</p></td></tr></tbody></table>

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


<table><thead><tr><th width="138">Name</th><th width="121">Data Type</th><th width="153">Category</th><th>Description</th></tr></thead><tbody><tr><td>ticker_id</td><td>String</td><td>Mandatory</td><td>A pair such as "COLLAR_ETH", with delimiter between different cryptoassets</td></tr><tr><td>timestamp</td><td>timestamp</td><td>Recommended</td><td>Unix timestamp in milliseconds for when the last updated time occurred.</td></tr><tr><td>bids</td><td>decimal</td><td>Mandatory</td><td>An array containing 2 elements. The offer price and quantity for each bid order</td></tr><tr><td>asks</td><td>decimal</td><td>Mandatory</td><td>An array containing 2 elements. The ask price and quantity for each ask order</td></tr></tbody></table>

Reference: (Nov 21, 2022 will be added soon)
