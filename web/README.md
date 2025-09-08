## Frontend Access Specification

### Supported Platforms

DCS pages support the following access methods:

| Access Type    | Supported | Configuration                                                                 |
|-----------------|-----------|-------------------------------------------------------------------------------|
| Native Browser | Yes       | None                                                                         |
| IFrame         | Yes       | CSP domain whitelist, channel whitelist                                      |
| WebView        | Yes       | User-Agent whitelist                                                         |
| APP SDK     | Yes       | [APP SDK](https://github.com/decard-tech/open-kyc-ios)   |

> The access party must inform the platform of the access type, and the platform will handle the corresponding configuration.

---

### Notes on Link Generation

To ensure user safety, all initial links accessing DCS will undergo security validation. Access will be restricted (Page Not Found) if validation fails. Access will be granted if any of the following conditions are met:

#### 1. Validating keywords in the `user-Agent` header (applicable only in WebView scenarios)

```code
AppleWebKit/537.36 (KHTML, like Gecko) Decard/0.0.1 Mobile Safari/537.36
```
If the `user-Agent` string matches the keyword `Decard`, access will be granted.

**Responsibility:**
- **Access Party**:
  - Provide a customized `user-Agent` keyword.
- **Platform (DCS)**:
  - Add the keyword to the whitelist on the DCS platform.

#### 2. Full validation of the `user-Agent` in the browser header against the `user-Agent` provided in the access party's API request

**Responsibility:**
- **Access Party**:
  - Ensure the `user-Agent` in the API request and the browser's `user-Agent` are identical.

#### 3. Validating the `referer` in the browser header against the `referer` provided in the access party's API request (include check)

**Responsibility:**
- **Access Party**:
  - Ensure the `referer` in the API request and the `referer` when accessing the DCS page are consistent.

---

### Platform Communication

To enhance user experience in IFrame and WebView scenarios, specific interactions use the `postMessage` mechanism to notify the parent container. WebView and IFrame containers can listen to these events and respond accordingly.

| Event Name                   | Description                                     | Data Format                                                                     |
|------------------------------|-------------------------------------------------|---------------------------------------------------------------------------------|
| `DECARD_OPENAPI_CLOSE`       | Close the page                                 | `{ "name": "DECARD_OPENAPI_CLOSE", "payload": null }`                       |
| `DECARD_OPENAPI_MFA_OPENED`  | Triggered when the OTP verification popup is activated | `{ "name": "DECARD_OPENAPI_MFA_OPENED", "payload": null }`                |
| `DECARD_OPENAPI_CARD_DETAIL_OPENED` | Triggered when card details are activated      | `{ "name": "DECARD_OPENAPI_CARD_DETAIL_OPENED", "payload": null }`          |

---

### Error Handling and Redirection

All error scenarios in DCS are handled automatically.

| Code  | Description                                |
|-------|--------------------------------------------|
| 10000 | General error                              |
| 10001 | Login state lost                           |
| 10002 | Access to non-process pages is prohibited  |
| 10003 | Not in the `user-Agent` whitelist          |
| 10004 | Secret is unavailable                      |

---

### ⚠️ Notes

- Due to Safari's security policy limitations, cookies with the `sameSite` attribute cannot be passed even if set. It is recommended not to use IFrame solutions in Safari.
- Similarly, asynchronous `window.open` will be blocked. It is recommended to open new links based on the current page.
