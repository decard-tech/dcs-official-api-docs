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
| language           | string | N    | 语言(默认英文) <br>- `cn`：中文  <br>- `en`：英文                   |
| cardMantissa       | string | Y    | 卡号后四位                                                   |
| selectCardPageShow | string | Y    | 当action=`KYC_GUIDE`时生效。只有一张卡时，是否展示选卡页面, 0:不跳过；1:跳过      |

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


[//]: # (### 8. 生成文件预上传地址)

[//]: # ()
[//]: # (**描述：** 生成文件预上传地址，用于获取文件上传的临时URL)

[//]: # ()
[//]: # (- **URL:** `/account/v1/generate-file-upload-prepare`)

[//]: # (- **方法:** `POST`)

[//]: # (- **请求参数:**)

[//]: # ()
[//]: # (| 名称        | 类型           | 是否必须 | 描述       |)

[//]: # (|-----------|--------------|------|----------|)

[//]: # (| fileNames | List<String> | Y    | 文件名列表    |)

[//]: # ()
[//]: # (**请求报文示例：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "fileNames": [)

[//]: # (    "document1.pdf",)

[//]: # (    "document2.jpg",)

[//]: # (    "document3.png")

[//]: # (  ])

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (- **成功响应:**)

[//]: # ()
[//]: # (| 名称        | 类型     | 描述      |)

[//]: # (|-----------|--------|---------|)

[//]: # (| fileName  | String | 源文件名    |)

[//]: # (| url       | String | 上传地址    |)

[//]: # (| objectKey | String | 重命名后文件名 |)

[//]: # ()
[//]: # (**成功响应：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "code": "SYS_SUCCESS",)

[//]: # (  "message": null,)

[//]: # (  "messageDetail": null,)

[//]: # (  "data": [)

[//]: # (    {)

[//]: # (      "fileName": "document1.pdf",)

[//]: # (      "url": "https://s3.amazonaws.com/bucket/upload-url-1",)

[//]: # (      "objectKey": "uploads/20240819/abc123-document1.pdf")

[//]: # (    },)

[//]: # (    {)

[//]: # (      "fileName": "document2.jpg", )

[//]: # (      "url": "https://s3.amazonaws.com/bucket/upload-url-2",)

[//]: # (      "objectKey": "uploads/20240819/def456-document2.jpg")

[//]: # (    },)

[//]: # (    {)

[//]: # (      "fileName": "document3.png",)

[//]: # (      "url": "https://s3.amazonaws.com/bucket/upload-url-3", )

[//]: # (      "objectKey": "uploads/20240819/ghi789-document3.png")

[//]: # (    })

[//]: # (  ],)

[//]: # (  "success": true)

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (**失败响应：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "code": "ERROR-CODE",)

[//]: # (  "message": "simple describe, see error-code list",)

[//]: # (  "success": false)

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # ()
[//]: # ()
[//]: # (***)

[//]: # ()
[//]: # (### 9. 上传EDD文件)

[//]: # ()
[//]: # (**描述：** 上传增强尽职调查&#40;EDD&#41;文件，用于提交财富来源和资金来源相关文档)

[//]: # ()
[//]: # (- **URL:** `/account/v1/upload-edd-file`)

[//]: # (- **方法:** `POST`)

[//]: # (- **请求参数:**)

[//]: # ()
[//]: # (| 名称                | 类型                | 是否必须 | 描述                    |)

[//]: # (|-------------------|-------------------|------|-----------------------|)

[//]: # (| externalUserId    | String            | Y    | DeCard用户ID            |)

[//]: # (| requestRef        | String            | Y    | 唯一幂等ID                |)

[//]: # (| applyId           | String            | Y    | 虚拟卡申请ID               |)

[//]: # (| eddSowSalary      | EddSowSalary      | N    | 财富来源-薪水              |)

[//]: # (| eddSowBizRevenue  | EddSowBizRevenue  | N    | 财富来源-经营              |)

[//]: # (| eddSowGift        | EddSowGift        | N    | 财富来源-礼物              |)

[//]: # (| eddSowInheritance | EddSowInheritance | N    | 财富来源-继承              |)

[//]: # (| eddSowInvest      | EddSowInvest      | N    | 财富来源-投资              |)

[//]: # (| eddSowOther       | EddSowOther       | N    | 财富来源-其他              |)

[//]: # (| eddSofSaving      | EddSofSaving      | N    | 资金来源-储蓄收入            |)

[//]: # (| eddSofSale        | EddSofSale        | N    | 资金来源-出售收入            |)

[//]: # (| eddSofBizRevenue  | EddSofBizRevenue  | N    | 资金来源-经营收入            |)

[//]: # (| eddSofCrypto      | EddSofCrypto      | N    | 资金来源-数字货币收入          |)

[//]: # (| eddSofInvest      | EddSofInvest      | N    | 资金来源-投资收入            |)

[//]: # (| eddSofOther       | EddSofOther       | N    | 资金来源-其他              |)

[//]: # ()
[//]: # (**备注：** 至少需要提供一个财富来源字段&#40;eddSow*&#41;)

[//]: # ()
[//]: # (**EddSowSalary 对象结构:**)

[//]: # ()
[//]: # (| 名称           | 类型                           | 是否必须 | 描述     |)

[//]: # (|--------------|------------------------------|------|--------|)

[//]: # (| employerName | String                       | Y    | 雇员名称   |)

[//]: # (| jobTitle     | String                       | Y    | 工作头衔   |)

[//]: # (| annualIncome | String                       | Y    | 年收入    |)

[//]: # (| docUrls      | List<APIIntentTicketFileRequest> | Y    | 文件地址列表 |)

[//]: # ()
[//]: # (**EddSowBizRevenue 对象结构:**)

[//]: # ()
[//]: # (| 名称               | 类型                           | 是否必须 | 描述     |)

[//]: # (|------------------|------------------------------|------|--------|)

[//]: # (| businessName     | String                       | Y    | 业务名称   |)

[//]: # (| natureOfBusiness | String                       | Y    | 业务性质   |)

[//]: # (| annualRevenue    | String                       | Y    | 年营收    |)

[//]: # (| docUrls          | List<APIIntentTicketFileRequest> | Y    | 文件地址列表 |)

[//]: # ()
[//]: # (**EddSowGift 对象结构:**)

[//]: # ()
[//]: # (| 名称                        | 类型                           | 是否必须 | 描述       |)

[//]: # (|---------------------------|------------------------------|------|----------|)

[//]: # (| contributorName           | String                       | Y    | 贡献者的名字   |)

[//]: # (| relationshipToYou         | String                       | Y    | 关系       |)

[//]: # (| contributorSourceOfWealth | String                       | Y    | 贡献者的财富来源 |)

[//]: # (| estimatedNetWorth         | String                       | Y    | 财富净值     |)

[//]: # (| typeOfAssets              | String                       | Y    | 资产类型     |)

[//]: # (| yearAssetsReceived        | String                       | N    | 年接受资产    |)

[//]: # (| recurringGifts            | RecurringGiftsDetail         | N    | 经常性礼物    |)

[//]: # (| docUrls                   | List<APIIntentTicketFileRequest> | Y    | 文件地址列表   |)

[//]: # ()
[//]: # (**APIIntentTicketFileRequest 对象结构:**)

[//]: # ()
[//]: # (| 名称       | 类型     | 是否必须 | 描述   |)

[//]: # (|----------|--------|------|------|)

[//]: # (| fileName | String | Y    | 文件名  |)

[//]: # (| filePath | String | Y    | 文件路径 |)

[//]: # ()
[//]: # (**请求报文示例：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "externalUserId": "298fe06f-0066-4565-ae20-3697cd9d664e",)

[//]: # (  "requestRef": "REQ_20240819_001",)

[//]: # (  "applyId": "APPLY_20240819_001",)

[//]: # (  "eddSowSalary": {)

[//]: # (    "employerName": "ABC Company Ltd",)

[//]: # (    "jobTitle": "Software Engineer",)

[//]: # (    "annualIncome": "100000",)

[//]: # (    "docUrls": [)

[//]: # (      {)

[//]: # (        "fileName": "salary_certificate.pdf",)

[//]: # (        "filePath": "uploads/20240819/abc123-salary_certificate.pdf")

[//]: # (      },)

[//]: # (      {)

[//]: # (        "fileName": "employment_contract.pdf", )

[//]: # (        "filePath": "uploads/20240819/def456-employment_contract.pdf")

[//]: # (      })

[//]: # (    ])

[//]: # (  },)

[//]: # (  "eddSofCrypto": {)

[//]: # (    "platform": "Binance",)

[//]: # (    "holdingType": "CUSTODIAN",)

[//]: # (    "docUrls": [)

[//]: # (      {)

[//]: # (        "fileName": "crypto_statement.pdf",)

[//]: # (        "filePath": "uploads/20240819/ghi789-crypto_statement.pdf")

[//]: # (      })

[//]: # (    ])

[//]: # (  })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (- **成功响应:**)

[//]: # ()
[//]: # (| 名称   | 类型   | 描述  |)

[//]: # (|------|------|-----|)

[//]: # (| data | void | N/A |)

[//]: # ()
[//]: # (**成功响应：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "code": "SYS_SUCCESS",)

[//]: # (  "message": null,)

[//]: # (  "messageDetail": null,)

[//]: # (  "data": null,)

[//]: # (  "success": true)

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (**失败响应：**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (  "code": "ERROR-CODE",)

[//]: # (  "message": "simple describe, see error-code list",)

[//]: # (  "success": false)

[//]: # (})

[//]: # (```)
