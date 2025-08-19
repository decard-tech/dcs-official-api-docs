### 1. ~~查看卡申请列表~~

**描述：** 查看可申请的卡列表

- **URL:** `/card/v1/list`
- **方法:**  `GET`
- **请求参数:**  无

- **响应:**

| 名称    | 类型     | 描述                                       |
|-------|--------|------------------------------------------|
| type  | string | 卡类型 <br>- `VIP`：VIP卡  <br>- `NORMAL`：普通卡 |
| name  | string | 卡名称                                      |
| badge | string | 卡徽章                                      |
| image | string | 卡图片链接                                    |
| tags  | list   | 卡功能标签字符串列表                               |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "association": "MASTER_CARD",
      "type": "VIP",
      "name": "高端卡",
      "badge": "VIP",
      "image": "https://xxx.xxx.com/card/apply/vip.webp",
      "tags": [
        "无限额度消费",
        "消费时自动兑换U",
        "专属尊享礼遇"
      ]
    },
    {
      "association": "MASTER_CARD",
      "type": "NORMAL",
      "name": "普通卡",
      "badge": "",
      "image": "https://xxx.xxx.com/card/apply/normal.webp",
      "tags": [
        "轻触支付",
        "畅游无忧",
        "选择心仪卡面，彰显您的个性"
      ]
    }
  ],
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

---

### 2. 卡详情(状态)查询

**描述：** 查看卡详情

- **URL:** `/card/v1/detail`
- **方法:**  `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | string | Y    | DeCard用户ID |
| cardMantissa   | string | N    | 卡号后4位      |

- **响应:**

| 名称                 | 类型     | 描述                                                                                                                                |
|--------------------|--------|-----------------------------------------------------------------------------------------------------------------------------------|
| cardType           | string | 卡类型 <br>- `VIP`：VIP卡  <br>- `NORMAL`：普通卡                                                                                          |
| cardNo             | string | 卡号，只展示后4位                                                                                                                         |
| cardHolder         | string | 持卡人姓名                                                                                                                             |
| cardImage          | string | 卡背景图片                                                                                                                             |
| cardEmbossingName  | string | 卡片刻印名                                                                                                                             |
| cardStatus         | string | 卡片的当前状态：<br>- `NORMAL`：正常<br>- `FROZEN`：冻结<br>- `CANCELLED`：已取消<br>- `INACTIVE`：未激活                                               |
| physicalCardStatus | string | 实体卡状态实体卡片的当前状态：<br>- `UN_APPLY`：未申请<br>- `INACTIVE`：未激活<br>- `ACTIVE`：活跃<br>- `REPLACE`：需更换<br>- `FROZEN`：冻结<br>- `CANCELLED`：已取消 |
| walletBalance      | string | 钱包余额                                                                                                                              |
| caBalance          | string | CA余额                                                                                                                              |
| cardBalance        | string | 卡片可用余额                                                                                                                            |
| points             | int    | 用户积分                                                                                                                              |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "association": "MASTER_CARD",
      "cardType": "NORMAL",
      "cardNo": "************0643",
      "cardHolder": "****HAO",
      "cardImage": "https://xxx.xxx.com/card/main/normal-bg.webp",
      "cardEmbossingName": "",
      "cardStatus": "NORMAL",
      "physicalCardStatus": "ACTIVE",
      "walletBalance": "1.87",
      "caBalance": "0",
      "cardBalance": "1.87",
      "points": 0
    }
  ],
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

---

### 3. 冻卡解冻

**描述：** 卡片冻结解冻

- **URL:** `/card/v1/block`
- **方法:**  `POST`
- **请求参数:**

| 名称             | 类型      | 是否必须 | 描述                     |
|----------------|---------|------|------------------------|
| externalUserId | string  | Y    | DeCard用户ID             |
| block          | boolean | Y    | true: 冻结卡；false: 解冻卡   |
| cardMantissa   | string  | N    | 卡尾号4位<br />多卡区分，默认第一张卡 |
| smsCode        | string  | N    | 短信验证码；**解冻卡需传递**       |

- **响应:**

| 名称   | 类型      | 描述    |
|------|---------|-------|
| data | boolean | 成功或失败 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": true,
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

---

### 4. ~~卡消费历史USD~~

**描述：** 卡消费历史（USD）

转至[卡账单列表](readme.md/#5-卡账单列表)

- **URL:** `/card/v1/transactions`
- **方法:**  `POST`
- **请求参数:**

| 名称             | 类型            | 是否必须 | 描述                                      |
|----------------|---------------|------|-----------------------------------------|
| externalUserId | string        | Y    | DeCard用户ID                              |
| cardMantissa   | string        | N    | 卡尾号4位，多卡区分，默认第一张卡                       |
| page           | int           | N    | 默认1                                     |
| size           | int           | N    | 默认100；每页条数[1,100]                       |
| type           | string        | N    | 交易类型 <br>- `SPEND`：消费 <br>- `REFUND`：退款 |
| startTime      | LocalDateTime | N    | 开始时间戳                                   |
| endTime        | LocalDateTime | N    | 截止时间戳                                   |

- **响应:**

| 名称           | 类型     | 描述                                                                    |
|--------------|--------|-----------------------------------------------------------------------|
| id           | string | 卡交易序列号                                                                |
| status       | string | 卡交易状态 [COMPLETE]                                                      |
| type         | string | 卡交易类型 <br>- `SPEND`：消费 <br>- `REFUND`：退款  <br>- `PARTIAL_REFUND`：部分退款 |
| asset        | string | 卡交易币种                                                                 |
| amount       | string | 交易金额                                                                  |
| refundAmount | string | 退款金额                                                                  |
| merchantName | string | 商户名称                                                                  |
| merchantNo   | string | 商户编号                                                                  |
| time         | string | 交易时间                                                                  |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "total": 1,
    "rows": [
      {
        "id": "586188753138826666912468",
        "status": "COMPLETE",
        "type": "SPEND",
        "asset": "USD",
        "amount": "5.51",
        "refundAmount": "0",
        "merchantName": "FIRST THIRD BANK       ST. LOUIS     MO ",
        "merchantNo": "1726996149737",
        "time": "1726216150289"
      }
    ]
  },
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 5. 卡账单列表

**描述：** 卡账单

- **URL:** `/card/v1/statements`
- **方法:**  `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述                |
|----------------|--------|------|-------------------|
| page           | int    | N    | 默认1               |
| rows           | int    | N    | 默认100；每页条数[1,100] |
| externalUserId | string | Y    | DeCard用户ID        |
| cardMantissa   | string | N    | 卡号后4位             |

