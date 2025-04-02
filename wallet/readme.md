### 查询用户钱包配置

**描述：** 查看用户钱包配置

- **URL:** `/crypto/v1/network-coin`
- **方法:**  `GET`
- **请求参数:**

| 名称   | 类型     | 是否必须 | 描述                 |
|------|--------|------|--------------------|
| type | string | Y    | DEPOSIT,  WITHDRAW |

- **响应:**

| 名称                      | 类型      | 描述         |
|-------------------------|---------|------------|
| network                 | string  | 网络         |
| networkNativeAsset      | string  | 原生代币       |
| asset                   | string  | 币种         |
| logo                    | string  | 币种logo     |
| depositEnable           | boolean | 充值开关       |
| withdrawEnable          | boolean | 提现开关       |
| withdrawIsTag           | boolean | 提币是否需要tag  |
| blockUrl                | String  | 区块浏览器      |
| txUrl                   | string  | 地址URL      |
| addressUrl              | string  | 地址URL      |
| addressRegex            | string  | 地址正则       |
| memoRegex               | string  | memo 正则    |
| minConfirm              | int     | 最小确认数      |
| lockConfirm             | int     | 锁定数        |
| estimatedArrivalTime    | int     | 预计到达时间(分钟) |
| withdrawIntegerMultiple | string  | 提币整数倍      |
| depositDesc             | string  | 充值描述       |
| withdrawDesc            | string  | 提现描述       |
| specialTips             | string  | 特殊提示       |
| withdrawFee             | string  | 提币手续费      |
| withdrawMin             | string  | 单笔最小提币数量   |
| withdrawMax             | string  | 单笔最大提币数量   |
| depositDust             | string  | 充值最小值      |
| contractAddress         | String  | 合约地址       |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": [
    {
      "network": "TRON",
      "asset": "USDT",
      "logo": "https://static-dev.thedecard.com/20240813/1adaec74-05ec-40fc-930c-24932b2ca042.svg",
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
    }
  ],
  "success": true
}
```

---

### 查询入金地址

**描述：** 入金地址查询

- **URL:** `/crypto/v1/deposit-address`
- **方法:**  `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | string | Y    | DeCard用户ID |
| coin           | string | Y    | 资产币种       |
| network        | string | Y    | 网络         |

- **响应:**

| 名称                | 类型     | 描述    |
|-------------------|--------|-------|
| network           | string | 网络    |
| coin              | string | 币种    |
| address           | string | 地址    |
| addressTips       | string | 提示    |
| minDepositAmount  | string | 充值最小值 |
| minConfirmationNo | int    | 最小确认数 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "network": "TRON",
    "networkName": "TRON",
    "networkNativeAsset": "TRX",
    "coin": "USDT",
    "address": "TJX8Xzj3XzL1Bf9XR63TwTn5ynjnD1qwrh",
    "addressTips": "crypto_spec_tips",
    "minDepositAmount": "0.00000001",
    "minConfirmationNo": 15
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

### 提现

**描述：** 提现

- **URL:** `/crypto/v1/withdraw-apply`
- **方法:**  `POST`
- **请求参数:**

| 名称             | 类型         | 是否必须 | 描述              |
|----------------|------------|------|-----------------|
| externalUserId | string     | Y    | DeCard用户ID      |
| smsCode        | string     | Y    | 短信验证码           |
| network        | string     | Y    | 网络              |
| coin           | string     | Y    | 币种              |
| address        | string     | Y    | 地址              |
| addressTag     | string     | N    | 标签              |
| amount         | bigDecimal | Y    | 数量(positive)    |
| fee            | bigDecimal | Y    | 提币手续费(positive) |

- **响应:**

| 名称                   | 类型     | 描述     |
|----------------------|--------|--------|
| externalTranId       | string | 交易ID   |
| applyTime            | string | 申请时间   |
| estimatedArrivalTime | string | 预计到账时间 |

- **响应示例:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "externalTranId": "1077919180558176257",
    "applyTime": "1734954365223",
    "estimatedArrivalTime": "1734972365223"
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

### 获取fomo充值地址

**描述**：获取充值地址

- **URL**:`/crypto/v2/deposit-address`
- **方法**：`POST`
- **请求参数：**

| 名称           | 类型   | 是否必须 | 描述         |
| -------------- | ------ | -------- | ------------ |
| externalUserId | String | Y        | DeCard用户ID |
| network        | String | Y        |              |

**请求示例：**

```
{
   "externalUserId":"1111111",
    "network":"TRON"
  }
```

- **响应**：

| 名称    | 类型   | 描述     |
| ------- | ------ | -------- |
| network | String |          |
| address | String | 充值地址 |
| fxRate  | String | 费率     |

**成功响应示例：**

```
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "network": "TRON",
    "address": "TJX8Xzj3XzL1Bf9XR63TwTn5ynjnD1qwrh",
    "fxRate": 0.54
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

### 获取banktransfer充值地址

**描述**：获取银行转账充值地址

- **URL**:`/crypto/v1/bank-address`
- **方法**：`POST`
- **请求参数：**

| 名称           | 类型   | 是否必须 | 描述         |
| -------------- | ------ | -------- | ------------ |
| externalUserId | String | Y        | DeCard用户ID |
| network        | String | Y        |              |

**请求示例：**

```
{
   "externalUserId":"1111111",
    "network":"TRON"
  }
```

- 

- **响应**：

| 名称    | 类型   | 描述 |
| ------- | ------ | ---- |
| account | String |      |
| name    | String |      |

**成功响应示例：**

```
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "account": "xxx",
    "name": "tom"
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

