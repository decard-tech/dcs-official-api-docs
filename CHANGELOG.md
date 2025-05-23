## 2025-01-15
- [websocket 消息增加序列号](/websocket/readme.md#监听频道)
- [websocket 消息查询](/websocket/readme.md#消息查询)

---

## 2024-12-30
- [提现](/wallet/readme.md#提现)
- 手机号支持[加密传输](flow/readme.md#敏感信息加密算法)
- [引导页](account/readme.md#2-获取引导页链接) 请求参数增加卡后四位字段
- [查询用户资产历史](asset/readme.md#2-资金历史查询) 返回增加唯一id字段
- [卡账单详情](card/readme.md#6-卡账单详情) 返回增加postedTransactionId字段

---

## 2024-12-12

- 测试域名变更*
  - api-testnet.thedecard.com -> api.uatdcd.com
  - stream-test.thedecard.com -> stream.uatdcd.com

- websocket
  - timestamp 序列化成string类型
  - BALANCE_CHANGE 添加 ExternalTranId
  - CARD_TRANSACTION 添加 externalTranId 、 systemTraceAuditNumber 、requestAmountInUsd

- asset详情接口增加 `REWARD_DISTRIBUTION` 和 `REWARD_PAY` [类型查询](asset/readme.md#3-查询资产变动详情)
- 引导页添加[语言入参](account/readme.md#2-获取引导页链接)

---


## 2024-11-15

**account**

- [新增接口用于传递邀请关系](account/readme.md#5-绑定推荐关系)
- [引导页支持可传入referer和UA](account/readme.md#2-获取引导页链接)

**websocket**

- [kyc状态](websocket/readme.md#kyc状态-kyc_status)
- [资产变更](websocket/readme.md#数字资产变动-balance_change)
- [卡交易记录推送](websocket/readme.md#卡交易记录-card_transaction)

**asset**

- [balance会新增asset（rusd-奖励金）只可消费不可退款](asset/readme.md#1-资产查询)
- [crypto历史会增加三种交易类型](asset/readme.md#2-资金历史查询)
  - reward_pay: 奖励金支付
  - reward_distribution: 奖励金发放（可能来源消费挖矿和推荐返佣等活动）
  - reward_refund: 奖励金退还
- 资金池[充值](asset/readme.md#4-资金池充值)、[提现](asset/readme.md#5-资金池提现)、[操作结果查询](asset/readme.md#6-资金池操作查询)

**card**
- [新增卡交易历史接口v2-账单列表](card/readme.md#5-卡账单列表)
- [新增卡交易历史接口v2-账单详情](card/readme.md#6-卡账单详情)

------

## 2025-04-01

**新增接口：**

**account**

- [查询travel rule](account/readme.md#6-查询travel rule)
- [更新travel rule](account/readme.md#7-更新travel rule)

**card**

- [查询法币变动记录](card/readme.md#7-查询法币变动记录)

**wallet**

- [获取FOMO充值地址](wallet/readme.md#获取fomo充值地址)
- [获取BankTransfer充值地址](wallet/readme.md#获取banktransfer充值地址)

**websocket**

- [订单状态通知](websocket/readme.md#订单状态通知 (ORDER_STATUS))

**修改接口**：

**asset**

- [资产查询](asset/readme.md#1-资产查询)
