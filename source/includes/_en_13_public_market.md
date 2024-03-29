# Public Market

The public market API refers to an API that can be accessed directly without the need for logging in.

## Get Trading Information

API Description: Retrieve all current trades on the exchange.

API Path: GET [/v1/market/trades](https://testnet.app.zkex.com/v1/market/trades)

API Request Parameters:

- None

```
API response example:
```
| Field                      | Description      |
|:------------------------|---------|
| **spotsSymbols**        | Spot trading pair information |
| **contractsCurrencies** | Contract settlement currency  |
| **contractsSymbols**    | Contract trading pair information | 
| **indexes**             | Price information    |
| **spotsCurrencies**     | Spot currency |
| **riskLimits**          | Contract risk coefficient information |
| **chains**              | Supported chain information  |
| **currencies**          |  Currency basic information|
| **tokenMappingList**    |  Mapping relationship between spot, contract, and chain IDs|

Data Format:

`spotsSymbols`

| Field                       | Description                  |
|:--------------------------|---------------------|
| **id**                    | Spot trading pair ID             |
| **name**                  | Spot trading pair name, e.g., WBTC_USDC |
| **startTime**             | Symbol effective time         |
| **endTime**               | Symbol end time         |
| **trade**                 | Trading type SPOTS          |
| **baseName**              | baseName            |
| **baseMinimumIncrement**  | Minimum increment of base            |
| **baseScale**             | Base precision              |
| **baseMinimumQuantity**   | Minimum order quantity of base           |
| **baseMaximumQuantity**   | Maximum order quantity of base           |
| **quoteName**             | quote   Name        |
| **quoteMinimumIncrement** | Minimum increment of quote           |
| **quoteScale**            | Quote precision             |
| **supportMarginTrade**    | Support margin trading  |
| **alwaysChargeQuote**     | Whether always charges quote as fee     |

`contractsSymbols`

| Field                              | Description             |
|:--------------------------------|----------------|
| **id**                          | Contract ID           |
| **name**                        | Contract name, e.g., XBTC    |
| **startTime**                   | Contract effective time        |
| **endTime**                     | Contract end time        |
| **trade**                       | Trading type CONTRACTS |
| **type**                        | Contract type PERPETUAL |
| **baseCurrency**                | baseCurrency   |
| **marginCurrency**              | marginCurrency |
| **quoteCurrency**               | quoteCurrency  |
| **multiplier**                  | face value             |
| **quoteMinimumIncrement**       | Minimum increment of quote      |
| **quoteName**                   | quote   Name   |
| **baseScale**                   | base precision        |
| **quoteScale**                  | Quote precision        |
| **baseMinimumIncrement**        | Minimum increment of base       |
| **maximumQuantityPerOrder**     | Maximum order quantity per contract      |

`riskLimit`

| Field                        | Description       |
|:--------------------------|----------|
| **initialMarginRate**     | Initial margin rate   |
| **maintenanceMarginRate** | Maintenance margin rate   |
| **marginRateStep**        | Margin rate change step |
| **maxLeverage**           | Maximum leverage   |
| **riskLimitBase**         | Basic position    |
| **riskLimitStep**         | Position step    |
| **maxRiskLimitSteps**     | Maximum risk gradient count |


```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "spotsSymbols": [
      {
        "id": 100103,
        "name": "WBTC_USDC",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "BTC",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.5,
        "quoteScale": 1,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false
      }
    ],
    "contractsCurrencies": [
      "USDT"
    ],
    "contractsSymbols": [
      {
        "id": 145,
        "name": "XETH",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "CONTRACTS",
        "type": "PERPETUAL",
        "baseCurrency": "ETH",
        "marginCurrency": "USDT",
        "quoteCurrency": "USDT",
        "multiplier": 0.1,
        "quoteMinimumIncrement": 0.5,
        "quoteScale": 1,
        "baseScale": 0,
        "baseMinimumIncrement": 1,
        "maximumQuantityPerOrder": 100,
        "riskLimit": {
          "id": 106,
          "initialMarginRate": 0.02,
          "maintenanceMarginRate": 0.01,
          "marginRateStep": 0.01,
          "maxLeverage": 5,
          "riskLimitBase": 200,
          "riskLimitStep": 100,
          "maxRiskLimitSteps": 9,
          "createdAt": 1546956031080
        },
        "settlementFeeRate": 0.0,
        "referencedIndexes": {
          "SPOT": 10135,
          "FAIR": 10138,
          "PREMIUM_1M": 10139,
          "PREMIUM_AGGREGATE": 10140,
          "MAX_PREMIUM_RATE": 10137,
          "FUNDING_RATE_1M": 10141,
          "FUNDING_RATE_AGGREGATE": 10142,
          "LENDING_RATE_1D": 10136
        }
      }],                                                                                                                                                                                                                           ],
    "indexes": [
      {
        "id": 10109,
        "name": "BCH",  // Index Rate
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 2
      },
      {
        "id": 10110,
        "name": "BCH_DAILYRATE", // Daily Rate
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10111,
        "name": "BCH_MAXBCHUSDPI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10112,
        "name": "BCH_USD_FAIR", // Marked Rate
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10113,
        "name": "BCH_USD_PI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10114,
        "name": "BCH_USD_PI8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10115,
        "name": "BCH_FR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10116,
        "name": "BCH_FR8H", 
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10117,
        "name": "BCH_INS1H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10118,
        "name": "BCH_INS1D",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      }
    ],
    "spotsCurrencies": [
      "BTC",
      "ETH",
      "BCH",
      "ARM",
      "USDT"
    ],
    "riskLimits": [
      {
        "id": 105,
        "initialMarginRate": 0.01,
        "maintenanceMarginRate": 0.005,
        "marginRateStep": 0.005,
        "maxLeverage": 10,
        "riskLimitBase": 200,
        "riskLimitStep": 100,
        "maxRiskLimitSteps": 9,
        "createdAt": 1546956010600
      }
    ],
    "chains": {  // zk supported chain id
      "145": [    // tokenId
        {
          "id": 27,
          "chainTokenId": 145,  // tokenId
          "chainId": 1,    // chainId
          "address": "0x1f34934e3165b3e5428f6a4d873d2620302c7223", // address
          "decimals": 18,  // precision
          "fastWithdraw": true, // whether support fast withdraw
          "isSupport": true,
          "updateTime": 1684493403660
        },
        {
          "id": 28,
          "chainTokenId": 145,
          "chainId": 2,
          "address": "0x8f8b0bfb8458f73249024f22b4cf7b6c0eb60996",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493403857
        }
      ]
    },
    "tokenMappingList": [ // zkex symbol and zklink mapping relation
      {
        "id": 1,
        "tokenId": 100,  // zkex symbol Id
        "tokenName": "BTC", // zkex name
        "mappingTokenId": 1,  // zklink Id
        "tokenIconUrl": "https://static.zk.link/token/icons/default/btc.svg",
        "tokenType": 0    // symbol type 0 SPOTS 1 CONTRACTS
      },
      {
        "id": 3,
        "tokenId": 132,
        "tokenName": "XBTC",
        "mappingTokenId": 0,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/btc.svg",
        "tokenType": 1
      }
    ],
    "currencies": [
      {
        "id": 100,
        "name": "BTC",
        "main": false,  
        "conversionRatio": 1,  // Conversion rate of the collateral currency
        "createdAt": 0,
        "displayName": "Bitcoin"
      }
    ]
  }
}
```

## Get Time

API Description: Retrieve the current server time in milliseconds.

API Path: GET [/v1/market/timestamp](https://testnet.app.zkex.com/v1/market/timestamp)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "timestamp": 1688098283693
  }
}
```

## Get Exchange Rate

API Description: Retrieve the current market exchange rates.

API Path: GET [/v1/market/fex](https://testnet.app.zkex.com/v1/market/fex)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    // Base Exchange Rate:
    "from": "USD",
    // Conversion Rates:
    "tos": [
      "AUD",
      "CAD",
      "CNY",
      "EUR",
      "GBP",
      "HKD",
      "JPY",
      "KRW",
      "RUB",
      "SGD",
      "TWD"
    ],
    // Current Exchange Rates:
    "exchanges": {
      "USD_GBP": 0.79251,
      "USD_CAD": 1.32423,
      "USD_JPY": 144.76,
      "USD_HKD": 7.83533,
      "USD_TWD": 31.104,
      "USD_EUR": 0.9193,
      "USD_RUB": 87.2942,
      "USD_KRW": 1319.57,
      "USD_SGD": 1.35535,
      "USD_AUD": 1.5077,
      "USD_CNY": 7.2543
    }
  }
}
```

