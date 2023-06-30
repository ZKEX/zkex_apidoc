# 资产中心

资产中心API可用于获取资产中心账户余额、与币币账户、合约账户划转。



## 获取资产中心账户余额

API描述：查询用户资产中心账户余额。

API路径：GET /v2/assets/user/accounts

API请求参数：无

返回：

```json
{
    "USDS":[
        5024,
        0
    ],
    "BTC":[
        45.591,
        0
    ],
    "ETH":[
        5094.859925211325094096,
        0
    ],
    "USDT":[
        32934.951247512771511524,
        0
    ]
}
```



## 资产中心划转到币币账户

API描述：从资产中心划转资产到币币账户

API路径：POST /v2/assets/spots/transfer/out

API请求参数：JSON Object

| 参数         | 类型        | 说明                               |
| :----------- | ----------- | :--------------------------------- |
| **sourceId** | **string**  | **必填**<br>请求ID，唯一标识       |
| **currency** | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**   | **decimal** | **必填**<br/>划转数量              |

返回：

```json
{
    "id": 10650,
    "sourceId": "38dea7c5108f498a8c4036214d02cf66",
    "targetAddress": null,
    "transferType": "INTERNAL",
    "transferFrom": "ASSETS",
    "transferTo": "SPOTS",
    "userId": 122017,
    "userTargetId": null,
    "currencyId": 100,
    "amount": 1.000000000000000000,
    "fee": null,
    "status": "DONE",
    "confirm": 0,
    "txid": null,
    "msg": null,
    "sign": "9f083fddfefbce8316ad6fcb380f9b3f0eaa3a9c",
    "createdAt": 1603352591724,
    "updatedAt": 1603352591853,
    "currency": "BTC"
}
```





## 资产中心划转到合约账户

API描述：从资产中心划转资产到合约账户

API路径：POST /v2/assets/contracts/transfer/out

API请求参数：JSON Object

| 参数         | 类型        | 说明                               |
| :----------- | ----------- | :--------------------------------- |
| **sourceId** | **string**  | **必填**<br>请求ID，唯一标识       |
| **currency** | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**   | **decimal** | **必填**<br/>划转数量              |

返回：

```json
{
    "id": 10650,
    "sourceId": "38dea7c5108f498a8c4036214d02cf66",
    "targetAddress": null,
    "transferType": "INTERNAL",
    "transferFrom": "ASSETS",
    "transferTo": "CONTRACTS",
    "userId": 122017,
    "userTargetId": null,
    "currencyId": 100,
    "amount": 1.000000000000000000,
    "fee": null,
    "status": "DONE",
    "confirm": 0,
    "txid": null,
    "msg": null,
    "sign": "9f083fddfefbce8316ad6fcb380f9b3f0eaa3a9c",
    "createdAt": 1603352591724,
    "updatedAt": 1603352591853,
    "currency": "BTC"
}
```



## 资产中心划转到币信账户

API描述：从资产中心划转资产到币信账户

API路径：POST /v2/assets/bx/transfer/out

API请求参数：JSON Object

| 参数         | 类型        | 说明                               |
| :----------- | ----------- | :--------------------------------- |
| **sourceId** | **string**  | **必填**<br>请求ID，唯一标识       |
| **currency** | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**   | **decimal** | **必填**<br/>划转数量              |

返回：

```json
{
    "id": 10693,
    "sourceId": "0AA68271-F8C8-4503-8F8E-BC1CE813625D",
    "targetAddress": null,
    "transferType": "BX_INTERNAL",
    "transferFrom": "ASSETS",
    "transferTo": "BX",
    "userId": 121397,
    "userTargetId": "c32ceda0a68141ce9169f1bef12c1b08",
    "currencyId": 105,
    "amount": 1,
    "fee": null,
    "status": "PENDING",
    "confirm": 0,
    "txid": null,
    "msg": null,
    "sign": "dbeff52709cb6dc9b06d14442132aa87fe0c41f5",
    "createdAt": 1604038628500,
    "updatedAt": 1604038628500,
    "currency": "USDT"
}
```







