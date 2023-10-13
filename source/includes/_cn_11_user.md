# 用户信息

用户级别的操作api

## 获取用户激活、提现、转账等操作手续费

## 激活

## 提现

## 转账

## 查询交易详情

## 查询交易历史

## 查询AccountNonce

## todo 签名 


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
