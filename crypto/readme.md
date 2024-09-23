# 查询用户钱包配置
`GET:   /crypto/v1/network-coin`

**参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  type | String  | YES  | DEPOSIT,  WITHDRAW  |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "network": "TRON", // 网络
                "asset": "USDT", // 币种
                "depositEnable": true, // 充值开关
                "withdrawEnable": true, // 提现开关
                "withdrawIsTag": 0, // 提币是否需要tag
                "blockUrl": "https://tronscan.org/#/block/", // 区块浏览器
                "txUrl": "https://tronscan.org/#/transaction/", // 地址URL
                "addressUrl": "https://tronscan.org/#/address/", // 地址URL
                "addressRegex": "^T[a-zA-Z0-9]{33,34}$", // 地址正则
                "memoRegex": "", // memo 正则
                "minConfirm": 1, // 最小确认数
                "lockConfirm": 1, // 锁定数
                "estimatedArrivalTime": 30, // 预计到达时间(分钟) 
                "withdrawIntegerMultiple": "0.000001", // 提币整数倍
                "depositDesc": null, // 充值描述
                "withdrawDesc": null, // 提现描述
                "specialTips": null, // 特殊提示
                "withdrawFee": "6.5", // 提币手续费
                "withdrawMin": "0.5", // 单笔最小提币数量
                "withdrawMax": "9999999", // 单笔最大提币数量
                "depositDust": "0.00000001", // 充值最小值
                "contractAddress": "" // 合约地址
        }],
        "success": true
}
```


# 查询入金地址
`GET:   /crypto/v1/deposit-address`

**参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  externalUserId | String  | YES  | DeCard用户ID  |
|  coin | String  | YES  | 资产币种  |
|  network | String  | YES  | 网络  |


**响应:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": { 
                "network": "TRON", // 网络 
                "coin": "USDT", // 币种
                "address": "TJX8Xzj3XzL1Bf9XR63TwTn5ynjnD1qwrh", // 地址
                "addressTips": "crypto_spec_tips", // 提示
                "minDepositAmount": "0.00000001", // 充值最小值
                "minConfirmationNo": 15 // 最小确认数
        },
        "success": true
}
```
