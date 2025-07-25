### 1. 新建用户

**描述：** 新建用户

- **URL:** `/account/v1/register`
- **方法:** `POST`
- **请求参数:**

| 名称         | 类型     | 是否必传 | 描述            |
|------------|--------|------|---------------|
| mobileCode | string | Y    | 国家简码，示例：CN，US |
| mobile     | string | Y    | 手机号           |
| smsCode    | string | Y    | 短信验证码         |

- **响应示例:**

| 名称   | 类型     | 描述         |
|------|--------|------------|
| data | string | DeCard用户ID |

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": "298fe06f-0066-4565-ae20-3697cd9d664e",
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

备注：手机号已兼容支持[AES密文传输](../flow/readme.md#敏感信息加密算法)，接入方可按需使用



***

### 2. 获取引导页链接

**描述：** 获取引导页链接

- **URL:** `/redirect/v1/guidance-link`
- **方法:** `POST`
- **请求参数:**

| 名称                 | 类型     | 是否必须 | 描述                                                      |                
|--------------------|--------|------|---------------------------------------------------------| 
| action             | string | Y    | 引导页类型  <br>- `KYC_GUIDE`：KYC引导页  <br>- `CARD_INFO`：卡信息页 |                                                                                          |
| externalUserId     | string | Y    | DeCard用户ID                                              |    
| successRedirectUrl | string | Y    | 成功后的跳转地址                                                | 
| errorRedirectUrl   | string | Y    | 失败后的跳转地址                                                |
| referer            | string | N    | 引用来源 (添加该参数后，打开返回的链接时会校验请求的引用来源)                        |
| userAgent          | string | Y    | 用户代理 (添加该参数后，打开返回的链接时会校验请求的用户代理),不填可能导致页面访问失败           |
| language           | string | N    | 语言(默认英文)                                                |
| cardMantissa       | string | Y    | 卡号后四位                                                   |

- **响应:**

| 名称   | 类型     | 描述     |
|------|--------|--------|
| data | string | 临时访问链接 |

```json
{
  "code": "SYS_SUCCESS",
  "data": "https://{domain}/{zh|en}/card/{scheme|detail}?secret=qIHyX5xWBJ24PJIcOdmos1piblnglBNoTrw0Ejkqmso",
  "success": true
}

提醒：
（1）引导页链接有效期为5分钟，超时无法打开，打开后的链接会自动失效。
（2）用户提供或约定referer、ua等来自partners的信息，只接受提供或约定的referer和ua打开页面

```

- 失败响应

```json
  {
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

***

### 3. 查询用户状态

**描述：** 获取引导页链接

- **URL:** `/account/v1/user-status`
- **方法:** `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | string | Y    | DeCard用户ID |

- **成功响应:**

| 名称                    | 类型      | 描述        |
|-----------------------|---------|-----------|
| forbidWithdraw        | boolean | 用户是否禁止提现  |
| forbidCardTransaction | boolean | 用户是否禁止卡交易 |

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
    "forbidWithdraw": false,
    "forbidCardTransaction": false
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

***

### 4. 查看用户KYC状态

**描述：** 获取引导页链接

- **URL:** `/account/v1/kyc-status`
- **方法:** `GET`
- **请求参数:**

| 名称             | 类型     | 是否必须 | 描述         |
|----------------|--------|------|------------|
| externalUserId | string | Y    | DeCard用户ID |

- **成功响应:**

| 名称   | 类型     | 描述                                                                                                                                                                                                                          |
|------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| data | string | KYC状态 <br>- `UNDO`：未做KYC <br>- `INIT`：初始状态 <br>- `PENDING`：申请已经成功提交完成，正在审核中。<br />该状态有几种可能：<br />1）等待自动审核： 一般 3 分钟内通过，如果没有则进入人工审核。<br />2）等待人工审核： 人工审核包括 Operation team 通过邮件联系用户提供更多资料。 <br>- `PASS`：审核通过 <br>- `REFUSE`：拒绝 |

**成功响应:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": "PASS",
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

### 5. 绑定推荐关系

**描述：** 绑定推荐关系

- **URL:** `/account/v1/bind-invite-relationship`
- **方法:** `POST`
- **请求参数:**

| 名称                    | 类型     | 是否必须 | 描述   |
|-----------------------|--------|------|------|
| externalUserId        | string | Y    | 被推荐人 |
| externalInviterUserId | string | Y    | 推荐人  |

- **成功响应:**

| 名称   | 类型   | 描述  |
|------|------|-----|
| data | void | N/A |

**成功响应:**

```json
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": null,
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

### 6. 查询travel rule

**描述**：根据External User ID查询对应travel rule信息

- **URL**：`/account/v1/query-travel-rule`

- **方法**：`GET`
- **请求参数**：

| 名称             | 类型     | 是否必须 | 描述        |
|----------------|--------|------|-----------|
| externalUserId | string | Y    | decard用户号 |

- **成功响应**：

| 名称             | 类型     | 描述         |
|----------------|--------|------------|
| externalUserId | String | decard用户id |
| name           | String | 用户名        |
| type           | String | 证件类型       |
| value          | String | 证件号        |
| countryOfIssue | String | 证件颁发国家     |
| city           | String | 城市         |
| postalCode     | String | 邮编         |
| addressLines   | String | 详细地址       |
| dateOfBirth    | String | 出生日期       |
| placeOfBirth   | String | 出生国家       |

**成功响应**：

```
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": {
  		"externalUserId":"1111",
        "name": "John Tan",
        "id": {
            "type": "PASSPORT",
            "value": "S7654321A",
            "countryOfIssue": "SG"
        },
        "address": {
            "city": "Singapore",
            "postalCode": "068907",
            "addressLines": "140 Robinson Road"
        },
        "dateOfBirth": "2000-01-01",
        "placeOfBirth": "SG"
    },
  "success": true
}
```

**失败响应：**

```
{
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

### 7. 更新travel rule

**描述：**根据External User ID更新对应travel rule信息

- **URL**:`/account/v1/update-travel-rule`
- **方法**：`POST`
- **请求参数**：

| 名称                | 类型     | 描述         | 是否必须 | 示例值                  |
|-------------------|--------|------------|------|----------------------|
| externalUserId    | String | decard用户id | Y    |                      |
| name              | String | 用户名        | Y    |                      |
| type              | String | 证件类型       | Y    | PASSPORT<br/>ID_CARD |
| value             | String | 证件号        | Y    |                      |
| countryOfIssue    | String | 证件颁发国家     | Y    | ISO 2位国家代码           |
| city              | String | 城市         | Y    |                      |
| postalCode        | String | 邮编         | Y    |                      |
| addressLines      | String | 详细地址       | Y    |                      |
| dateOfBirth       | String | 出生日期       | Y    |                      |
| placeOfBirth      | String | 出生国家       | Y    | ISO 2位国家代码           |
| dependentLocality | String |            |      |                      |
| region            | String | 区域         |      |                      |
| country           | String | 国家         |      | ISO 2位国家代码           |
| channelName       | String | 渠道名称       | Y    |                      |
| institutionName   | String |            | Y    | FOMO                 |

**请求报文示例：**

```
{
	"externalUserId": "888368b5-b97b-4bc8-a05e-f5d382d85ca8",
	"channelName": "Wallet",
	"institutionName": "FOMO",
	"name": "John Tan",
	"id": {
		"type": "PASSPORT",
		"value": "S7654321A",
		"countryOfIssue": "SG"
	},
	"address": {
		"city": "Singapore",
		"country": "SG",
		"dependentLocality": "string",
		"postalCode": "068907",
		"region": "string",
		"addressLines": "140 Robinson Road"
	},
	"dateOfBirth": "2000-01-01",
	"placeOfBirth": "SG"
}
```

- 成功响应

| 名称   | 类型   | 描述  |
|------|------|-----|
| data | void | N/A |

**成功响应：**

```
{
  "code": "SYS_SUCCESS",
  "message": null,
  "messageDetail": null,
  "data": null,
  "success": true
}
```

**失败响应：**

```
{
  "code": "ERROR-CODE",
  "message": "simple describe, see error-code list",
  "success": false
}
```

