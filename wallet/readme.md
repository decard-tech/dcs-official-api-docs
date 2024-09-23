# balance查询

`GET:   /user-asset/v1/balance`

**参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  externalUserId | String  | YES  | DeCard用户ID  |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "asset": "USDT", // 资产币种
                "free": "5.512075", // 可用数量
                "freeze": "0", // 冻结数量
                "total": "5.512075", // 总数量
                "network": "TRON" // 链
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



# 查询crypto资金历史

`POST:   /user-asset/v1/transactions`

**参数:**

| 名称             | 类型  | 是否必须 | 描述                                                                                         |
|----------------| ------------ |------|--------------------------------------------------------------------------------------------|
| externalUserId | String  | YES  | DeCard用户ID                                                                                 |
| type           | String  | NO  | 交易类型 <br>DEPOSIT、WITHDRAW、CARD_PRINTING_FEE、CARD_POSTAL_FEE、CARD_VIP_FROZEN_FEE、CONVERSION |
| page           | Integer       | NO   | 页码[1,……]；默认1                                                                               |
| size           | Integer       | NO    | 每页条数[1,100] ； 默认100                                                                        |
| startTime      | LocalDateTime | NO   | 开始时间戳;默认Now()-7D                                                                           |
| endTime        | LocalDateTime | NO   | 截止时间戳;  默认Now()                                                                                    |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "tranId": "1041201345182711808", // 交易单号
                "externalId": "1041201333329608704", // 外部交易ID
                "type": "CONVERSION", // 交易类型
                "freeDelta": "-3.66208981", // 交易数量
                "time": "1726200144966", // 交易时间
                "asset": "USDT" // 交易币种
        }],
        "success": true
}
```


# 查询crypto资金变动详情

`POST:   /user-asset/v1/transaction-detail`

**参数:**

| 名称            | 类型     | 是否必须 | 描述                                                                                         |
|---------------|--------|------|--------------------------------------------------------------------------------------------|
| externalUserId | String | YES  | DeCard用户ID                                                                                 |
| tranId        | Long   | YES  | 交易单号                                                                                          |
| externalTranId| String | YES  | 外部交易ID                                                                                          |
| type          | String | YES  | 交易类型<br>DEPOSIT、WITHDRAW、CARD_PRINTING_FEE、CARD_POSTAL_FEE、CARD_VIP_FROZEN_FEE、CONVERSION|


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": {
                "type": "CONVERSION", // 交易类型
                "time": "1726216153194", // 交易时间
                "freeDelta": "5.50768677", // 资产变动数量
                "asset": "USDT", // 资产
                "orderId": "1041268486317686785", // 单号
                "status": "SUCCESS" // 状态 [SUCCESS, FAIL, PENDING]
                // 以下部分根据交易类型返回不同
                // CONVERSION
                "baseAsset": "USDT", // 基础资产 
                "quoteAsset": "USD", // 目标资产
                "price": "1.00042", // 报价
                // DEPOSIT
                "network": "TRON",
                "networkName": "TRON",
                "transactionHash": "4edc1eb2d9d6b6a153629f65909bdcfced9d00d50988a5f1b57049fa1cb34126",
                "networkTxUrl": "https://tronscan.org/#/transaction/",
                "sendAddress": "TErLvDQXaDciLR8RPH45BfM3z7Hxua4sVt",
                // WITHDRAW
                "network": "TRON",
                "networkName": "TRON",
                "transactionHash": "2c80428268edd5cc5436e6fadf175503167f982644924985d68a1deefbcacbd6",
                "networkTxUrl": "https://tronscan.org/#/transaction/",
                "actualReceived": "990",
                "feeAmount": "10",
                "receivedAddress": "TErLvDQXaDciLR8RPH45BfM3z7Hxua4sVt",
        },
        "success": true
}
```















