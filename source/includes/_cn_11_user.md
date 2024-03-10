# 用户信息

用户级别的操作api

## 获取用户操作手续费

API描述：查询用户激活、提现、转账等需要支付的手续费。

API路径：GET /v1/account/op/fee

是否需要鉴权：是

API请求参数：

| 参数         | 类型        | 说明                                               |
|:-----------|-----------|:-------------------------------------------------|
| *chainId*  | **param** | **非必填**<br> 操作指令对应的chain                         |
| *op*       | **param** | **必填**<br>  指令类型: Transfer/Withdraw/ChangePubKey |
| *address*  | **param** | **必填**<br> 操作地址: transfer 需要填to地址                |
| *currency* | **param** | **必填**<br> 操作币种： USDC WBTC等                      |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": "5.70000000",  // 对应操作 Currency 数量
  "success": true
}
```

## 激活

API描述：用户充值后需要进行账户激活，此操作需要进行L1签名及L2签名

API路径：POST /v1/account/0/changePubKey

是否需要鉴权：是

signature and ethAuthData 需要进行zkLink签名获取

```
API请求样例:
```
```json

{
    "currency": "BTC",
    "chainId": 2,
    "newPkHash": "0x8522196355f57a36e2d7b2347b6ab3fdd38f50d1",
    "fee": "0",
    "nonce": 1,
    "ts": 1690290259,
    "pubKey": "d99a5e3d62be94e5c1dcfd0a19540b17d466bade136051561d3f01d4f6208b26",
    "signature": "81662597c6937501c59c33fe6a180f52ca06ca7e7178893b6dacc981c03cd68e67da45256af33856a1c4c3aebd0b0e66758c3ad3af8966c366ed79852eba3b00",
    "ethAuthData": {
        "type": "EthECDSA",
        "ethSignature": "0x318a3105234e857052a4ddaf76d6b421e5eedb93244ccff275bb5e1a5f5bff734207a60bbecdbdfeb62d7a17b1c7d2c3a03c0df5f896876b053d1beb6c15614c1c"
    }
}

```


```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 312525971260161,
    "txHash": "0x9914039ef5845e478fa4c8d92cba5e5ccdd2b50e7c58fc145cbc67f94e83c271"
  },
  "success": true
}
```


## 提现

API描述：用户可以将资产进行提现，此操作需要进行L1签名及L2签名

API路径：POST /v1/account/0/withdraw

是否需要鉴权：是

signature and ethAuthData 需要进行zkLink签名获取

```
API请求样例:
```
```json

{
  "currency": "USDC",
  "toChainId": 7,
  "to": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
  "fee": "5.66",
  "amount": "10",
  "withdrawToL1": 0,
  "withdrawFeeRatio": 0,
  "nonce": 0,
  "ts": 1709787329,
  "signature": "41fc696284f30d9f5d1617dc195e2bb4133d1154723c61c71ee230c081161e04969a84c11fb02676c790a245f3e8f42fddeeda43830a26f69751cf7cea14b702",
  "ethSignature": {
    "type": "EthereumSignature",
    "signature": "0x2e1203ed691d9985331eb3fe238a8e040b52d6c8a69436b5119568f9a8f7a42f24edcd380bc809fbf1a60861b43c144e5e8b326c6534c6e35de953fd87ad651f1c"
  }
}

```


```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 312527078557313,
    "txHash": "0x7296ab4e047edfc6285ca1f2c16c1ceece5de6eaa58728ebd8cfcbd350994899"
  },
  "success": true
}
```



## 转账

API描述：用户可以将资产划转到指定的账户地址，此操作需要进行L1签名及L2签名

API路径：POST /v1/account/0/transfer

是否需要鉴权：是

signature and ethAuthData 需要进行zkLink签名获取

```
API请求样例:
```
```json

{
  "currency": "USDC",
  "to": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
  "fee": "1",
  "amount": "100",
  "nonce": 21,
  "ts": 1709787114,
  "signature": "c702fd9908ed2c17b9e453ab8a630c0595ab0d0f92d5fe64b7c0f63af86ce49caf8e9aa1bde78b9c2a5152364b8ab9675ace67a2d5ae5dcd4dbe5be5dd365301",
  "ethSignature": {
    "type": "EthereumSignature",
    "signature": "0x2597f04e7872e5d76e4b0975136dbece5b50ad4e638dc5f6fda37302fc863c983fb0fc52e4659a207c6d7a8fff713bea523d0d1f2095ca47f8e3085845ae7f5f1b"
  }
}

