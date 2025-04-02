### 创建监听频道

**描述：** 获取渠道的私有频道, 用于监听当前渠道用户的数据变化

- **URL:** `/websocket/v1/get-channel`
- **方法:**  `GET`
- **请求参数:** 无

- **响应:**

| 名称      | 类型     | 描述        |
|---------|--------|-----------|
| channel | string | 当前渠道的私有频道 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": "HBHmMK99SJEVMVUqCb4",
  "success": true
}
```

* 为确保私有频道数据的安全性，每个私有频道的存活时间为24小时，监听方应在24小时内重新获取并创建监听；
* 获取新的频道后旧的频道将不再推送数据；

---

### 监听频道

**描述：** 根据获取的频道，监听当前渠道用户的数据变化

**URL:** `wss://{domain}/ws/{channel}`

| Mainnet | stream.thedecard.com |
|---------|----------------------|
| Testnet | stream.uatdcd.com    |

**响应数据结构:**

| 名称             | 类型     | 描述         |
|----------------|--------|------------|
| type           | string | 响应类型       |
| timestamp      | string | 时间戳        |
| externalUserId | string | DeCard用户ID |
| data           | object | 具体内容       |

#### KYC状态 (KYC_STATUS)

| 名称     | 类型     | 描述                                                                                                                                                                                                                          |
|--------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status | string | KYC状态 <br>- `UNDO`：未做KYC <br>- `INIT`：初始状态 <br>- `PENDING`：申请已经成功提交完成，正在审核中。<br />该状态有几种可能：<br />1）等待自动审核： 一般 3 分钟内通过，如果没有则进入人工审核。<br />2）等待人工审核： 人工审核包括 Operation team 通过邮件联系用户提供更多资料。 <br>- `PASS`：审核通过 <br>- `REFUSE`：拒绝 |

**响应示例:**

```json
{
  "type": "KYC_STATUS",
  "timestamp": "1731471194021",
  "version": "1",
  "externalUserId": "f1b718e0",
  "data": {
    "status": "PASS"
  }
}
```

#### 数字资产变动 (BALANCE_CHANGE)

| 名称             | 类型     | 描述                                                          |
|----------------|--------|-------------------------------------------------------------|
| tranId         | string | 流水单号                                                        |
| externalTranId | string | 外部交易ID                                                      |
| asset          | string | 资产币种                                                        |
| network        | string | 网络                                                          |
| freeDelta      | string | 资产变动数量                                                      |
| freezeDelta    | string | 资产冻结变动数量                                                    |
| free           | string | 可用持仓                                                        |
| freeze         | string | 冻结资产                                                        |
| type           | string | 变动类型,与[transactions](../asset/readme.md#transactions)接口定义一致 |

**响应示例:**

```json
{
  "type": "BALANCE_CHANGE",
  "timestamp": "1733980483134",
  "version": "2",
  "externalUserId": "9f8b5f82",
  "data": {
    "asset": "USD",
    "network": "",
    "freeDelta": "12.64",
    "freezeDelta": "0",
    "tranId": "4285261023005170688",
    "externalTranId": "4285261022921284608",
    "free": "12.64",
    "freeze": "0",
    "type": ""
  }
}
```

#### 卡交易记录 (CARD_TRANSACTION)

| 名称                      | 类型     | 描述                                  |
|-------------------------|--------|-------------------------------------|
| cardNumber              | string | 卡号后4位                               |
| transactionCurrencyCode | string | 交易币种ISO代码                           |
| transactionAmount       | string | 交易金额                                |
| localTransactionDate    | string | 交易日期                                |
| localTransactionTime    | string | 交易时间                                |
| response                | string | 交易结果 (A-accept/success，D-deny/fail) |
| direction               | string | 交易方向 (CREDIT-资金增加/退款，DEBIT-资金扣减/消费) |
| externalTranId          | string | 外部交易ID                              |
| systemTraceAuditNumber  | string | 系统跟踪号                               |
| requestAmountInUsd      | string | 美元交易金额                              |

**响应示例:**

```json
{
  "type": "CARD_TRANSACTION",
  "timestamp": "1733981368030",
  "version": "3",
  "externalUserId": "9f8b5f82-610d-46e5-b006-3db6b2c8a395",
  "data": {
    "cardNumber": "**** **** **** 0617",
    "transactionCurrencyCode": "702",
    "transactionAmount": "20.2",
    "localTransactionDate": "1211",
    "localTransactionTime": "161420",
    "response": "D",
    "direction": "DEBIT",
    "systemTraceAuditNumber": "G_4295180843619725568",
    "requestAmountInUsd": "14.86",
    "externalTranId": "4285261022921284608"
  }
}
```

* 为确保私有频道数据的安全性，每个私有频道的存活时间为24小时，监听方应在24小时内重新获取并创建监听；
* 获取新的频道后旧的频道将不再推送数据；

---

### 消息查询

**描述：** 根据消息 `version` 查询消息内容

- **URL:** `/websocket/v1/search`
- **方法:**  `GET`
- **请求参数:** 无

- **响应:**

| 名称      | 类型   | 描述   |
|---------|------|------|
| version | Long | 消息版本 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": "{\"data\": {\"status\": \"PASS\"}, \"type\": \"KYC_STATUS\", \"version\": \"3\", \"timestamp\": \"1736838937744\", \"externalUserId\": \"2074fcdd-a2fe-a46b9b458b34\"}",
  "success": true
}
```

#### 订单状态通知 (ORDER_STATUS)

| 名称               | 类型   | 描述                                                         |
| ------------------ | ------ | ------------------------------------------------------------ |
| txHash             | String | 交易hash值 唯一id                                            |
| transStatus        | String | 交易状态    <br />**PENDING_NORMAL** – 处理中（系统自动审核）。 <br />**PENDING_CREDIT** – 待入账（等待资金到账）。 <br />**PENDING_AUDIT** – 待审核（需人工确认）。 <br />**SUCCESS** – 成功（已完成）。 <br />**FAILED** – 失败（未完成，可能因错误或拒绝） |
| amount             | String | 充值金额                                                     |
| asset              | String |                                                              |
| network            | String |                                                              |
| creditAmount       | String | 上账金额                                                     |
| creditCurrency     | String | SGD                                                          |
| fromAddress        | String |                                                              |
| toaddress          | String |                                                              |
| convertAmount      | String | 转换金额                                                     |
| convertCurrency    | String | 转换金额币种                                                 |
| convertFeeAmount   | String | 转换费用                                                     |
| convertFeeCurrency | String | 转换费用币种                                                 |


**响应示例：**

```
{
  "type": "ORDER_STATUS",
  "timestamp": "1733981368030",
  "version": "1",
  "externalUserId": "9f8b5f82-610d-46e5-b006-3db6b2c8a395",
  "data": {
    "txHash": "0x0d1c3de69760d09e528284c032d7b31bab4e492c0fee3efd69af74cd7a4fe05f",
    "transStatus": "PENDING_CREDIT",
    "amount": "2.59000000",
    "asset": "ETH",
    "network": "Ethereum",
    "creditAmount": "2.59000000",
    "creditCurrency": "SGD",
    "fromAddress": "0xed63647a0875ff48673e10ed632bc0cfd8664b89",
    "toaddress": "0x5caeec3c95d5dd77cfb399f0c73c106ce3b98b60",
    "convertAmount": "2.59000000",
    "convertCurrency": "USD",
    "convertFeeAmount": "2.59000000",
    "convertFeeCurrency":""
  }
}
```