## Get System Error Codes

API Description: Retrieve all error codes defined in the system.

API Path: GET [/v1/market/errorCodes](https://testnet.app.zkex.com/v1/market/errorCodes)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ACCOUNT_FREEZE_FAILED": "Account error: freeze failed.",
    "ACCOUNT_NO_ENOUGH_AVAILABLE": "Account error: no enough available.",
    "ACCOUNT_NO_ENOUGH_BALANCE": "Account error: no enough balance."
  }
}
```

## Get Current System Indexes (Contracts)

API Description: Retrieve the latest values of all current indexes.

API Path: GET [/v1/market/indexes](https://testnet.app.zkex.com/v1/market/indexes)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC_USD_PI": [
      "1688098860000", // timestamp
      "0.000096"  // value
    ],
    "BTC_MAXBTCUSDPI": [
      "1546272000000",
      "0.0005"
    ]
  }
}
```

Some indexes represent prices, 
such as `BCH`, which is the spot-weighted index of the coin. Other indexes represent calculated values. 
For instance, `BCH_USD_PI` indicates the current premium rate of the coin against the US dollar, 
while `BCH_USD_FAIR` represents the mark price of the coin against the contract.

## Get Recent Historical Values for an Index (Contract)

API Description: Retrieve the recent historical values for a specific index.

