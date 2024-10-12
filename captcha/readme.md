### 1. 发送短信验证码

**描述：**  发送短信验证码

- **URL:** `/captcha/v1/send-mobile-code`
- **方法:**  `POST`
- **请求参数:** 

| 名称             | 类型     | 是否必须 | 描述                |
|----------------|--------|------|-------------------|
| mobileCode | string | N  | 二字国家简码 |
| mobile | string | N  | 手机号 |
| externalUserId | string | N  | DeCard用户ID |
| behavioral | string | Y  | 行为<br /> - `REGISTER` :                  注册<br /> - `CARD_UNFROZEN`:        卡解冻 |


- **响应:**

| 名称                  | 类型    | 描述                                      |
| --------------------- | ------- |-----------------------------------------|
| data | String | mobile |


- **响应示例:**
```json
{
        "code": "SYS_SUCCESS",
        "message": null,
        "messageDetail": null,
        "data": "13012345678",
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

* **提示**
  1. 该接口只能通过IP白名单访问；
  2. 短信验证码有效期5分钟；
  3. mobileCode + mobile 或 externalUserId ， 二者必填其一；同时存在以externalUserId为主；



---



