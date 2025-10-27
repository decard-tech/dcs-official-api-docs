# 前端集成指南

## 支持的接入方式

DCS 页面支持多种接入方式，满足不同场景的集成需求：

| 接入方式 | 支持状态 | 配置要求 |
|----------|----------|----------|
| 原生浏览器 | ✅ 支持 | 无需配置 |
| IFrame 嵌入 | ✅ 支持 | 需配置 CSP 域名白名单和渠道白名单 |
| WebView 容器 | ✅ 支持 | 需配置 User-Agent 白名单 |
| 移动端 SDK | ✅ 支持 | 参考 [APP SDK 接入方案](https://github.com/decard-tech/open-kyc-ios) |

> **重要提示**：接入前请告知您的访问类型，我们将为您进行相应的平台配置。

## 安全验证机制

为保障用户安全，DCS 对所有访问链接实施严格的安全校验。未通过验证的访问将被拒绝（显示 Page Not Found）。

### 验证方式

以下任一验证方式通过即可正常访问：

#### 方式一：User-Agent 关键词验证

**适用场景**：App 内 WebView 环境

**示例配置**：
```
AppleWebKit/537.36 (KHTML, like Gecko) Decard/0.0.1 Mobile Safari/537.36
```

**配置要求**：
- **接入方**：提供自定义的 User-Agent 关键词
- **平台方**：将关键词添加到白名单（如上例中的 "Decard"）

#### 方式二：User-Agent 完全匹配验证

**配置要求**：
- 接入方需确保 API 请求时的 User-Agent 与浏览器访问时的 User-Agent 完全一致
- 采用严格的字符串匹配（===）

#### 方式三：Referer 包含匹配验证

**配置要求**：
- 接入方需确保 API 请求时的 Referer 与页面访问时的 Referer 保持一致
- 采用包含匹配（include）方式验证

## 容器通信机制

为提升用户体验，DCS 在 IFrame 和 WebView 环境中使用 `postMessage` 机制与父容器通信。

### 通信协议

所有事件数据均以 JSON 字符串格式传递，包含以下字段：
- `name`：事件名称
- `payload`：事件参数

### 事件列表

#### DECARD_OPENAPI_CLOSE

**功能**：通知容器关闭 DCS 页面

**参数**：
- `null`：正常关闭
- `{ "type": "INTERNAL_ERROR_PAGE" }`：错误页面关闭

**触发场景**：
- KYC 流程完成后点击 Complete 按钮
- 卡片详情页 MFA 弹窗关闭
- 卡片详情页卡面信息弹窗关闭

**示例代码**：
```javascript
window.addEventListener('message', (event) => {
  const data = JSON.parse(event.data);
  if (data.name === 'DECARD_OPENAPI_CLOSE') {
    // 处理页面关闭逻辑
    closeWebView(); // 或 closeIframe()
  }
});
```

#### DECARD_OPENAPI_MFA_OPENED

**功能**：通知容器 MFA 验证弹窗已打开

**参数**：`null`

**触发场景**：卡片详情页 MFA 弹窗打开时

#### DECARD_OPENAPI_CARD_DETAIL_OPENED

**功能**：通知容器卡面详情弹窗已打开

**参数**：`null`

**触发场景**：卡片详情页卡面信息弹窗打开时

#### DECARD_OPENAPI_APPLY_PHYSICAL_CARD_TOP_UP

**功能**：实体卡申请充值提示

**参数**：`null`

**触发场景** 实体卡申请

## 错误处理

DCS 系统内置完善的错误处理机制，所有异常情况均由系统自动处理并展示相应提示。

### 错误码说明

| 错误码 | 描述 | 解决方案 |
|--------|------|----------|
| 10000 | 系统通用错误 | 请稍后重试或联系技术支持 |
| 10001 | 用户登录状态失效 | 请重新登录 |
| 10002 | 访问未授权页面 | 请通过正确的流程入口访问 |
| 10003 | User-Agent 未在白名单中 | 请联系技术支持配置白名单 |
| 10004 | API Secret 无效或已过期 | 请检查 Secret 配置或联系技术支持 |

## 重要注意事项

### Safari 浏览器限制

⚠️ **Cookie 传递限制**
- Safari 的安全策略限制了跨域 Cookie 传递
- 即使设置 `sameSite` 属性也无法解决此问题
- **建议**：Safari 环境下避免使用 IFrame 方案，推荐使用新窗口或当前页面跳转

⚠️ **弹窗阻止机制**
- Safari 会阻止异步调用的 `window.open()`
- **建议**：使用当前页面跳转替代弹窗方案

### 最佳实践

1. **环境检测**：建议在集成前检测浏览器环境，针对不同浏览器采用最适合的集成方案
2. **降级方案**：为 Safari 等限制较多的浏览器准备降级方案
3. **用户体验**：优先考虑用户体验，选择最流畅的集成方式