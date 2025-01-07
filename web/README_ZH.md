## 前端接入规范

### 支持平台

DCS 页面支持以下访问方式：

| 访问类型     | 是否支持 | 配置                                                                     |
|-----------------|-----------|-------------------------------------------------------------------------------|
| 原生浏览器  | 是       | 无                                                                          |
| IFrame          | 是       | CSP 域名白名单，渠道白名单 |
| WebView         | 是       | user-Agent 白名单   |

> 接入方需告知访问类型，平台需进行相关配置。


### 生成链接注意点
出于对用户安全的考虑，会对所有访问 DCS 的初始链接进行安全校验，未校验通过会限制页面访问（Page Not Found），以下条件满足其一即放行通过。

####  1. 通过对浏览器Header中的 user-Agent 做关键词判断（仅适用于app内的webview场景）

```code
AppleWebKit/537.36 (KHTML, like Gecko) Decard/0.0.1 Mobile Safari/537.36
```
上面 user-Agent 字符匹配到 Decard 关键词则直接放行
由接入方 和 平台方 (DCS) 共同支持

接入方
  - 需提供定制化的 user-Agent 关键词

平台方 (DCS)
  - DCS 平台方需要在白名单中进行加白处理

#### 2. 通过对浏览器 Header 中的 user-Agent 与接入方接口请求中提供的 user-Agent 进行(===)全校验
由接入方支持
- 接入方需自行保证 “请求链接接口时” 和 “链接访问浏览器” 的 user-Agent 完全一致

#### 3. 通过对浏览器 Header 中的 referer 与接入方接口请求中提供的 referer 做（include）对比
- TODO：接入方
- 接入方需自行保证 “请求链接接口时” 的 referer 与 “访问 DCS 页面时” 的 referer 一致

### 平台内通信
为提升用户体验，在 IFrame 与 WebView 中，特定交互场景下采用 `postMessage` 机制通知父容器，实现更好的用户体验，因此 Webview 和 Iframe 容器可以监听这些事件作出处理

| 名称     | 描述 | 数据类型                                                                     |
|-----------------|-----------|-------------------------------------------------------------------------------|
| `DECARD_OPENAPI_CLOSE`  | 关闭页面       | { "name": "DECARD_OPENAPI_CLOSE", "payload": null }                                                                          |
| `DECARD_OPENAPI_MFA_OPENED`          | 卡详情 OTP 验证弹窗激活时触发       | { "name": "DECARD_OPENAPI_MFA_OPENED", "payload": null }      |
| `DECARD_OPENAPI_CARD_DETAIL_OPENED`         | 卡面信息激活时触发       | { "name": "DECARD_OPENAPI_CARD_DETAIL_OPENED", "payload": null }   |

### 错误处理与跳转
DCS 中的所有错误情况都由内部自动处理。

| Code     | Description |
|-----------|-----------|
| 10000     | 通用错误       |
| 10001     | 登录态丢失       |
| 10002     | 禁止访问非流程以外的页面        |
| 10003     | 不在UA 白名单中     |
| 10004     | Secret 不可用     |

### ⚠️ 注意事项
- 由于 Safari 本身的安全策略限制， 即便设置 cookie 的sameSite 属性也无法进行 cookie 的传递，因此建议 Safari 不使用 Iframe 的方案。
- 同样的，异步的 window.open 也会被阻止，建议基于当前页面打开新链接。
