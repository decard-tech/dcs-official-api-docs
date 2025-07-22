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
| statementDateStart   | long   | 账单开始时间                                                          |
| statementDateEnd     | long   | 账单结束时间、出账时间                                                     |
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
| postingAmountInSgd | string | 入账金额(USD)                        |
| postingAmountInUsd | string | 入账金额(SGD)                        |
| debitCreditIndcator | string | 借贷记标识 <br>- `C`：退款 <br>- `D`：消费  |
| transactionDescription | string | 交易描述                             | 
| cardNumber         | string | 卡号，后四位为未打码数字                     | 
| postingDate        | string | 入账日期                             | 
| transactionAmount  | string | 交易金额                             | 
| transactionCurrency | string | 交易币种                             | 
| externalTranId     | string | external tran id                 |
| postedTransactionId | string | 已入账交易id                          |
| cardScheme         | string |        卡组        <br>- `MASTERCARD` <br>- `VISA`     <br>- `UNION_PAY`                           |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "merchantName": null,
      "cardOrganizationLogo": "https://static.thedecard.com/images/card/schemes/master.webp",
      "postIndicator": 1,
      "transactionDateTime": "1801526399000",
      "postingAmountInSgd": "114.04",
      "postingAmountInUsd": "0",
      "debitCreditIndcator": "D",
      "transactionDescription": "Revolving Interest Charge",
      "cardNumber": "5304 **** ****1759",
      "postingDate": "1801440000000",
      "transactionAmount": "114.04",
      "transactionCurrency": "SGD",
      "externalTranId": "1112231221",
      "cardScheme": "MASTERCARD"
    },
    {
      "merchantName": null,
      "cardOrganizationLogo": "https://static.thedecard.com/images/card/schemes/master.webp",
      "postIndicator": 1,
      "transactionDateTime": "1801526399000",
      "postingAmountInSgd": "2.55",
      "postingAmountInUsd": "0",
      "debitCreditIndcator": "D",
      "transactionDescription": "Revolving Interest Charge",
      "cardNumber": "5304 **** ****1759",
      "postingDate": "1801440000000",
      "transactionAmount": "2.55",
      "transactionCurrency": "SGD",
      "externalTranId": "1112231221",
      "cardScheme": "MASTERCARD"
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
| channelCode | String | 充值渠道：<br>F ：fomo  |
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