```


```
API响应样例:
```

```json

{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 312525275005953,
    "txHash": "0xa692f2447d6f41d101df7c48314ec63c3906072ff8d5e710dc57344a4f05d27d"
  },
  "success": true
}

```

## 查询交易详情

API描述：用户可以根据txId 查询交易详情

API路径：GET /v1/account/0/transaction

是否需要鉴权：是


API请求参数：

| 参数         | 类型        | 说明                     |
|:-----------|-----------|:-----------------------|
| *txId*  | **param** | **必填**<br> 操作指令成功返回的Id |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "txId": 312527078557313,
    "userId": 26687,
    "accountId": 0,
    "l2AccountId": null,
    "txHash": "0x7296ab4e047edfc6285ca1f2c16c1ceece5de6eaa58728ebd8cfcbd350994899",
    "ethHash": null,
    "txType": "WITHDRAW",
    "txStatus": "SUCCESS",
    "chainId": 7,
    "fromAddress": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
    "toAddress": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
    "currencyId": 103,
    "fee": 5.660000000000000000,
    "amount": 10.000000000000000000,
    "extra": "{\"withdrawFeeRatio\":0,\"withdrawToL1\":0}",
    "newPkHash": null,
    "nonce": null,
    "ts": null,
    "pubKey": null,
    "signature": null,
    "ethAuthData": null,
    "ethSignature": null,
    "createdAt": 1709787332053,
    "updatedAt": 1709787332053
  },
  "success": true
}
```

## 查询交易历史

API描述：用户可以查询账户操作历史记录

API路径：GET /v1/account/0/transactions

是否需要鉴权：是

API请求参数：

| 参数          | 类型        | 说明                                                        |
|:------------|-----------|:----------------------------------------------------------|
| *pageIndex* | **param** | **必填**<br> 索引页                                            |
| *pageSize*  | **param** | **必填**<br> 每页大小                                           |
| *type*      | **param** | **非必填**<br> 操作指令类型 Deposit/Transfer/Withdraw/ChangePubKey |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "total": 1,
    "pageIndex": 0,
    "pageSize": 10,
    "results": [
      {
        "txId": 312527078557313,
        "userId": 26687,
        "accountId": 0,
        "l2AccountId": null,
        "txHash": "0x7296ab4e047edfc6285ca1f2c16c1ceece5de6eaa58728ebd8cfcbd350994899",
        "ethHash": null,
        "txType": "WITHDRAW",
        "txStatus": "SUCCESS",
        "chainId": 7,
        "fromAddress": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
        "toAddress": "0xd554bb87ddd0ddb492506d70a7bdfb7fb355c27b",
        "currencyId": 103,
        "fee": 5.66,
        "amount": 10,
        "extra": "{\"withdrawFeeRatio\":0,\"withdrawToL1\":0}",
        "newPkHash": null,
        "nonce": null,
        "ts": null,
        "pubKey": null,
        "signature": null,
        "ethAuthData": null,
        "ethSignature": null,
        "createdAt": 1709787332053,
        "updatedAt": 1709787332053
      }
    ]
  },
  "success": true
}
```
## 查询AccountNonce

API描述：用户进行withdraw/changePubKey/transfer 时需要携带账户nonce，可以从该接口获取

API路径：GET /v1/accountNonce/gen/{nonceType}


是否需要鉴权：是

API请求参数：

| 参数          | 类型       | 说明                                           |
|:------------|----------|:---------------------------------------------|
| *nonceType* | **path** | **必填**<br> nonce类型 changePubKey: 0  other: 2 |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "nonce": 1,
    "status": true,
    "nonceType": 2
  },
  "success": true
}
```






## 查询账户资产
API描述：查询用户现货账户余额。

API路径：GET /v1/trading/:accountId/balances

API请求参数(PATH)：

| 参数        | 类型       | 说明               |
| :---------- |----------| :----------------- |
| *accountId* | **path** | **必填**<br>账户id |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC": [
      "0", // 可用 
      "0" // 现货冻结
    ],
    "USDT": [
      "0",
      "0"
    ]
  }
}
```
