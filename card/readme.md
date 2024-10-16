### 1. 查看卡申请列表

**描述：** 查看可申请的卡列表

- **URL:** `/card/v1/list`
- **方法:**  `GET`
- **请求参数:**  无

- **响应:**

| 名称                  | 类型    | 描述                                      |
| --------------------- | ------- |-----------------------------------------|
| type | string | 卡类型 <br>- `VIP`：VIP卡  <br>- `NORMAL`：普通卡 |
| name | string | 卡名称                                     |
| badge | string | 卡徽章                                     |
| image | string | 卡图片链接                                   |
| tags | list | 卡功能标签字符串列表                              |

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

| 名称             | 类型     | 是否必须 | 描述                |
|----------------|--------|------|-------------------|
| externalUserId | string | Y  | DeCard用户ID        |


- **响应:**

| 名称  | 类型   | 描述                 |
| ----- | ------ | -------------------- |
| cardType | string | 卡类型 <br>- `VIP`：VIP卡  <br>- `NORMAL`：普通卡 |
| cardNo | string | 卡号，只展示后4位 |
| cardHolder | string | 持卡人姓名 |
| cardImage | string | 卡背景图片 |
| cardEmbossingName | string | 卡片刻印名 |
| cardStatus | string | 卡片的当前状态：<br>- `NORMAL`：正常<br>- `FROZEN`：冻结<br>- `CANCELLED`：已取消<br>- `INACTIVE`：未激活 |
| physicalCardStatus | string | 实体卡状态实体卡片的当前状态：<br>- `UN_APPLY`：未申请<br>- `INACTIVE`：未激活<br>- `ACTIVE`：活跃<br>- `REPLACE`：需更换<br>- `FROZEN`：冻结<br>- `CANCELLED`：已取消 |
| walletBalance | string | 钱包余额 |
| caBalance | string | CA余额 |
| cardBalance | string | 卡片可用余额 |
| points | int | 用户积分 |


- **响应示例:**

```json
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": [
        {
            "association":"MASTER_CARD",
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

| 名称             | 类型      | 是否必须 | 描述                   |
|----------------|---------|------|----------------------|
| externalUserId | string  | Y  | DeCard用户ID           |
| block | boolean | Y  | true: 冻结卡；false: 解冻卡           |
| cardMantissa   | string  | N   | 卡尾号4位<br />多卡区分，默认第一张卡 |
| smsCode | string  | N   | 短信验证码；**解冻卡需传递** |

- **响应:**

| 名称 | 类型    | 描述       |
| ---- | ------- | ---------- |
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

### 4. 卡消费历史USD

**描述：** 卡消费历史（USD）

- **URL:** `/card/v1/transactions`
- **方法:**  `POST`
- **请求参数:**

| 名称             | 类型            | 是否必须 | 描述                                      |
|----------------|---------------|------|-----------------------------------------|
| externalUserId | string        | Y  | DeCard用户ID                              |
| cardMantissa   | string        | N   | 卡尾号4位，多卡区分，默认第一张卡                       |
| page           | int           | N   | 默认1                                     |
| size           | int       | N    | 默认100；每页条数[1,100]                       |
| type           | string        | N   | 交易类型 <br>- `SPEND`：消费 <br>- `REFUND`：退款 |
| startTime      | LocalDateTime | N   | 开始时间戳                                   |
| endTime        | LocalDateTime | N   | 截止时间戳                                   |

- **响应:**

| 名称         | 类型   | 描述                                                                        |
| ------------ | ------ |---------------------------------------------------------------------------|
| id           | string | 卡交易序列号                                                                    |
| status       | string | 卡交易状态 [COMPLETE]                                                          |
| type         | string | 卡交易类型 <br>- `SPEND`：消费 <br>- `REFUND`：退款  <br>- `PARTIAL_REFUND`：部分退款 |
| asset        | string | 卡交易币种                                                                     |
| amount       | string | 交易金额                                                                      |
| refundAmount | string | 退款金额                                                                      |
| merchantName | string | 商户名称                                                                      |
| merchantNo   | string | 商户编号                                                                      |
| time         | string | 交易时间                                                                      |


- **响应示例:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": {
                "total": 1,
                "rows": [{
                        "id": "586188753138826666912468",
                        "status": "COMPLETE",
                        "type": "SPEND",
                        "asset": "USD",
                        "amount": "5.51",
                        "refundAmount": "0",
                        "merchantName": "FIRST THIRD BANK       ST. LOUIS     MO ",
                        "merchantNo": "1726996149737",
                        "time": "1726216150289"
                }]
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