- **响应:**

| 名称                   | 类型     | 描述                                                              |
|----------------------|--------|-----------------------------------------------------------------|
| type                 | string | 账单类型 <br>- `NOT_POSTED`：未出账单 <br>- `POSTED`：已出账单                |
| statementDateStart   | string | 账单开始时间                                                          |
| statementDateEnd     | string | 账单结束时间、出账时间                                                     |
| paymentAmountInSgd   | string | 账单金额(SGD)                                                       |
| paymentAmountInUsd   | string | 账单金额(USD)                                                       |
| nonPostedAmountInSgd | string | 未入账金额(SGD)                                                      |
| nonPostedAmountInUsd | string | 未入账金额(USD)                                                      |
| statementId          | string | 账单id                                                            | 
| cardScheme           | string | 卡组        <br>- `MASTERCARD` <br>- `VISA`     <br>- `UNION_PAY` |
| cardOrganizationLogo | string | 卡组logo                                                          |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "type": "NOT_POSTED",
      "statementDateStart": "1801526400000",
      "statementDateEnd": "1803859200000",
      "paymentAmountInSgd": "100",
      "paymentAmountInUsd": "0",
      "nonPostedAmountInSgd": "0",
      "nonPostedAmountInUsd": "0",
      "statementId": "NO_POSTED",
      "cardScheme": "MASTERCARD",
      "cardOrganizationLogo": "https://static.thedecard.com/images/card/schemes/master.webp"
    },
    {
      "type": "POSTED",
      "statementDateStart": "1798848000000",
      "statementDateEnd": "1801440000000",
      "paymentAmountInSgd": "11302.23",
      "paymentAmountInUsd": "0",
      "nonPostedAmountInSgd": null,
      "nonPostedAmountInUsd": null,
      "statementId": "172888972041194860325506",
      "cardScheme": "MASTERCARD",
      "cardOrganizationLogo": "https://static.thedecard.com/images/card/schemes/master.webp"
    }
  ],
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 6. 卡账单详情

**描述：** 卡账单详情

