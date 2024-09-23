### 查询用户钱包配置
**描述：** 查看可申请的卡列表

- **URL:** `/crypto/v1/network-coin`
- **方法:**  `GET`
- **请求参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  type | String  | Y  | DEPOSIT,  WITHDRAW  |

- **响应:**

| 名称                    | 类型    | 描述               |
| ----------------------- | ------- | ------------------ |
| network                 | String  | 网络               |
| asset                   | String  | 币种               |
| depositEnable           | Boolean | 充值开关           |
| withdrawEnable          | Boolean | 提现开关           |
| withdrawIsTag           | Boolean | 提币是否需要tag    |
| blockUrl                | String  | 区块浏览器         |
| txUrl                   | String  | 地址URL            |
| addressUrl              | String  | 地址URL            |
| addressRegex            | String  | 地址正则           |
| memoRegex               | String  | memo 正则          |
| minConfirm              | Integer | 最小确认数         |
| lockConfirm             | Integer | 锁定数             |
| estimatedArrivalTime    | Integer | 预计到达时间(分钟) |
| withdrawIntegerMultiple | String  | 提币整数倍         |
| depositDesc             | String  | 充值描述           |
| withdrawDesc            | String  | 提现描述           |
| specialTips             | String  | 特殊提示           |
| withdrawFee             | String  | 提币手续费         |
| withdrawMin             | String  | 单笔最小提币数量   |
| withdrawMax             | String  | 单笔最大提币数量   |
| depositDust             | String  | 充值最小值         |
| contractAddress         | String  | 合约地址           |


- **响应示例:**


```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": [{
                "network": "TRON",
                "asset": "USDT",
                "depositEnable": true,
                "withdrawEnable": true,
                "withdrawIsTag": true,
                "blockUrl": "https://tronscan.org/#/block/",
                "txUrl": "https://tronscan.org/#/transaction/",
                "addressUrl": "https://tronscan.org/#/address/",
                "addressRegex": "^T[a-zA-Z0-9]{33,34}$",
                "memoRegex": "",
                "minConfirm": 1,
                "lockConfirm": 1,
                "estimatedArrivalTime": 30,
                "withdrawIntegerMultiple": "0.000001",
                "depositDesc": null,
                "withdrawDesc": null,
                "specialTips": null,
                "withdrawFee": "6.5",
                "withdrawMin": "0.5",
                "withdrawMax": "9999999",
                "depositDust": "0.00000001",
                "contractAddress": ""
        }],
        "success": true
}
```

---



### 查询入金地址

**描述：** 入金地址查询

- **URL:** `/crypto/v1/deposit-address`
- **方法:**  `GET`
- **请求参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  externalUserId | String  | Y  | DeCard用户ID  |
|  coin | String  | Y  | 资产币种  |
|  network | String  | Y  | 网络  |

- **响应:**

| 名称              | 类型    | 描述       |
| ----------------- | ------- | ---------- |
| network           | String  | 网络       |
| coin              | String  | 币种       |
| address           | String  | 地址       |
| addressTips       | String  | 提示       |
| minDepositAmount  | String  | 充值最小值 |
| minConfirmationNo | Integer | 最小确认数 |


- **响应示例:**


```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": { 
                "network": "TRON",
                "coin": "USDT",
                "address": "TJX8Xzj3XzL1Bf9XR63TwTn5ynjnD1qwrh",
                "addressTips": "crypto_spec_tips",
                "minDepositAmount": "0.00000001",
                "minConfirmationNo": 15
        },
        "success": true
}
```
