## 2024-11-15
[All Commits](https://github.com/traefik/traefik/compare/v3.1.3...v3.1.4)

**account**
- [新增接口用于传递邀请关系](account/readme.md#)
- 

**websocket**
- [kyc状态](websocket/readme.md#kyc状态-kyc_status)
- [资产变更](websocket/readme.md#数字资产变动-balance_change)
- [卡交易记录推送](websocket/readme.md#卡交易记录-card_transaction)

**asset**
- [balance会新增asset（rusd-奖励金）只可消费不可退款](asset/readme.md#1-资产查询)
- [crypto历史会增加三种交易类型](asset/readme.md#2-资金历史查询)
  - reward_pay:  奖励金支付
  - reward_distribution: 退换奖励金
  - reward_refund: 发放奖励金（可能来源消费挖矿和推荐返佣等活动）
- 资金池[充值](asset/readme.md#4-资金池充值)、[提现](asset/readme.md#5-资金池提现)、[操作结果查询](asset/readme.md#6-资金池操作查询)

**card**
- [新增卡交易历史接口v2](card/readme.md#5)