- **URL:** `/card/v1/statements/detail`
- **方法:**  `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述                |
|----------------|--------|------|-------------------|
| page           | int    | N    | 默认1               |
| rows           | int    | N    | 默认100；每页条数[1,100] |
| externalUserId | string | Y    | DeCard用户ID        |
| statementId    | string | Y    | 账单id              |
| cardMantissa   | string | N    | 卡号后4位             |

- **响应:**

| 名称                 | 类型     | 描述                               |
|--------------------|--------|----------------------------------|
| merchantName       | string | 商户名称                             |
| cardOrganizationLogo | string | 卡组(master card、UnionPay)的logo    |
| postIndicator      | int    | 入账标识 <br>- `0`：未入账 <br>- `1`：已入账 |
| transactionDateTime | string | 交易时间                             |
| postingAmountInSgd | string | 入账金额(SGD)                        |
| postingAmountInUsd | string | 入账金额(USD)                        |
| debitCreditIndcator | string | 借贷记标识 <br>- `C`：退款 <br>- `D`：消费  |
| transactionDescription | string | 交易描述                             | 
| cardNumber         | string | 卡号，后四位为未打码数字                     | 
| postingDate        | string | 入账日期                             | 
| transactionAmount  | string | 交易金额                             | 
| transactionCurrency | string | 交易币种                             | 
| externalTranId     | string | external tran id                 |
| postedTransactionId | string | 已入账交易id                          |
| cardScheme         | string |        卡组        <br>- `MASTERCARD` <br>- `VISA`     <br>- `UNION_PAY`                           |
| assetMovements     | Array  | 资产变动明细（仅在相关交易时返回）            |

**assetMovements字段表：**

| 名称           | 类型   | 描述             |
|--------------|------|----------------|
| asset        | String | 资产类型（如USDT、USDC等） |
| amount       | String | 变动金额（负数表示扣款，正数表示退款） |
| movementTime | String | 变动时间（毫秒时间戳） |
| movementType | String | 变动类型：<br>- `DEPOSIT` - 充值<br>- `WITHDRAW` - 提现<br>- `CARD_TRANSACTION` - 卡交易<br>- `CARD_TRANSACTION_REFUND` - 卡交易退款<br>- `CONVERSION` - 兑换<br>- `FEE_COLLECT` - 手续费收取<br>等 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data":   [
  {
    "merchantName": "ACQUIRER NAME            KUALA LUMPUR MY",
    "cardOrganizationLogo": "https://static.thedecard.com/images/card/schemes/visa.webp",
    "postIndicator": 0,
    "transactionDateTime": "1818096263000",
    "postingAmountInSgd": "28.85",
    "postingAmountInUsd": "21.22",
    "debitCreditIndcator": "D",
    "transactionDescription": null,
    "cardNumber": "**** **** **** 0601",
    "postingDate": null,
    "transactionAmount": "22",
    "transactionCurrency": "USD",
    "externalTranId": "4647911004599997441",
    "cardOrganization": "VISA",
    "postedTransactionId": "175499548181925590000009",
    "cardScheme": "VISA",
    "systemTraceAuditNumber": "003768",
    "transactionDateTimeStr": "2027-08-12 18:44:23",
    "assetMovements": [
      {
        "asset": "USDT",
        "amount": "-21.2220717",
        "movementTime": "1754995484440",
        "movementType": "CONVERSION"
      },
      {
        "asset": "USD",
        "amount": "21.22",
        "movementTime": "1754995484453",
        "movementType": "CONVERSION"
      },
      {
        "asset": "USD",
        "amount": "-21.22",
        "movementTime": "1754995484460",
        "movementType": "CONVERSION"
      }
    ]
  }],
  "success": true
}
```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 7. 查询法币变动记录

**描述：** 查询入账记录

- **URL:** `/card/v1/fiat/transactions`
- **方法:** `GET`
- **请求参数:**

| 名称           | 类型   | 是否必须 | 描述             |
| -------------- | ------ | -------- | ---------------- |
| externalUserId | String | Y        | decard用户id     |
| page           | int    | N        | 页码 默认值 1    |
| rows           | int    | N        | 每页条数 默认 20 |

- **响应**：

| 名称                | 类型   | 描述                       |
| ------------------- | ------ | -------------------------- |
| debitCreditIndcator | String | 借贷方向 C：贷记   D：借记 |
| postingTransType    | String | 交易类型                   |
| transactionAmount   | String | 交易金额                   |
| transactionCurrency | String | 交易币种                   |
| merchantName        | String | 商户名称                   |
| transactionDateTime | String | 充值到账时间               |
| postedTransactionId | String | 入账流水id                 |
| transferDetails     | Object | 转账明细对象               |

**transferDetails字段表：**

| 名称                | 类型   | 描述               |
| ------------------- | ------ |------------------|
| channelCode | String | 充值渠道代码：<br>F - FOMO Pay<br>T - D Token<br>6 - APP<br>8 - PayNow<br>9 - DBS VA<br>R - VA转账<br>O - 境外银行<br>W - 提现<br>Y - TripleA |
| txnAmt    | String | 充值金额             |
| txnCcy   | String | 币种               |
| sender | String | 发送地址             |
| receiving        | String | 接收地址             |
| timeStamp | String | 充值发起时间           |
| txHash | String | 表示交易唯一标识           |


**成功响应示例：**

```
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "debitCreditIndcator": "D",
      "postingTransType": "AU007",
      "transactionAmount": "10.00",
      "transactionCurrency": "702",
      "merchantName": "TOM",
      "transactionDateTime": "1747793974890",
      "postedTransactionId": "123455325325353152151"
      "transferDetails":{
      	"channelCode":"icn",
      	"txnAmt":"3.234",
      	"txnCcy":"SGD",
      	"sender":"sfsf",
      	"receiving":"sgsff",
      	"timeStamp":"1747793974890",
      	"txHash":"0x0d1c3de69760d09e528284c032d7b31bab4e492c0fee3efd69af74cd7a4fe05f"
      }
    }
  ],
  "success": true
}
```