API Path: GET /v1/market/indexes/:index_name

API Example: GET [/v1/market/indexes/ETH_USD_FAIR](https://testnet.app.zkex.com/v1/market/indexes/ETH_USD_FAIR)

API Path Parameters:

| Parameter     | Type     | Description                                            |
| :------- | -------- |:----------------------------------------------|
| **index_name** | **path** | **Required**<br>Index name, for example `ETH_USD_FAIR`  |

API Request Parameters:

| Parameter      | Type     | Description                                           |
| :-------- | :------- | ---------------------------------------------- |
| **start** | **long** | **Optional**<br/>Start timestamp, in milliseconds                |
| **end**   | **long** | **Optional**<br/>End timestamp, in milliseconds.                |
| **limit** | **long** | **Optional**<br/>The number of BARs to retrieve, default is 1000, maximum is 1000 |

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      [
        1688099240000,
        "564.05311"
      ]
    ]
  }
}
```

## Get 24-hour Statistical Price

API Description: Retrieve the latest 24-hour statistical price for a specific trading pair. The interface does not distinguish between spot and contract trading.

API Path: GET /v1/market/ticker/:symbolName

API Example: GET [/v1/market/ticker/WBTC_USDC](https://testnet.app.zkex.com/v1/market/ticker/WBTC_USDC)

API Path Parameters:

| Parameter            | Type     | Description                            |
|:---------------| -------- |:------------------------------|
| **symbolName** | **path** | **Required**<br>Symbol name, for example `WBTC_USDC` |

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688281000000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ]
  }
}
```
Note:

Data format:

`[timestamp, open, high, low, close, amount, volume]`



## Get All Statistical Prices

API Description: Retrieve the latest 24-hour statistical prices for all trading pairs. The interface does not distinguish between spot and contract trading pairs.

API Path: GET /v1/market/allTicker

