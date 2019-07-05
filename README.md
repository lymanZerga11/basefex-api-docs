Rest API 和 Websocket推送中文版文档
================

以下接口部分需要获取签名Token，请查看[这里(英文)](./sign.md)

<!-- toc -->

# 概览

  - REST API
      - [订单](#open-api-orders)
      - [持仓](#open-api-positions)
      - [交易](#open-api-trades)
      - [账户](#open-api-accounts)
      - [工具和其他](#open-api-misc)
  - WebSocket API
      - [Websocket推送接口](#open-api-ws)

## 网页端Swagger

  - 模拟交易环境: <https://testnet-api.basefex.com/explorer/index.html#/>
  - 正式交易环境:
<https://api.basefex.com/explorer/index.html#/>

## <span id="open-api-orders"> 订单(Orders) </span>

<!-- GET /orders/{id} -->

<!-- Done -->

### 获取单个订单

通过订单id获取订单详情。

##### URL

[https://api.basefex.com/orders/{id}](https://api.basefex.com/orders/%7Bid%7D)

##### HTTP请求方式

> GET

##### 请求参数

| 参数 | 必选                   | 类型     | 说明   |
| -- | -------------------- | ------ | ---- |
| id | :white\_check\_mark: | string | 订单id |

##### URL请求示例

<https://api.basefex.com/orders/5aec8f9f-1609-4e54-0005-86e30e0cb1c6>

##### 返回示例

``` js
{
  "liquidateUserId": null,
  "side": "BUY",                                    // 买卖方向
  "meta": {                                         // 
    "bestPrice": null,                              // 
    "markPrice": 10051.43,                          // 
    "bestPrices": {                                 // 
      "ask": null,                                  // 
      "bid": null                                   // 
    }                                               // 
  },                                                // 
  "userId": "5aec525e-335d-4724-0005-20153b361f89", // 
  "filledNotional": 0,                              // 已成交部分的价值
  "ts": 1562063567960,                              // 
  "notional": 0.102512,                             // 订单价值
  "status": "NEW",                                  // 订单状态
  "isLiquidate": false,                             // 是否强平
  "reduceOnly": false,                              // 是否只减仓
  "type": "LIMIT",                                  // 订单类型
  "symbol": "BTCUSD",                               // 合约类型
  "seqNo": null,                                    // 
  "filled": 0,                                      // 成交数量
  "conditional": null,                              // 
  "id": "5aec8f9f-1609-4e54-0005-86e30e0cb1c6",     // 
  "size": 1000,                                     // 订单大小（合约数量）
  "avgPrice": 0,                                    // 
  "price": 9755                                     // 订单价格
}                                                     
```

<!-- POST /orders -->

<!-- Done -->

### 提交单个订单

##### URL

<https://api.basefex.com/orders>

##### HTTP请求方式

> POST

##### 请求参数

| 参数          | 必选                   | 类型                | 说明                                                                                                                                     |
| ----------- | -------------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| size        | :white\_check\_mark: | number            | 合约数量                                                                                                                                   |
| symbol      | :white\_check\_mark: | string            | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| type        | :white\_check\_mark: |                   | 订单类型，可选值：`LIMIT`, `MARKET`, `IOC`, `FOK`, `POST_ONLY`                                                                                  |
| side        | :white\_check\_mark: | 买入`BUY`或者卖出`SELL` |                                                                                                                                        |
| price       |                      |                   | 订单价格                                                                                                                                   |
| reduceOnly  |                      |                   | 如果为true，则只减少持仓而不会增加持仓，即如果这个订单将要增加仓位时，这个订单将会被自动取消。                                                                                      |
| conditional |                      |                   | 可通过 conditional 是否为 null 来判断是否为条件委托，conditional的type目前只有 `REACH`，priceType 包括 `MARK_PRICE`、`INDEX_PRICE`、`MARKET_PRICE`                |

##### URL请求示例

<https://api.basefex.com/orders>

##### 请求体示例

``` js
{
  "size": 1,
  "symbol": "BTCUSD",
  "type": "LIMIT",
  "side": "BUY",
  "price":8000,
  "reduceOnly": false,
  "conditional": {
    "type": "REACH",
    "price": 8000,
    "priceType": "MARKET_PRICE"
  }
}
```

##### 返回示例

``` js
{
  "meta": null,                                     // 
  "reduceOnly": false,                              // 是否只支持平仓
  "symbol": "BTCUSD",                               // 合约类型
  "type": "LIMIT",                                  // 订单类型
  "userId": "5aec525e-335d-4724-0005-20153b361f89", // 
  "liquidateUserId": null,                          // 
  "size": 1,                                        // 订单大小（合约数量）
  "notional": 0,                                    // 订单价值
  "status": "UNTRIGGERED",                          // 订单状态
  "id": "5aed7b45-5d19-40f2-0005-ca49d01f33e3",     // 
  "side": "BUY",                                    // 买卖方向
  "filledNotional": 0,                              // 已成交价值
  "seqNo": null,                                    // 
  "filled": 0,                                      // 成交数量
  "price": 8000,                                    // 订单价格
  "conditional": {                                  // 
    "type": "REACH",                                // 条件
    "priceType": "MARKET_PRICE",                    // 使用哪种价格
    "price": 8000                                   // 触发条件价格
  }
}
```

<!-- DELETE /orders/{id} -->

<!-- Done -->

### 撤销单个订单

通过订单id撤销这个订单

##### URL

[https://api.basefex.com/orders/{id}](https://api.basefex.com/orders/%7Bid%7D)

##### HTTP请求方式

> DELETE

##### 请求参数

| 参数 | 必选                   | 类型     | 说明   |
| -- | -------------------- | ------ | ---- |
| id | :white\_check\_mark: | string | 订单id |

##### 请求URL示例

<https://api.basefex.com/orders/5aec8f9f-1609-4e54-0005-86e30e0cb1c6>

无返回内容

<!-- GET /orders -->

<!-- Done -->

### 获取订单列表（分页）

使用限定参数(可选)获取订单列表

##### URL

<https://api.basefex.com/orders>

##### HTTP请求方式

> GET

##### 请求参数

| 参数     | 必选 | 类型     | 说明                                                                                                                                     |
| ------ | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| id     |    | string | 前一次请求中最后一个订单的id，用于分页                                                                                                                   |
| type   |    | string | `LIMIT` `MARKET` `IOC` `FOK` `POST_ONLY`                                                                                               |
| side   |    | string | 买入`BUY`或者卖出`SELL`                                                                                                                      |
| status |    | string | 订单状态，包括`NEW`, `PARTIALLY_FILLED`, `PARTIALLY_CANCELED`, `CANCELED`, `REJECTED`, `FILLED`, `UNTRIGGERED`, `PENDING_CANCEL`, `TRIGGERED` |
| opt    |    | string | 已触发`TRIGGERED` 或者强平`LIQUIDATE`                                                                                                         |
| limit  |    | number | 单次请求结果数目限制                                                                                                                             |

无参数限定时返回所有订单。

##### 请求URL示例

<https://api.basefex.com/orders?symbol=BTCUSD&type=LIMIT&side=BUY&status=NEW&limit=30>

##### 返回示例

``` js
[
  {
    "liquidateUserId": null,                          // 
    "id": "5aed94fc-703e-42a2-0005-578d4c468767",     // 
    "ts": 1562132083136,                              // 时间戳
    "side": "BUY",                                    // 买卖方向
    "userId": "5aec525e-335d-4724-0005-20153b361f89", // 
    "filledNotional": 0,                              // 已成交价值
    "notional": 0.102512,                             // 订单价值
    "status": "NEW",                                  // 订单状态
    "isLiquidate": false,                             // 是否强平
    "reduceOnly": false,                              // 是否只支持减仓
    "type": "LIMIT",                                  // 订单类型
    "symbol": "BTCUSD",                               // 合约类型
    "seqNo": null,                                    // 
    "filled": 0,                                      // 已成交数量
    "conditional": null,                              // 
    "size": 1000,                                     // 
    "avgPrice": 0,                                    // 
    "price": 9755,                                    // 订单价格
    "meta": {                                         // 
      "bestPrice": 11311,                             // 
      "markPrice": 11536.1075491,                     // 
      "bestPrices": {                                 // 
        "ask": 11311,                                 // 
        "bid": 11305.5                                // 
      }
    }
  }
]
```

<!-- POST /orders/batch -->

### 批量提交订单

##### URL

<https://api.basefex.com/orders/batch>

##### HTTP请求方式

> POST

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| orders | :white\_check\_mark: |        | price为合约单价，类型为number；size为合约数量，类型为number；side为买卖方向，`BUY`或者`SELL`，类型为string                                                             |

##### 请求示例URL

<https://api.basefex.com/orders/batch>

##### 请求体示例

``` js
{
  "symbol": "BTCUSD",
  "orders": [
    {
      "price": 11234,
      "size": 200,
      "side": "BUY"
    }
  ]
}
```

##### 返回示例

``` js
[
  {
    "liquidateUserId": null,                           // 
    "price": 11234,                                    // 价格
    "size": 200,                                       // 合约数量
    "id": "5aedb78e-6641-4d00-0005-2b2439f84663",      // 
    "userId": "5aec525e-335d-4724-0005-20153b361f89",  // 
    "filledNotional": 0,                               // 已成交价值
    "ts": 1562141145497,                               // 订单提交时间
    "status": "NEW",                                   // 订单状态
    "isLiquidate": false,                              // 是否强平
    "reduceOnly": false,                               // 是否只支持减仓
    "type": "LIMIT",                                   // 订单种类
    "symbol": "BTCUSD",                                // 合约类型
    "seqNo": null,                                     // 
    "filled": 0,                                       // 已成交数量
    "conditional": null,                               // 委托条件
    "side": "BUY",                                     // 买卖方向
    "avgPrice": 0,                                     // 
    "meta": {                                          // 
      "markPrice": 11303.14,                           // 标记价格
      "bestPrices": {                                  // 
        "ask": 11311,                                  // 卖盘最低价
        "bid": 11305.5                                 // 买盘最高价
      },                                               // 
      "bestPrice": 11305.5                             // 
    },                                                 // 
    "notional": 0.017804                               // 订单价值
  }                                                    
]
```

<!-- DELETE /orders -->

<!-- Deprecate -->

<!-- ### 批量撤销订单

##### URL

https://api.basefex.com/orders

##### HTTP请求方式

> DELETE

https://api.basefex.com/orders?symbol=BTCUSD&side=BUY

##### 请求参数

| 参数   | 必选 | 类型   | 说明                                   |
|--------|------|--------|----------------------------------------|
| symbol |      | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| side   |      | string | `BUY` `SELL`                           |

无返回内容 -->

### 批量撤销订单

##### URL

<https://api.basefex.com/orders/batch>

##### HTTP请求方式

> DELETE

##### 请求参数

| 参数  | 必选 | 类型       | 说明     |
| --- | -- | -------- | ------ |
| ids |    | string列表 | 订单id列表 |

##### 请求示例URL

<https://api.basefex.com/orders/batch>

##### 请求体示例

``` js
{
  "ids": [
    "5aedb78e-6641-4d00-0005-2b2439f84663",
    "5aed7b45-5d19-40f2-0005-ca49d01f33e3"
  ]
}
```

无返回内容

<!-- GET /orders/opening -->

<!-- Done -->

### 获取活跃订单

##### URL

<https://api.basefex.com/orders/opening>

##### HTTP请求方式

> GET

##### 请求参数

| 参数     | 必选 | 类型     | 说明                                                                                                                                     |
| ------ | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| id     |    | string | 前一次请求中最后一个订单的id，用于分页                                                                                                                   |
| limit  |    | number | 单次请求的结果数目限制，不传值则为100                                                                                                                   |

##### URL请求示例

<https://api.basefex.com/orders/opening?symbol=BTCUSD&limit=30>

##### 返回示例

``` js
[
  {
    "liquidateUserId": null,                          // 
    "ts": 1562125342068,                              // 
    "size": 1,                                        // 
    "notional": 0,                                    // 订单价值
    "side": "BUY",                                    // 买卖方向
    "userId": "5aec525e-335d-4724-0005-20153b361f89", // 
    "filledNotional": 0,                              // 已成交价值
    "isLiquidate": false,                             // 是否强平
    "reduceOnly": false,                              // 是否只支持减仓
    "type": "LIMIT",                                  // 订单类型
    "symbol": "BTCUSD",                               // 合约类型
    "seqNo": null,                                    // 
    "filled": 0,                                      // 成交数量
    "meta": null,                                     // 
    "status": "UNTRIGGERED",                          // 订单状态
    "avgPrice": 0,                                    // 
    "price": 8000,                                    // 
    "conditional": {                                  // 委托条件
      "type": "REACH",                                // 委托类型
      "price": 8000,                                  // 委托价格
      "priceType": "MARKET_PRICE"                     // 使用哪种价格
    },                                                // 
    "id": "5aed7b45-5d19-40f2-0005-ca49d01f33e3"      // 
  }                                                     
]                                                     
```

<!-- GET /orders/count -->

<!-- Done -->

### 获取订单数目

##### URL

<https://api.basefex.com/orders/count>

##### HTTP请求方式

> GET

##### 请求参数

| 参数     | 必选 | 类型     | 说明                                                                                                                                     |
| ------ | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| id     |    | string | 前一次请求中最后一个订单的id，用于分页                                                                                                                   |
| type   |    | string | `LIMIT` `MARKET` `IOC` `FOK` `POST_ONLY`                                                                                               |
| side   |    | string | 买入`BUY`或者卖出`SELL`                                                                                                                      |
| status |    | string | 订单状态，包括`NEW`, `PARTIALLY_FILLED`, `PARTIALLY_CANCELED`, `CANCELED`, `REJECTED`, `FILLED`, `UNTRIGGERED`, `PENDING_CANCEL`, `TRIGGERED` |
| opt    |    | string | `TRIGGERED` `LIQUIDATE`                                                                                                                |

##### 请求示例URL

<https://api.basefex.com/orders/count?symbol=BTCUSD&type=LIMIT&side=BUY&status=NEW>

##### 返回示例

``` js
{
  "count": 1             // 数量
}
```

<!-- GET /orders/opening/count -->

<!-- Done -->

### 获取活跃订单数目

##### URL

<https://api.basefex.com/orders/opening/count>

##### HTTP请求方式

> GET

##### 请求参数

| 参数     | 必选 | 类型     | 说明                                                                                                                                     |
| ------ | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例URL

<https://api.basefex.com/orders/opening/count?symbol=BTCUSD>

##### 返回示例

``` js
{
  "count": 2                 // 数量
}
```

## <span id="open-api-positions"> 持仓(Positions) </span>

### 杠杆调节

##### URL

[https://api.basefex.com/positions/{symbol}/margin](https://api.basefex.com/positions/%7Bsymbol%7D/margin)

##### HTTP请求方式

> PUT

##### 请求参数

| 参数       | 必选                   | 类型      | 说明                                                                                                                                     |
| -------- | -------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol   | :white\_check\_mark: | string  | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| margin   |                      | number  |                                                                                                                                        |
| leverage |                      | number  | 杠杆倍数，isCross为false时生效。                                                                                                                 |
| isCross  |                      | boolean | true为全仓，false时leverage字段生效                                                                                                             |

##### 请求示例URL

<https://api.basefex.com/positions/BTCUSD/margin>

##### 请求体示例

``` js
{
  "margin": 0,
  "leverage": 0,
  "isCross": true
}
```

##### 返回示例

``` js
{
  "feeRateMaker": -0.0002,                             // maker费率
  "id": "5aec8f3f-9846-41af-0005-1b306eeb533a",        // 
  "value": 0,                                          // 仓位价值
  "marginRate": 0.01,                                  // 保证金比率
  "size": 0,                                           // 
  "liquidatePrice": 0,                                 // 强平价格
  "markPrice": 11303.14,                               // 标记价格
  "risk": 0,                                           // 风险程度                                         
  "notional": 0,                                       // 仓位名义价值
  "userId": "5aec525e-335d-4724-0005-20153b361f89",    // 用户ID   
  "buyingNotional": 0,                                 // 
  "isCross": true,                                     // 是否全仓      
  "entryPrice": 0,                                     // 入场价格
  "symbol": "BTCUSD",                                  // 合约类型
  "seqNo": null,                                       // 
  "sellingNotional": -0.116909,                        // 
  "riskLimit": 100,                                    // 风险限额
  "totalPnl": 0,                                       // 总盈亏
  "feeRateTaker": 0.0007,                              // taker费率
  "sellingSize": -1400,                                // 卖出数量
  "unrealizedPnl": 0,                                  // 未实现盈亏
  "realisedPnl": 0,                                    // 已实现盈亏
  "equity": 0,                                         // 
  "buyingSize": 0,                                     // 
  "orderMargin": 0.0013327626,                         // 订单保证金
  "leverage": 100,                                     // 杠杆
  "margin": 0,                                         // 保证金
  "rom": 0                                             // 回报率
}
```

### 设置风险限额

##### URL

[https://api.basefex.com/positions/{symbol}/risk-limit](https://api.basefex.com/positions/%7Bsymbol%7D/risk-limit)

##### HTTP请求方式

> PUT

##### 请求参数

| 参数       | 必选                   | 类型     | 说明                                                                                                                                     |
| -------- | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol   | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| notional | :white\_check\_mark: | number |                                                                                                                                        |

##### 请求示例URL

<https://api.basefex.com/positions/BTCUSD/risk-limit>

##### 请求体示例

``` js
{
  "notional": 200
}
```

##### 返回示例

``` js
{
  "notional": 200, 
  "IMR": 0.015   // initial margin rate，初始保证金比率
}
```

## <span id="open-api-trades"> 账户(Trades) </span>

### 获取交易列表

##### URL

<https://api.basefex.com/trades>

##### HTTP请求方式

> GET

##### 请求参数

| 参数       | 必选 | 类型     | 说明                                                                                                                                     |
| -------- | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol   |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| id       |    | string | 前一次请求中最后一个订单的id，用于分页                                                                                                                   |
| limit    |    | number | 单次请求结果数目限制，不传值则为10                                                                                                                     |
| side     |    | string | 买入`BUY`，卖出`SELL`，或者平台每8个小时触发的`FUNDING`                                                                                                 |
| order-id |    | string | 订单id                                                                                                                                   |

##### 请求示例URL

<https://api.basefex.com/trades?limit=30&side=SELL>

##### 返回示例

``` js
[
  {
    "fee": 0.00006200177148,                           // 手续费
    "symbol": "BTCUSD",                                // 合约类型
    "feeRate": 0.0007,                                 // 费率
    "size": 1000,                                      // 合约数量
    "ts": 1562244089804,                               // 成交时间
    "notional": 0.08857395925,                         // 成交价值
    "orderId": "5aef4041-ee43-4a60-0005-705a0f1edcb4", // 订单id
    "id": "5aef4041-f300-0000-0001-00000000001b",      // 交易id
    "side": "SELL",                                    // 卖出（还是买入）
    "order": {                                         // 
      "id": "5aef4041-ee43-4a60-0005-705a0f1edcb4",    // 订单id
      "type": "LIMIT",                                 // 订单类型
      "size": 1000,                                    //
      "price": 11290                                   // 
    },
    "price": 11290
  }
]
```

### 获取交易数量

##### URL

<https://api.basefex.com/trades/count>

##### HTTP请求方式

> GET

##### 请求参数

| 参数       | 必选 | 类型     | 说明                                                                                                                                     |
| -------- | -- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol   |    | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |
| id       |    | string | 前一次请求中最后一个订单的id，用于分页                                                                                                                   |
| limit    |    | number | 单次请求结果数目限制，不传值则为10                                                                                                                     |
| side     |    | string | 买入`BUY`，卖出`SELL`，或者平台每8个小时触发的`FUNDING`                                                                                                 |
| order-id |    | string | 订单id                                                                                                                                   |

##### 请求示例URL

<https://api.basefex.com/trades/count?symbol=BTCUSD&limit=30&side=BUY>

##### 返回示例

``` js
{
  "count": 3       // 数量
}
```

## <span id="open-api-accounts"> 账户(Accounts) </span>

### 获取账户余额和持仓详情

##### URL

<https://api.basefex.com/accounts>

##### HTTP请求方式

> GET

##### 请求参数

无

##### 请求示例URL

<https://api.basefex.com/accounts>

##### 返回示例

``` js
{
  "cash": {                                            // 结算方式
    "orderMargin": 0.0983953764,                       // 委托保证金
    "balances": 1000,                                  // 余额
    "marginRate": 0,                                   // 保证金比率
    "userId": "5aec525e-335d-4724-0005-20153b361f89",  // 
    "leverage": 0,                                     // 杠杆倍数
    "marginBalances": 1000,                            // 保证金余额
    "positionMargin": 0,                               // 仓位保证金
    "available": 999.9016046236,                       // 可用余额
    "unrealizedPnl": 0,                                // 未实现盈亏
    "id": "5aec8f3b-ea46-4eaa-0005-639ddae22e5f",      // 
    "currency": "BTC",                                 // 货币类型
    "margin": 0                                        // 保证金
  },
  "positions": {
    "ETHXBT": {                                          // 合约类型
      "markPrice": 0.02608751,                           // 标记价格
      "value": 0,                                        // 
      "size": 0,                                         // 合约数量
      "liquidatePrice": 0,                               // 
      "risk": 0,                                         // 风险程度
      "symbol": "ETHXBT",                                // 合约类型
      "notional": 0,                                     // 仓位价值
      "userId": "5aec525e-335d-4724-0005-20153b361f89",  // 
      "buyingNotional": 0,                               // 买入总价值
      "isCross": true,                                   // 是否满仓
      "feeRateMaker": 0,                                 // maker费率
      "entryPrice": 0,                                   // 入场价格
      "sellingNotional": 0,                              // 卖出总价值
      "marginRate": 0.02,                                // 保证金比率
      "id": "5aec8f3f-ad95-43a7-0005-c80b43cdb509",      // 
      "seqNo": null,                                     // 
      "leverage": 50,                                    // 杠杆倍数
      "totalPnl": 0,                                     // 总盈亏
      "unrealizedPnl": 0,                                // 未实现盈亏
      "feeRateTaker": 0.002,                             // 
      "orderMargin": 0,                                  // 
      "sellingSize": 0,                                  // 买入合约数量
      "realisedPnl": 0,                                  // 已实现盈亏
      "equity": 0,                                       // 
      "buyingSize": 0,                                   // 买入数量
      "riskLimit": 50,                                   // 风险限额
      "margin": 0,                                       // 保证金
      "rom": 0                                           // 回报率
    },
    "anotherContract": {
        ...
    },
    ...
  }
}
```

### 获取充值和提现记录

##### URL

<https://api.basefex.com/accounts/transactions>

##### HTTP请求方式

> GET

##### 请求参数

| 参数    | 必选 | 类型     | 说明                              |
| ----- | -- | ------ | ------------------------------- |
| type  |    | string | 交易类型，包括充值`DEPOSIT`和提现`WITHDRAW` |
| id    |    | string | 前一次请求中最后一个订单的id，用于分页            |
| limit |    | number | 单次请求结果数目限制，不传值则为100             |

##### 请求示例URL

<https://api.basefex.com/accounts/transactions?type=DEPOSIT&limit=30>

##### 返回示例

``` js
[
  {
    "address": "2N2BUEAqDH1mhYe2tVy1tRPda9jY4KniyjB",                                  // 充值地址
    "foreignTxId": "a862ade7c3a642968b1f1e1412701fd3c854397fa2706b57a9ce1838d3bfddf7", // 外部交易id
    "userId": "5aec525e-335d-4724-0005-20153b361f89",                                  // 
    "status": "NEW",                                                                   // 状态（ NEW,AUDITED PENDING COMPLETED CANCELED REJECTED ）
    "id": "5aeeeba7-2843-f607-0005-2fdaac7eda8f",                                      // 
    "subtype": null,                                                                   // 
    "amount": 0.0653564,                                                               // 充值或者提现金额
    "balances": null,                                                                  // 
    "ts": 1562221911201,                                                               // 时间
    "type": "DEPOSIT",                                                                 // 充值（或者提现）
    "readableId": null,                                                                // 
    "audit": null,                                                                     // 
    "fee": 0,                                                                          // 费用
    "note": null,                                                                      // 
    "currency": "BTC"                                                                  // 结算类型
  }
]
```

### 获取充值及提现记录数量

##### URL

<https://api.basefex.com/accounts/transactions/count>

##### HTTP请求方式

> GET

##### 请求参数

| 参数   | 必选 | 类型     | 说明                              |
| ---- | -- | ------ | ------------------------------- |
| type |    | string | 交易类型，包括充值`DEPOSIT`和提现`WITHDRAW` |

##### 请求示例URL

<https://api.basefex.com/accounts/transactions/count?type=DEPOSIT>

##### 返回示例

``` js
{
  "count": 1
}
```

## <span id="open-api-misc"> 其他(Misc) </span>

### 获取合约价格

获取平台所有合约的价格

##### URL

<https://api.basefex.com/instruments/prices>

##### HTTP请求方式

> GET

##### 请求参数

无

##### 请求示例URL

<https://api.basefex.com/instruments/prices>

##### 返回示例

> 说明：目前价格单位只有`BTCUSD`为美元，其他合约为比特币。

``` js
{
    "BTCUSD": [
    {
      "time": 1562223600000,  // 时间戳（距离1970年1月1日的毫秒数）
      "price": 11290          // 合约价格（单位：美元）
    },
    {
      "time": 1562222700000,
      "price": 11290
    },
    {
      "time": 1562221800000,
      "price": 11290
    }
  ],
  "ETHXBT": [
    {
      "time": 1562223600000,   // 时间戳
      "price": 0.023           // 价格 （单位：BTC）
    },
    {
      "time": 1562222700000,
      "price": 0.023
    },
    {
      "time": 1562221800000,
      "price": 0.023
    }
  ],
  "HTXBT": [],
  ...
}
```

### 获取买卖盘实时行情

##### URL

[https://api.basefex.com/depth@{symbol}/snapshot](https://api.basefex.com/depth@%7Bsymbol%7D/snapshot)

##### HTTP请求方式

> GET

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例URL

<https://api.basefex.com/depth@BTCUSD/snapshot>

##### 返回示例

``` js
{
    "to": 90289544,
    "bestPrices": {
        "ask": 11778.5,
        "bid": 11776
    },
    "lastPrice": 11777.0,
    "bids": {
        "11770.5": 783,
        "11776": 1943,
        "6500": 300,
        "11766": 5148
    },
    "asks": {
        "11790": 28285,
        "11778.5": 3333,
        "12350": 1000
    },
    "from": 0
}
```

### 获取合约详情列表

##### URL

<https://api.basefex.com/instruments>

##### HTTP请求方式

> GET

##### 请求参数

无

##### 请求示例URL

<https://api.basefex.com/instruments>

##### 返回示例

``` js
[
  {
    "turnover24h": 2606.3164679306547,    // 前24小时交易额（单位BTC）
    "openInterest": 3009639,              // 未平仓合约数量
    "volume24hInUsd": 29738395,           // 24小时交易量（美元计）
    "fundingRate": 0,                     // 资金费率
    "volume24h": 29738395,                // 24小时交易量
    "last24hMaxPrice": 12063,             // 24小时内最大成交价
    "btcPrice": 11767.48,                 // 现货价格（美元计）
    "latestPrice": 11769.5,               // 最新成交价
    "symbol": "BTCUSD",                  // 合约类型
    "openValue": 255.98924199050046,     // 未平仓合约总价值（BTC计）
    "last24hMinPrice": 10950.5,          // 前24小时最低价
    "openTime": 0,                       // 
    "markPrice": 11767.48,               // 标记价格
    "indexPrice": 11767.48               // 指数价格
  },
  {
    "turnover24h": 1042.63566,
    "openInterest": 3919,
    "volume24hInUsd": 12269194.276336798,
    "fundingRate": 0.0001,
    "volume24h": 40270,
    "last24hMaxPrice": 0.02626,
    "btcPrice": 11767.48,
    "latestPrice": 0.02524,
    "symbol": "ETHXBT",                  // 合约类型
    "openValue": 98.61654386057411,
    "last24hMinPrice": 0.02523,
    "openTime": 0,
    "markPrice": 0.0252588,
    "indexPrice": 0.02525804
  },
```

## <span id="open-api-ws"> Websocket推送接口 </span>

### 获取行情

获取某个合约的实时详情

##### URL

[wss://ws.basefex.com/instruments@{symbol}](wss://ws.basefex.com/instruments@%7Bsymbol%7D)

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例（使用wscat）

> wscat -c <wss://ws.basefex.com/instruments@BTCUSD>

##### 返回示例

``` js
[
    {
        "turnover24h": 0.354295837,       // 24小时交易额（单位BTC）
        "openInterest": 3412,             // 未平仓合约数量
        "volume24hInUsd": 4000,           // 24小时交易量（美元计）
        "fundingRate": 0,                 // 资金费率
        "volume24h": 4000,                // 24小时交易量
        "last24hMaxPrice": 11290,         // 24小时内最大成交价
        "btcPrice": 11255.51,             // 现货价格（美元计）
        "latestPrice": 11290,             // 最新成交价
        "symbol": "BTCUSD",               // 合约类型
        "openValue": 0.300825969916003,   // 未平仓合约总价值（BTC计）
        "last24hMinPrice": 11290,         // 前24小时最低价
        "openTime": 0,                    // 
        "markPrice": 11255.51,            // 标记价格
        "indexPrice": 11255.51            // 指数价格
    }
]
```

### 获取买卖盘信息

获取某一种合约的买卖盘信息，分为两部分，首先推送买卖盘当前状态的快照，后续推送买卖盘的实时变动。

##### URL

[wss://ws.basefex.com/depth@{symbol}](wss://ws.basefex.com/depth@%7Bsymbol%7D)

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例（使用wscat）

> wscat -c <wss://ws.basefex.com/depth@BTCUSD>

##### 返回示例

快照推送示例

``` js
{
    "to": 91277134,
    "bestPrices": {
        "ask": 11202.5,     // 卖盘最低价
        "bid": 11192.5      // 买盘最高价
    },
    "lastPrice": 11183.5,
    "bids": {
        "11192": 35,    
        "11184": 742,
        "11192.5": 389,     // 买盘最高价及订单数量
        "11188": 293
    },
    "asks": {
        "11202.5": 1806,    // 卖盘最低价
        "11212.5": 2138,
        "11211.5": 19308
    },
    "from": 0
}
```

后续推送示例

``` js
{
    "bids": {                   
        "11183": 3500
    },
    "lastPrice": 11183.5,
    "from": 91277135,
    "bestPrices": {         // 最优价
        "ask": 11202.5,
        "bid": 11192.5
    },
    "asks": {},
    "to": 91277135
}
```

### 获取K线数据

根据时间粒度获取某一种合约的K线

##### URL

[wss://ws.basefex.com/candlesticks/{type}@{symbol}](wss://ws.basefex.com/candlesticks/%7Btype%7D@%7Bsymbol%7D)

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| type   | :white\_check\_mark: | string | 时间周期                                                                                                                                   |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例（使用wscat）

> wscat -c <wss://ws.basefex.com/candlesticks/1MIN@BTCUSD>

##### 返回示例

``` js
[
    {
        "symbol": "BTCUSD",           // 合约类型
        "type": "1MIN",               // 时间周期
        "time": 1562301000000,        // 当期开始时间
        "open": 11119.5,              // 开始价
        "close": 11109.5,             // 结束价
        "high": 11123.5,              // 当期最高价
        "low": 11109.5,               // 当期最低价
        "nTrades": 4,                 // 交易数量
        "volume": 3427,               // 交易量（单位美元）
        "turnover": 0.30820388075,    // 交易额（单位BTC）
        "version": 91383701,          // 
        "nextTs": 1562301060000       // 下一周期
    }
]
```

### 获取交易记录

实时获取平台的交易记录，首先推送当前

##### URL

[wss://ws.basefex.com/trades@{symbol}](wss://ws.basefex.com/trades@%7Bsymbol%7D)

##### 请求参数

| 参数     | 必选                   | 类型     | 说明                                                                                                                                     |
| ------ | -------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| symbol | :white\_check\_mark: | string | 合约类型，包括`BTCUSD`, `ETHXBT`, `XRPXBT`, `BCHXBT`, `LTCXBT`, `EOSXBT`, `ADAXBT`, `TRXXBT`, `BNBXBT`, `HTXBT`, `OKBXBT`, `GTXBT`, `ATOMXBT` |

##### 请求示例（使用wscat）

> wscat -c <wss://ws.basefex.com/trades@BTCUSD>

##### 返回示例

快照推送示例

``` js
[
    {
        "id": "5aefeab9-6840-0000-0001-0000056fe3c8", // 交易id
        "symbol": "BTCUSD",                           // 合约类型
        "price": 11178.5,                             // 
        "size": 100,                                  // 合约数量
        "matchedAt": 1562288776609,                   // 成交时间
        "side": "BUY"                                 // 买卖方向
    },
    {
        "id": "5aefeb33-e940-0000-0001-0000056fea51",
        "symbol": "BTCUSD",
        "price": 11180,
        "size": 60,
        "matchedAt": 1562288902053,
        "side": "SELL"
    },
    ...
]
```

后续推送示例

``` js
[
    {
        "id": "5af01b50-9700-0000-0001-000005727d9b",
        "symbol": "BTCUSD",
        "price": 11138,
        "size": 267,
        "matchedAt": 1562301514332,
        "side": "BUY"
    }
]
```
