### 创建监听频道
**描述：** 获取渠道的私有频道, 用于监听当前渠道用户的数据变化

- **URL:** `/websocket/v1/get-channel`
- **方法:**  `GET`
- **请求参数:** 无

- **响应:**

| 名称      | 类型      | 描述      |
|---------|---------|---------|
| channel | string  | 当前渠道的私有频道 |



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

| Mainnet | stream.thedecard.com      |
| ------- |---------------------------|
| Testnet | stream-test.thedecard.com |

**响应数据结构:**

| 名称      | 类型     | 描述      |
|---------|--------|---------|
| type | string | 响应类型 |
| timestamp | string | 时间戳 |
| externalUserId | string | DeCard用户ID|
| data | object | 具体内容 |


#### KYC状态 (KYC_STATUS)

| 名称     | 类型     | 描述      |
|--------|--------|---------|
| status | string | KYC状态 <br>- `UNDO`：未做KYC <br>- `INIT`：初始状态 <br>- `PENDING`：申请已经成功提交完成，正在审核中。<br />该状态有几种可能：<br />1）等待自动审核： 一般 3 分钟内通过，如果没有则进入人工审核。<br />2）等待人工审核： 人工审核包括 Operation team 通过邮件联系用户提供更多资料。 <br>- `PASS`：审核通过 <br>- `REFUSE`：拒绝  |


**响应示例:**
```json
{
    "type": "KYC_STATUS",
    "timestamp": 1731471194021,
    "externalUserId": "f1b718e0",
    "data": {
      "status": "PASS"
    }
}
```


#### 数字资产变动 (BALANCE_CHANGE)

| 名称     | 类型     | 描述  |
|--------|--------|-----|
| tranId | string | 流水单号 |
| asset | string | 资产币种 |
| freeDelta | string | 资产变动数量 |
| freezeDelta | string | 资产冻结变动数量 |
| free | string | 可用持仓 |
| freeze | string | 冻结资产 |

**响应示例:**
```json
{
  "type": "BALANCE_CHANGE",
  "timestamp": 1731471194021,
  "externalUserId": "f1b718e0",
  "data": {
    "tranId": "1041268486317686785",
    "asset": "USDT",
    "freeDelta": "-1",
    "freezeDelta": "1",
    "free": "1000",
    "freeze": "500"
  }
}
```



#### 卡交易记录 (CARD_TRANSACTION)

| 名称     | 类型     | 描述    |
|--------|--------|-------|
| cardNumber | string | 卡号后4位 |
| transactionCurrencyCode | string | 交易币种ISO代码   |
| transactionAmount | string | 交易金额   |
| localTransactionDate | string | 交易日期   |
| localTransactionTime | string | 交易时间   |
| response | string | 交易结果 (A-accept/success，D-deny/fail)   |
| direction | string | 交易方向 (C-资金增加/退款，D-资金扣减/消费)  |


**响应示例:**
```json
{
  "type": "CARD_TRANSACTION",
  "timestamp": 1731471194021,
  "externalUserId": "f1b718e0",
  "data": {
    "cardNumber": "***1234",
    "transactionCurrencyCode": "702",
    "transactionAmount": "123.45",
    "localTransactionDate": "1113",
    "localTransactionTime": "154542",
    "response": "A",
    "direction": "C"
  }
}
```

