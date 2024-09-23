# 查看卡申请列表
`GET:   /card/v1/list`

**参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|   |   |   |   |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                        "type": "VIP",  // 卡类型 [VIP, NORMAL]
                        "name": "高端卡", // 卡名称
                        "badge": "VIP", // 卡徽章
                        "image": "https://static.thedecard.com/card/apply/vip.webp", // 卡图片链接
                        "tags": [ // 卡功能标签
                                "无限额度消费",
                                "消费时自动兑换U",
                                "专属尊享礼遇"
                        ]
                },
                {
                        "type": "NORMAL",
                        "name": "普通卡",
                        "badge": "",
                        "image": "https://static.thedecard.com/card/apply/normal.webp",
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


# 卡详情(状态)查询

`GET:   /card/v1/detail`

**参数:**

| 名称             | 类型  | 是否必须 | 描述                   |
|----------------| ------------ |------|----------------------|
| externalUserId | String  | YES  | DeCard用户ID           |
| cardMantissa   | String  | NO   | 卡尾号4位<br>多卡区分，默认第一张卡 |

**响应:**
```json
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": {
        "cardType": "NORMAL", // 卡类型 [VIP, NORMAL]
        "cardNo": "************0643", // 卡号，只展示后4位
        "cardHolder": "****HAO", // 持卡人姓名
        "cardImage": "https://static.thedecard.com/card/main/normal-bg.webp", // 卡背景图片
        "cardEmbossingName": "", // 卡片刻印名
        "cardStatus": "NORMAL", // 卡片状态 [NORMAL, FROZEN, CANCELLED, INACTIVE]
        "physicalCardStatus": "ACTIVE", // 实体卡状态
        "walletBalance": "1.87", // 钱包余额
        "caBalance": "0", // CA余额
        "cardBalance": "1.87", // 卡片可用余额
        "points": 0 // 用户积分
    },
    "success": true
}
```



# 冻卡解冻

`POST:   /card/v1/block`

**参数:**

| 名称             | 类型     | 是否必须 | 描述                   |
|----------------|--------|------|----------------------|
| externalUserId | String | YES  | DeCard用户ID           |
| block | Boolean      | YES  | true: 冻结卡；false: 解冻卡           |
| cardMantissa   | String | NO   | 卡尾号4位<br>多卡区分，默认第一张卡 |

**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": true,
        "success": true
}
```


# 卡消费历史USD
`POST:   /card/v1/transactions`

**参数:**

| 名称             | 类型            | 是否必须 | 描述                   |
|----------------|---------------|------|----------------------|
| externalUserId | String        | YES  | DeCard用户ID           |
| cardMantissa   | String        | NO   | 卡尾号4位<br>多卡区分，默认第一张卡 |
| page           | Integer       | NO   | 页码[1,……]；默认1         |
| size           | Integer       | NO    | 每页条数[1,100] ； 默认100  |
| type           | String        | NO   | 交易类型[SPEND, REFUND]  |
| startTime      | LocalDateTime | NO   | 开始时间戳                |
| endTime        | LocalDateTime | NO   | 截止时间戳                |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": {
                "total": 1,
                "rows": [{
                        "id": "586188753138826666912468", // 卡交易序列号
                        "status": "COMPLETE", // 卡交易状态 [COMPLETE]
                        "type": "SPEND", // 卡交易类型[SPEND, REFUND, PARTIAL_REFUND]
                        "asset": "USD", // 卡交易币种
                        "amount": "5.51", // 交易金额
                        "refundAmount": "0", // 退款金额
                        "merchantName": "FIRST THIRD BANK       ST. LOUIS     MO ", // 商户名称
                        "merchantNo": "1726216149737", // 商户编号
                        "time": "1726216150289" // 交易时间
                }]
        },
        "success": true
}
```















