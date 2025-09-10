# Frontend Integration Guide

## Supported Integration Methods

DCS pages support multiple integration methods to meet different scenario requirements:

| Integration Method | Support Status | Configuration Requirements |
|-------------------|----------------|---------------------------|
| Native Browser | ✅ Supported | No configuration required |
| IFrame Embedding | ✅ Supported | CSP domain whitelist and channel whitelist required |
| WebView Container | ✅ Supported | User-Agent whitelist required |
| Mobile SDK | ✅ Supported | Refer to [APP SDK Integration Guide](https://github.com/decard-tech/open-kyc-ios) |

> **Important Note**: Please inform us of your access type before integration, and we will configure the platform accordingly.

## Security Verification Mechanism

To ensure user security, DCS implements strict security verification for all access links. Access that fails verification will be rejected (displaying Page Not Found).

### Verification Methods

Any of the following verification methods can be used for normal access:

#### Method 1: User-Agent Keyword Verification

**Applicable Scenario**: App WebView environment

**Example Configuration**:
```
AppleWebKit/537.36 (KHTML, like Gecko) Decard/0.0.1 Mobile Safari/537.36
```

**Configuration Requirements**:
- **Integrator**: Provide custom User-Agent keywords
- **Platform**: Add keywords to whitelist (e.g., "Decard" in the example above)

#### Method 2: User-Agent Exact Match Verification

**Configuration Requirements**:
- Integrator must ensure the User-Agent in API requests matches exactly with the User-Agent in browser access
- Uses strict string matching (===)

#### Method 3: Referer Inclusion Match Verification

**Configuration Requirements**:
- Integrator must ensure the Referer in API requests is consistent with the Referer during page access
- Uses inclusion matching (include) for verification

## Container Communication Mechanism

To enhance user experience, DCS uses the `postMessage` mechanism to communicate with parent containers in IFrame and WebView environments.

### Communication Protocol

All event data is passed as JSON strings, containing the following fields:
- `name`: Event name
- `payload`: Event parameters

### Event List

#### DECARD_OPENAPI_CLOSE

**Function**: Notify container to close DCS page

**Parameters**:
- `null`: Normal closure
- `{ "type": "INTERNAL_ERROR_PAGE" }`: Error page closure

**Trigger Scenarios**:
- Clicking Complete button after KYC process completion
- Closing MFA popup in card details flow
- Closing card face popup in card details flow

**Example Code**:
```javascript
window.addEventListener('message', (event) => {
  const data = JSON.parse(event.data);
  if (data.name === 'DECARD_OPENAPI_CLOSE') {
    // Handle page closure logic
    closeWebView(); // or closeIframe()
  }
});
```

#### DECARD_OPENAPI_MFA_OPENED

**Function**: Notify container that MFA verification popup has opened

**Parameters**: `null`

**Trigger Scenario**: When MFA popup opens in card details page

#### DECARD_OPENAPI_CARD_DETAIL_OPENED

**Function**: Notify container that card face details popup has opened

**Parameters**: `null`

**Trigger Scenario**: When card face information popup opens in card details page

## Error Handling

DCS system has built-in comprehensive error handling mechanisms. All exceptions are automatically handled by the system and display appropriate prompts.

### Error Code Description

| Error Code | Description | Solution |
|------------|-------------|----------|
| 10000 | System general error | Please retry later or contact technical support |
| 10001 | User login status expired | Please log in again |
| 10002 | Access to unauthorized page | Please access through the correct process entry |
| 10003 | User-Agent not in whitelist | Please contact technical support to configure whitelist |
| 10004 | API Secret invalid or expired | Please check Secret configuration or contact technical support |

## Important Considerations

### Safari Browser Limitations

⚠️ **Cookie Transfer Limitations**
- Safari's security policy restricts cross-domain Cookie transfer
- Setting `sameSite` attribute cannot resolve this issue
- **Recommendation**: Avoid using IFrame solution in Safari environment, recommend using new window or current page navigation

⚠️ **Popup Blocking Mechanism**
- Safari blocks asynchronously called `window.open()`
- **Recommendation**: Use current page navigation instead of popup solutions

### Best Practices

1. **Environment Detection**: Recommend detecting browser environment before integration and adopting the most suitable integration solution for different browsers
2. **Fallback Solutions**: Prepare fallback solutions for browsers with more restrictions like Safari
3. **User Experience**: Prioritize user experience and choose the smoothest integration method