API Example: GET [/v1/market/allTicker](https://testnet.app.zkex.com/v1/market/allTicker)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688278080000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ],
    "XBCH": [
      1688278080000,
      117.5,
      117.5,
      117.5,
      117.5,
      0.0,
      0.0
    ],
    "ETH_USDT": [
      1688278080000,
      1148.0,
      1148.0,
      1148.0,
      1148.0,
      0.0,
      0.0
    ]
  }
}
```



## Get All Spot Trading Pair Statistical Prices

API Description: Retrieve the latest 24-hour statistical prices for all spot trading pairs.

API Path: GET /v1/market/spots/allTicker

API Example: GET [/v1/market/spots/allTicker](https://testnet.app.zkex.com/v1/market/spots/allTicker)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ARM_USDT": null,
    "ETH_USDT": [
      1688284240000,
      1148.0,
      1148.0,
      1148.0,
      1148.0,
      0.0,
      0.0
    ],
    "BCH_USDT": [
      1688284240000,
      115.5,
      115.5,
      115.5,
      115.5,
      0.0,
      0.0
    ]
  }
}
```



## Get All Contract Trading Pair Statistical Prices

API Description: Retrieve the latest 24-hour statistical prices for all contract trading pairs.

API Path: GET /v1/market/contracts/allTicker

API Example: GET [/v1/market/contracts/allTicker](https://testnet.app.zkex.com/v1/market/contracts/allTicker)

API Request Parameters:

- None

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688278390000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ]
  }
}
```



## Get OrderBook

API Description: Retrieve the latest OrderBook for a specific trading pair.

API Path: GET /v1/market/orderBook/:symbol_name

API Example: GET [/v1/market/orderBook/WBTC_USDC](https://testnet.app.zkex.com/v1/market/orderBook/WBTC_USDC)

API Path Parameters:

| Parameter       | Type         | Description                                  |
| :--------- |------------|:------------------------------------|
| **symbol_name** | **path**   | **Required**<br>Symbol name, for example `WBTC_USDC`       |
| **depth** | **params** | **Optional**<br>Order book depth defaults to 25 levels and can go up to a maximum of 150 levels` |

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "symbolId": 100103,
    "symbol": "WBTC_USDC",
    "price": "40000.0",
    "sellOrders": [],
    "buyOrders": [
      [
        "39000.0",  // pirce
        "0.10"     // size
      ],
      [
        "38000.0",
        "0.10"
      ],
      [
        "36000.0",
        "0.10"
      ]
    ],
    "sequenceId": 651640,
    "direction": "SHORT",
    "timestamp": 1689064825725
  }
}
```

## Get Bar Data

API Description: Retrieve the latest bar data for a specific trading pair. The interface does not differentiate between spot and contract markets.

API Path: GET /v1/market/bars/:symbol_name/:type

API Example: GET [/v1/market/bars/WBTC_USDC/min](https://testnet.app.zkex.com/v1/market/bars/WBTC_USDC/min)

API Path Parameters:

|Parameter| Type | Description                                                                                       |
|:---|:---|------------------------------------------------------------------------------------------|
|**symbol**|**string**| **Required**<br>Symbol name, for example `WBTC_USDC`                                                          |
|**type**|**enum**| **Required**<br>Bar type, currently supports `MIN`, `MIN5`, `MIN15`, `MIN30`, `HOUR`, `HOUR4`, `DAY`, `WEEK`, `MONTH` |

API Request Parameters:

|Parameter| Type | Description |
|:---|:---|----|
|**start**|**long**|**Optional**<br/>Start timestamp, in milliseconds|
|**end**|**long**|**Optional**<br/>End timestamp, in milliseconds|
|**limit**|**long**|**Optional**<br/>The number of bars to retrieve, default is 200, maximum is 200|

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      [
        1685404800000,
        40000,
        40000,
        40000,
        40000,
        0.3,
        12000
      ],
      [
        1687132800000,
        40000,
        40000,
        40000,
        40000,
        1,
        40000
      ]
    ]
  }
}
```

Note：

bar data format：

`[timestamp, open, high, low, close, amount, volume]`

The results are always sorted in ascending order by timestamp.