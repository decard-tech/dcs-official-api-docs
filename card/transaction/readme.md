### 1. Auth

**描述：** 交易授权

- **URL:** ` `
- **方法:**  `POST`
- **请求参数:**  

| 名称                           | 类型         | 描述                                                                                |
|------------------------------|------------|-----------------------------------------------------------------------------------|
| cardNumber                   | string     | 卡号                                                                                |
| customerId                   | string     | 客户ID                                                                              |
| reqseqNo                     | string     | 请求序列号                                                                             |
| transactionTypeTopCode       | string     | 授权大类 <br>- `P`：反向交易                                                               |
| transactionTypeDetailCode    | string     | 授权细类                                                                              |
| transactionCurrencyCode      | string     | 交易币种 <br>- 三位数字币种代码                                                               |
| transactionAmount            | BigDecimal | 交易金额                                                                              |
| settlementCurrencyCode       | string     | 清算币种 <br>- 三位数字币种代码                                                               |
| settlementAmount             | BigDecimal | 清算金额                                                                              |
| requestCurrencyCode          | string     | 请求partner保证金币种 <br>- 三位数字币种代码                                                     |
| requestAmount                | BigDecimal | 请求partner保证金金额                                                                    |
| servicePointCardCode         | string     | Pos Entry Mode                                                                    |
| servicePointPinCode          | string     | 密码输入方式                                                                            |
| servicePointConditionCode    | string     | 条件码                                                                               |
| transmissionTime             | string     | 交易时间                                                                              |
| systemTraceAuditNumber       | string     | 系统跟踪号                                                                             |
| localTransactionTime         | string     | 交易时间                                                                              |
| localTransactionDate         | string     | 交易日期                                                                              |
| cardExpirationDate           | string     | 卡片有效期                                                                             |
| merchantType                 | string     | MCC                                                                               |
| merchantCountryCode          | string     | 商户国家码                                                                             |
| acquiringIdentificationCode  | string     | 收单机构号                                                                             |
| forwardingIdentificationCode | string     | 转发机构号                                                                             |
| cardAcceptorTerminalCode     | string     | 商户终端号                                                                             |
| cardAcceptorIdentification   | string     | 商户号                                                                               |
| cardAcceptorNameLocation     | string     | 商户名称+地址                                                                           |
| mobileNo                     | string     | 手机号                                                                               |
| networkReferenceId           | string     | 网络参考Id                                                                            |
| reversalType                 | string     | 冲正类型 <br>- `1`：授权交易 <br>- `3`：撤销交易 <br>- `4`：冲正交易 <br>- `5`：撤销冲正交易 <br>- `6`：退货交易 |
| originLocalTransactionTime   | string     | 原交易的交易时间 <br>- format: HHmmss                                                     |
| originLocalTransactionDate   | string     | 原交易交易日期 <br>- format: MMdd                                                        |
| originTraceNo                | string     | 原交易系统跟踪号                                                                          |
| preAuthInd                   | string     | 预授权指示 <br>- `0`：其他交易 <br>- `1`：预授权交易 <br>- `2`：预授权完成交易                            |

- **响应:**

| 名称 | 类型     | 描述                                              |
|----|--------|-------------------------------------------------|
|    | String | 在body中返回 <br>- `A`: approved  <br>- `D`: denied |

- **响应示例:**

```
A
```

---

### 2.入账文件内容示例

<a href="./partner_transaction_20241008.txt">partner_transaction_20241008.txt</a>
