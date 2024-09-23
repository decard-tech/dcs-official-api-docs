### 1. 资产查询

**描述：** 查询用户资产

- **URL:** `/user-asset/v1/balance`
- **方法:** `GET`
- **请求参数:**

| 名称           | 类型     | 是否必须 | 描述         |
| -------------- |--------| -------- | ------------ |
| externalUserId | string | Y        | DeCard用户ID |

- **响应参数:**

| 名称 | 类型   | 描述   |
| ---- | ------ |------|
| asset | string | 资产币种 |
| free | string | 可用数量 |
| freeze | string | 冻结数量 |
| total | string | 总数量  |
| network | string | 网络   |

**响应示例:**

```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "asset": "USDT", 
                "free": "5.512075", 
                "freeze": "0",
                "total": "5.512075",
                "network": "TRON"
        }, {
                "asset": "USDC",
                "free": "16173335.08009343",
                "freeze": "0",
                "total": "16173335.08009343",
                "network": "BASE"
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
---

### 2. 资金历史查询

**描述：**  查询用户资产历史

- **URL:** `/user-asset/v1/transactions`
- **方法:** `POST`
- **请求参数:**

| 名称             | 类型            | 是否必须 | 描述                                                                                                                                                                       |
|----------------|---------------|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| externalUserId | string        | Y  | DeCard用户ID                                                                                                                                                               |
| type           | string        | N  | 交易类型 <br>- `DEPOSIT`：充值  <br>- `WITHDRAW`：提现 <br>- `CARD_PRINTING_FEE`：制卡费  <br>- `CARD_POSTAL_FEE`：邮寄费  <br>- `CARD_VIP_FROZEN_FEE`：VIP冻结费  <br>- `CONVERSION`：convert费 |
| page           | int           | N   | 默认1                                                                                                                                                                      |
| size           | int       | N    | 默认100； 每页条数[1,100]                                                                                                                                                       |
| startTime      | LocalDateTime | N   | 开始时间戳;  默认Now()-7D                                                                                                                                                       |
| endTime        | LocalDateTime | N   | 截止时间戳;  默认Now()                                                                                                                                                          |

- **响应参数:**

| 名称       | 类型   | 描述       |
| ---------- | ------ | ---------- |
| tranId     | string | 交易单号   |
| externalId | string | 外部交易ID |
| type       | string | 交易类型   |
| freeDelta  | string | 交易数量   |
| time       | string | 交易时间   |
| asset      | string | 交易币种   |

**响应示例:**

```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "tranId": "1041201345182711808",
                "externalId": "1041201333329608704",
                "type": "CONVERSION",
                "freeDelta": "-3.66208981",
                "time": "1726200144966",
                "asset": "USDT"
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
---

### 3. 查询资产变动详情

**描述：**  查询用户资产变动详情

- **URL:** `/user-asset/v1/transaction-detail`
- **方法:** `POST`
- **请求参数:**

| 名称            | 类型     | 是否必须 | 描述                                                                                         |
|---------------|--------|------|--------------------------------------------------------------------------------------------|
| externalUserId | string | Y  | DeCard用户ID                                                                                 |
| tranId        | long   | Y  | 交易单号                                                                                          |
| externalTranId| string | Y  | 外部交易ID                                                                                          |
| type          | string | Y  | 交易类型<br>- `DEPOSIT`：充值  <br>- `WITHDRAW`：提现 <br>- `CARD_PRINTING_FEE`：制卡费  <br>- `CARD_POSTAL_FEE`：邮寄费  <br>- `CARD_VIP_FROZEN_FEE`：VIP冻结费  <br>- `CONVERSION`：convert费 |


- **响应参数:**

| 名称       | 类型   | 描述       |
| ---------- | ------ | ---------- |
| type | string | 交易类型 |
| time | string | 交易时间 |
| freeDelta | string | 资产变动数量 |
| asset | string | 资产 |
| orderId | string | 单号 |
| status | string | 状态 [SUCCESS, FAIL, PENDING] |

以下部分根据交易类型追加返回

**CONVERSION**

| 名称       | 类型   | 描述       |
| ---------- | ------ | ---------- |
| baseAsset | string | 基础资产 |
| quoteAsset | string | 目标资产 |
| price | string | 报价 |

**DEPOSIT**

| 名称            | 类型   | 描述     |
| --------------- | ------ | -------- |
| network         | string | 网络 |
| networkName     | string | 网络名称 |
| transactionHash | string | 交易哈希 |
| networkTxUrl | string | 地址URL |
| sendAddress | string | 地址 |

**WITHDRAW**

| 名称            | 类型   | 描述     |
| --------------- | ------ | -------- |
| network         | string | 网络     |
| networkName     | string | 网络名称 |
| transactionHash | string | 交易哈希 |
| networkTxUrl    | string | 地址URL  |
| actualReceived  | string | 接收数量 |
| feeAmount | string | 费用金额 |
| receivedAddress | string | 接收地址 |

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
                "receivedAddress": "TErLvDQXaDciLR8RPH45BfM3z7Hxua4sVt",
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







