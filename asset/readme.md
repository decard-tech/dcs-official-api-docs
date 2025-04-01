### 1. 资产查询

**描述：** 查询用户资产

- **URL:** `/user-asset/v1/balance`
- **方法:** `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | string | Y    | DeCard用户ID |

- **响应参数:**

| 名称      | 类型     | 描述       |
|---------|--------|----------|
| asset   | string | 资产币种     |
| free    | string | 可用数量     |
| freeze  | string | 冻结数量     |
| total   | string | 总数量      |
| network | string | 网络       |
| logo    | string | logo url |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "asset": "USDT",
      "logo": "https://static-dev.thedecard.com/20240813/1adaec74-05ec-40fc-930c-24932b2ca042.svg",
      "network": "TRON",
      "free": "134.18397879",
      "freeze": "0",
      "total": "134.18397879"
    },
    {
      "asset": "USDC",
      "logo": "https://static-dev.thedecard.com/20240813/30a1f898-f023-4f38-bbdf-5caf2c71f8f3.svg",
      "network": "TRON",
      "free": "1118.37430217",
      "freeze": "0",
      "total": "1118.37430217"
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
<a id="transactions"></a>

### 2. 资金历史查询

**描述：**  查询用户资产历史

- **URL:** `/user-asset/v1/transactions`
- **方法:** `POST`
- **请求参数:**

| 名称             | 类型            | 是否必须 | 描述                                                                                                                                                                                                                                                              |
|----------------|---------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| externalUserId | string        | Y    | DeCard用户ID                                                                                                                                                                                                                                                      |
| type           | string        | N    | 交易类型 <br>- `DEPOSIT`：充值  <br>- `WITHDRAW`：提现 <br>- `CARD_PRINTING_FEE`：制卡费  <br>- `CARD_POSTAL_FEE`：邮寄费  <br>- `CARD_VIP_FROZEN_FEE`：VIP冻结费 <br>- `CONVERSION`：convert费  <br>- `REWARD_DISTRIBUTION`：奖励金发放 <br>- `REWARD_PAY`：奖励金支付 <br>- `REWARD_REFUND`：奖励金退还 |
| page           | int           | N    | 默认1                                                                                                                                                                                                                                                             |
| size           | int           | N    | 默认100； 每页条数[1,100]                                                                                                                                                                                                                                              |
| startTime      | LocalDateTime | N    | 开始时间戳;  默认MIN                                                                                                                                                                                                                                                   |
| endTime        | LocalDateTime | N    | 截止时间戳;  默认Now()                                                                                                                                                                                                                                                 |

- **响应参数:**

| 名称         | 类型     | 描述     |
|------------|--------|--------|
| tranId     | string | 交易单号   |
| externalId | string | 外部交易ID |
| type       | string | 交易类型   |
| freeDelta  | string | 交易数量   |
| time       | string | 交易时间   |
| asset      | string | 交易币种   |
| id         | string | 唯一id   |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "tranId": "1041201345182711808",
      "externalId": "1041201333329608704",
      "type": "CONVERSION",
      "freeDelta": "-3.66208981",
      "time": "1726200144966",
      "asset": "USDT"
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

### 3. 查询资产变动详情

**描述：**  查询用户资产变动详情

- **URL:** `/user-asset/v1/transaction-detail`
- **方法:** `POST`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述                                                                                                                                                                                                                                                                  |
|----------------|--------|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| externalUserId | string | Y    | DeCard用户ID                                                                                                                                                                                                                                                          |
| tranId         | long   | Y    | 交易单号                                                                                                                                                                                                                                                                |
| externalTranId | string | Y    | 外部交易ID                                                                                                                                                                                                                                                              |
| type           | string | Y    | 交易类型  <br>- `DEPOSIT`：充值  <br>- `WITHDRAW`：提现 <br>- `CARD_PRINTING_FEE`：制卡费  <br>- `CARD_POSTAL_FEE`：邮寄费  <br>- `CARD_VIP_FROZEN_FEE`：VIP冻结费  <br>- `CONVERSION`：convert费   <br>- `REWARD_REFUND`：奖励金退还  <br>- `REWARD_DISTRIBUTION`：奖励金退还 <br>- `REWARD_PAY`：奖励金退还 |

- **响应参数:**

| 名称        | 类型     | 描述                                                                                                                                                                                                                                                              |
|-----------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type      | string | 交易类型 <br>- `DEPOSIT`：充值  <br>- `WITHDRAW`：提现 <br>- `CARD_PRINTING_FEE`：制卡费  <br>- `CARD_POSTAL_FEE`：邮寄费  <br>- `CARD_VIP_FROZEN_FEE`：VIP冻结费  <br>- `CONVERSION`：convert费 <br>- `REWARD_DISTRIBUTION`：奖励金发放 <br>- `REWARD_PAY`：奖励金支付 <br>- `REWARD_REFUND`：奖励金退还 |
| time      | string | 交易时间                                                                                                                                                                                                                                                            |
| freeDelta | string | 资产变动数量                                                                                                                                                                                                                                                          |
| asset     | string | 资产                                                                                                                                                                                                                                                              |
| logo      | string | 资产logo                                                                                                                                                                                                                                                          |
| orderId   | string | 单号                                                                                                                                                                                                                                                              |
| status    | string | 状态 [SUCCESS, FAIL, PENDING]                                                                                                                                                                                                                                     |

以下部分根据交易类型追加返回

**CONVERSION**

| 名称         | 类型     | 描述   |
|------------|--------|------|
| baseAsset  | string | 基础资产 |
| quoteAsset | string | 目标资产 |
| price      | string | 报价   |

**DEPOSIT**

| 名称                 | 类型     | 描述    |
|--------------------|--------|-------|
| network            | string | 网络    |
| networkName        | string | 网络名称  |
| networkNativeAsset | string | 原生代币  |
| transactionHash    | string | 交易哈希  |
| networkTxUrl       | string | 地址URL |
| sendAddress        | string | 地址    |

**WITHDRAW**

| 名称                 | 类型     | 描述    |
|--------------------|--------|-------|
| network            | string | 网络    |
| networkName        | string | 网络名称  |
| networkNativeAsset | string | 原生代币  |
| transactionHash    | string | 交易哈希  |
| networkTxUrl       | string | 地址URL |
| actualReceived     | string | 接收数量  |
| feeAmount          | string | 费用金额  |
| receivedAddress    | string | 接收地址  |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "type": "CONVERSION",
    "time": "1726216153194",
    "freeDelta": "5.50768677",
    "asset": "USDT",
    "orderId": "1041268486317686785",
    "status": "SUCCESS",
    "baseAsset": "USDT",
    "quoteAsset": "USD",
    "price": "1.00042",
    "network": "TRON",
    "networkName": "TRON",
    "transactionHash": "4edc1eb2d9d6b6a153*****5f1b57049fa1cb34126",
    "networkTxUrl": "https://tronscan.org/#/transaction/",
    "sendAddress": "TErLvDQXaDciLR8RPH45BfM3z7Hxua4sVt",
    "network": "TRON",
    "networkName": "TRON",
    "transactionHash": "4edc1eb2d9d6b6a153*****5f1b57049fa1cb34126",
    "networkTxUrl": "https://tronscan.org/#/transaction/",
    "actualReceived": "990",
    "feeAmount": "10",
    "receivedAddress": "TErLvDQXaDciLR8RPH45BfM3z7Hxua4sVt"
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

---

### 4. 资金池充值

**描述：** 从渠道系统账户划转资金给当前渠道用户

- **URL:** `/user-asset/v1/credit`
- **方法:** `POST`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述                                        |
|----------------|--------|------|-------------------------------------------|
| externalTranId | string | Y    | 唯一id，当前api用户维度全局唯一，如果重复，拒绝执行。长度限制为10-20位。 |
| asset          | string | Y    | 币种，必须是decard支持的币种                         |
| amount         | string | Y    | 充值金额，必须是正数，小数位数不得超过8位，整数部分不得超过10位         |
| externalUserId | string | Y    | DeCard用户ID                                |
| remark         | string | N    | 备注信息                                      |

- **响应参数:**

| 名称             | 类型     | 描述                             |
|----------------|--------|--------------------------------|
| externalTranId | string | externalTranId                 |
| asset          | string | 币种                             |
| amount         | string | 金额                             |
| externalUserId | string | 外部user id                      |
| remark         | string | 备注                             |
| status         | string | 状态 SUCCESS/FAIL 失败会通过接口报错的方式返回 |
| direction      | string | 方向 CREDIT/DEBIT                |
| tranId         | string | tranId                         |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "externalTranId": "1111111116",
    "asset": "USDT",
    "amount": "10",
    "externalUserId": "2dc7cfae-6a2d-4609-b302-3cd412b9a9e6",
    "direction": "CREDIT",
    "remark": "hello world!!",
    "status": "SUCCESS",
    "tranId": "4258308122102202624"
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

---

### 5. 资金池提现

**描述：** 从当前渠道用户划转资金给渠道系统账户

- **URL:** `/user-asset/v1/debit`
- **方法:** `POST`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述                                        |
|----------------|--------|------|-------------------------------------------|
| externalTranId | string | Y    | 唯一id,当前api用户维度全局唯一，如果重复，拒绝执行。长度限制为10-20位。 |
| asset          | string | Y    | 币种，必须是decard支持的币种                         |
| amount         | string | Y    | 充值金额，必须是正数，小数位数不得超过8位，整数部分不得超过10位         |
| externalUserId | string | Y    | DeCard用户ID                                |
| remark         | string | N    | 备注信息                                      |

- **响应参数:**

| 名称             | 类型     | 描述              |
|----------------|--------|-----------------|
| externalTranId | string | uniqueId        |
| asset          | string | 币种              |
| amount         | string | 金额              |
| externalUserId | string | 外部user id       |
| remark         | string | 备注              |
| direction      | string | 方向 CREDIT/DEBIT |
| tranId         | string | tranId          |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "externalTranId": "1111111116",
    "asset": "USDT",
    "amount": "10",
    "externalUserId": "2dc7cfae-6a2d-4609-b302-3cd412b9a9e6",
    "direction": "CREDIT",
    "remark": "hello world!!",
    "status": "SUCCESS",
    "tranId": "4258308122102202624"
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

---

### 6. 资金池操作查询

**描述：** 资金划转结果查询

- **URL:** `/user-asset/v1/transfer-query`
- **方法:** `GET`
- **请求参数:**

| 名称              | 类型       | 是否必须 | 描述    |
|-----------------|----------|------|-------|
| externalTranIds | string[] | Y    | 唯一id  |
| page            | int      | Y    | 默认1   |
| rows            | int      | Y    | 默认100 |

- **响应参数:**

| 名称             | 类型     | 描述              |
|----------------|--------|-----------------|
| externalTranId | string | Y               | 唯一id,当前api用户维度全局唯一，如果重复，拒绝执行     |
| asset          | string | Y               | 币种，必须是decard支持的币种 |
| amount         | string | Y               | 转账金额，必须是正数，小数位数不得超过8位 |
| externalUserId | string | Y               | DeCard用户ID |
| remark         | string | N               | 备注信息 |
| direction      | string | 方向 CREDIT/DEBIT |
| tranId         | string | tranId          |
| status         | string | 状态 SUCCESS/FAIL |

**响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "externalTranId": "1111111116",
      "asset": "USDT",
      "amount": "10.000000000000000000",
      "externalUserId": "2dc7cfae-6a2d-4609-b302-3cd412b9a9e6",
      "direction": "CREDIT",
      "remark": "hello world!!",
      "status": "SUCCESS",
      "tranId": "4258308122102202624"
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

### 7.查询法币入金记录

**描述：** 查询入账记录

- **URL:** `/fiat/v1/transactions`
- **方法:** `GET`
- **请求参数:**

| 名称           | 类型   | 是否必须 | 描述             |
| -------------- | ------ | -------- | ---------------- |
| externalUserId | String | Y        | decard用户id     |
| page           | int    | N        | 页码 默认值 1    |
| rows           | int    | N        | 每页条数 默认 20 |

- **响应**：

| 名称                | 类型          | 描述                       |
| ------------------- | ------------- | -------------------------- |
| debitCreditIndcator | String        | 借贷方向 C：贷记   D：借记 |
| postingTransType    | String        | 交易类型                   |
| transactionAmount   | BigDecimal    | 交易金额                   |
| transactionCurrency | String        | 交易币种                   |
| merchantName        | String        | 商户名称                   |
| transactionDateTime | LocalDateTime | 交易时间                   |
| postedTransactionId | String        | 入账流水id                 |

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
      "transactionAmount": 10.00,
      "transactionCurrency": "702",
      "merchantName": "TOM",
      "transactionDateTime": "2025-04-01T10:30:00",
      "postedTransactionId": "123455325325353152151"
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



### 8.用户资产查询

**描述：** 查询用户资产

- **URL:** `/fiat/v1/balance`
- **方法:** `GET`
- **请求参数:**

| 名称           | 类型   | 是否必须 | 描述         |
| -------------- | ------ | -------- | ------------ |
| externalUserId | string | Y        | DeCard用户ID |

- **响应参数:**

| 名称          | 类型    | 描述     |
| ------------- | ------- | -------- |
| asset         | string  | 资产币种 |
| free          | string  | 可用数量 |
| freeze        | string  | 冻结数量 |
| total         | string  | 总数量   |
| network       | string  | 网络     |
| logo          | string  | logo url |
| topUpStatus   | Boolean | 充值开关 |
| totalUsd      | String  | usd总额  |
| logoUrl       | String  |          |
| limitOverFlag | Boolean | 限额标识 |

**响应示例:**

```json
{
	"code": "SYS_SUCCESS",
	"message": null,
	"messageDetail": null,
	"data": {
		"freeUsd": "805.05",
		"freezeUsd": "0",
		"totalUsd": "805.05",
		"userAssets": [
			{
				"asset": "SGD",
				"free": "287.88",
				"freeze": "0",
				"freeUsd": "211.64",
				"freezeUsd": "0",
				"assetName": "Singapore Dollar",
				"assetLogo": "https://static.thedecard.com/images/dvault/SGD.png?w=48&q=75",
				"total": "287.88",
				"totalUsd": "211.64",
				"network": "",
				"fiat": 1
			}
		],
		"topUpStatus": true,
		"reward": {
			"totalUsd": "5",
			"logoUrl": "https://static.sitdcd.com/images/logo/Reward.svg"
		},
		"limitOverFlag": false
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