**失败响应示例：**

```
 {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 8. 申请虚拟卡

**描述：** 申请虚拟卡

- **URL:** `/card/v1/virtual-card/apply`
- **方法:** `POST`
- **请求参数:**

| 名称             |                       | 类型       | 是否必须 | 描述                                                   |
|----------------|-----------------------|----------|------|------------------------------------------------------|
| externalUserId |                       | String   | Y    | decard用户id                                           |
| categoryId     |                       | String   | Y    | 卡类别ID，联系DCS团队获取                                      |
| applyRef       |                       | String   | Y    | 幂等号                                                  |
| kycInfo        |                       | Object   | Y    | KYC信息                                                |
|                | sumsubShareToken      | String   | Y    | sumsub 令牌                                            |
|                | email                 | String   | Y    | 邮箱                                                   |
|                | poaDocType            | String   | Y    | [文件类型](/card/dictionary.md#地址证明文件类型-poaDocType)      |
|                | poaDocUrlList         | String[] | Y    | 文件URL                                                |
|                | poaDocDate            | String   | Y    | 文件日期  eg:2025-01-01                                  |
|                | addressLine1          | String   | Y    | 地址1                                                  |
|                | addressLine2          | String   | Y    | 地址2                                                  |
|                | state                 | String   | Y    | 州                                                    |
|                | city                  | String   | Y    | 城市                                                   |
|                | postalCode            | String   | Y    | 邮政编码                                                 |
|                | country               | String   | Y    | 国家（ 国家码，2位ISO，eg：CN/US/SG ）                          |
|                | employmentStatus      | String   | Y    | [工作状态](/card/dictionary.md#就业状态-employmentStatus)    |
|                | employerName          | String   | Y    | 就业公司名字,当 employmentStatus 为 "EMPLOYED" 为必填           |
|                | employmentJobIndustry | String   | Y    | [行业](/card/dictionary.md#行业分类-employmentJobIndustry) |
|                | occupation            | String   | Y    | [职业](/card/dictionary.md#职业-occupation)              |
|                | jobSeniority          | String   | Y    | [就业资历](/card/dictionary.md#就业资历-jobSeniority)        |
|                | purposeOfAccount      | String   | Y    | [开户目的](/card/dictionary.md#开户目的-purposeOfAccount)    |
|                | sourceOfFunds         | String   | Y    | [资金来源](/card/dictionary.md#资金来源-sourceOfFunds)       |
|                | sourceOfFundsCountry  | String   | Y    | 资金来源的国家（国家码，2位ISO，eg：CN/US/SG）                       |
|                | sourceOfWealth        | String   | Y    | [财富来源](/card/dictionary.md#财富来源-sourceOfWealth)      |

- **响应**：

| 名称          | 类型      | 描述     |
|-------------|---------|--------|
| applyId     | String  | 申请id   |
| categoryId  | Long    | 卡类别ID  |
| applyRef    | String  | 外部申请编号 |
| status      | String  | 状态     |
| errorCode   | String  | 错误码    |
| errorReason | String  | 错误原因   |
| needEddFile | Boolean | 需要edd  |
| remark      | String  | 备注     |

**成功响应示例：**

```
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": {
        "applyId": "1154469232939896833",
        "categoryId": "10102000",
        "applyRef": "2",
        "status": "PENDING",
        "errorCode": "",
        "errorReason": "",
        "needEddFile": false,
        "remark": ""
    },
    "success": true
}
```

**失败响应示例：**

```
 {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 9. 查询进度---申请虚拟卡

**描述：** 查询进度---申请虚拟卡

- **URL:** `/card/v1/virtual-card/detail`
- **方法:** `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | String | Y    | decard用户id |
| applyId        | String | Y    | 申请id       |
| applyRef       | String | Y    | 幂等号        |

- **响应**：

| 名称          | 类型      | 描述     |
|-------------|---------|--------|
| applyId     | String  | 申请id   |
| categoryId  | Long    | 卡类别ID  |
| applyRef    | String  | 外部申请编号 |
| status      | String  | 状态     |
| errorCode   | String  | 错误码    |
| errorReason | String  | 错误原因   |
| needEddFile | Boolean | 需要edd  |
| remark      | String  | 备注     |

**成功响应示例：**

```
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": {
        "applyId": "1154469232939896833",
        "categoryId": "10102000",
        "applyRef": "2",
        "status": "PENDING",
        "errorCode": "",
        "errorReason": "",
        "needEddFile": false,
        "remark": ""
    },
    "success": true
}
```

**失败响应示例：**

```
 {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```
