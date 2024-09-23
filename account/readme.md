# 交互流程
![](../images/flow.png)
---


# 新建用户
`POST:   /account/v1/register`

**参数:**

| 名称  | 类型  | 是否必须 | 描述 |
| ------------ | ------------ |------|----|
|  mobileCode | String  | YES  | 二字国家简码  |
|  mobile | String  | YES  | 手机号  |
|  email | String  | NO   | 邮箱  |


**响应:**
```json
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": "298fe06f-0066-4565-ae20-3697cd9d664e", // DeCard用户ID
    "success": true
}
```



# 获取引导页链接
`POST:   /redirect/v1/guidance-link`

**参数:**

| 名称           | 类型 | 是否必须 | 描述                               |                                                                                          |
| ------------------ | -------- | ------------ | -------------------------------------- | ----------------------------------------------------------------------------------------:|
| action             | String   | YES          | 引导页类型<br>[*KYC_GUIDE, CARD_INFO*] |                                                                                          |
| externalUserId     | String   | YES          | DeCard用户ID                           |                                                                                          |
| successRedirectUrl | String   | YES          | 引导完成成功后重定向地址               |                                                                                          |
| errorRedirectUrl   | String   | YES          | 发生错误后重定向地址                   |                                                                                          |

**响应:**
```json
{
    "code": "SYS_SUCCESS",
    "message": null,
    "messageDetail": null,
    "data": "https://{domain}/{zh|en}/card/{scheme|detail}?secret=qIHyX5xWBJ24PJIcOdmos1piblnglBNoTrw0Ejkqmso" // 临时访问链接
    "success": true
}
```
- 引导页链接有效期为5分钟；
- 约定referer 、user-agent 等来自partners的信息，只接受约定referer和ua打开页面；




# 查询用户状态
`POST:   /account/v1/user-status`

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
        "data": {
                "isUserActive": false, // 用户是否激活
                "isUserDisabled": false, // 用户是否禁用
                "isUserLock": false, // 用户是否锁定
                "isUserSpecial": false, // 用户是否有问题
                "isUserForbidWithdraw": false, // 用户是否禁止提现
                "isUserForbidCardTransaction": false // 用户是否禁止卡交易

        },
        "success": true
}
```



# 查看用户KYC状态
`POST:   /account/v1/kyc-status`

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
        "data": "PASS" // KYC状态 [INIT,REVIEW,PENDING,PASS,REFUSE]
        "success": true
}
```

