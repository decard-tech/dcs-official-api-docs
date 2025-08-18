### 逻辑交互流程

![](../images/flow.png)
---

### 最佳实践

```mermaid
sequenceDiagram
    participant User
    participant Institution
    participant Decard_OPENAPI
    participant Websocket_S3

    Note over User,Institution:  卡片申请流程
    User->>Institution:  Apply for a card
    activate Institution
    Institution->>Decard_OPENAPI:  /captcha/v1/send-mobile-code
    Decard_OPENAPI-->>Institution: send SMS code
    Institution-->>User: SMS code
    deactivate Institution

    User->>Institution: Register /account/v1/register
    activate Institution
    Institution->>Decard_OPENAPI: (Relay)
    Decard_OPENAPI-->>Institution: Response externaluserid
    Institution-->>User: (Relay)
    deactivate Institution

    User->>Institution: KYC
    activate Institution
    Institution->>Decard_OPENAPI: /redirect/v1/guidance-link
    Decard_OPENAPI-->>Institution: KYC-URL
    Institution-->>User: Response KYC-URL
    User->>Websocket_S3: KYC application
    Websocket_S3->>Institution: KYC Notification
    Institution->>Decard_OPENAPI: /account/v1/kyc-status
    Decard_OPENAPI-->>Institution: Response
    deactivate Institution

    Note over User,Institution: 查看卡片信息
    User->>Institution: Check Card info
    activate Institution
    Institution->>Decard_OPENAPI: /redirect/v1/guidance-link
    Decard_OPENAPI-->>Institution: Cardinfo URL
    Institution-->>User: Response Cardinfo URL
    deactivate Institution

    Note over User,Institution: 卡片管理
    User->>Institution: Freeze card
    activate Institution
    Institution->>Decard_OPENAPI: /card/v1/block
    Decard_OPENAPI-->>Institution: Response
    Institution-->>User: (Relay)
    deactivate Institution

    User->>Institution: Unfreeze request
    activate Institution
    Institution->>Decard_OPENAPI: /captcha/v1/send-mobile-code
    Decard_OPENAPI-->>Institution: send SMS code
    Institution-->>User: SMS code delivery
    User->>Institution: UnFreeze card (with code)
    Institution->>Decard_OPENAPI: /card/v1/block
    Decard_OPENAPI-->>Institution: Response
    Institution-->>User: (Relay)
    deactivate Institution


    Note over User,Institution: DPT模式，fomo充值流程
    activate Institution
    User->>Institution: sumbit KYC info
    Institution->>Decard_OPENAPI: /account/v1/update-travel-rule
    Decard_OPENAPI-->>Institution: (Relay)    
    Institution-->>User: (Relay)    
    User->>Institution: get deposit address
    Institution->>Decard_OPENAPI: /crypto/v2/deposit-address
    Decard_OPENAPI-->>Institution: (Relay)    
    Institution-->>User: (Relay)  
    User-->>User: deposit by chain
    User->>Institution: query deposit record
    Institution->>Decard_OPENAPI:  /card/v1/fiat/transactions
    Decard_OPENAPI-->>Institution: (Relay)    
    Institution-->>User: (Relay)    
    deactivate Institution
```

### 接口交互流程

#### AKSK定义

`ApiKey` 作为一种全局唯一的标识符，方便用户身份识别以及数据分析。为防止恶意使用别人的`ApiKey`来发起请求，会采用配对 `SecretKey` 的方式

AKSK通常会组合生成一套签名，并按照一定规则进行加密处理。在请求方发起请求时，需要将这个签名值一并提交给提供方进行验证。

我们把AKSK私下交付三方平台，三方平台对OPENAPI的任何接口调用都应该遵循约定的签名协议。

#### 签名算法

使用 `HmacSHA256` 算法生成签名，签名计算规则如下：

```
sign(API-KEY + TIMESTAMP + NONCE + payload)
```

- **payload**

```
if method is GET : 
        payload = original.url().encodedQuery();
    else :
        payload = Body;
```

- **TIMESTAMP:** 主要可用来防止同一个请求参数被无限期的使用，默认只处理5s内的有效请求
- **NONCE:** 由接口请求方生成的随机数，可实现请求一次性有效，避免接口重放攻击

#### 敏感信息加密算法

对于在传输过程有较高安全考量的团队，可以对用户的个人隐私信息采用AES加密算法进行传递。

```java
import cn.hutool.core.codec.Base64;
import cn.hutool.crypto.SecureUtil;
import org.junit.jupiter.api.Test;

public class AES {

    private static final String SECRET = "sjC951Crc3WxH3YXqnJ43v/miI8iCan2iQbK7Km0w1s=";
    
    public static void main(String[] args) {
        String data = "13866668888";
        System.out.println("ORIGIN: " + data);

        String encryptData = SecureUtil.aes(Base64.decode(SECRET)).encryptBase64(data);
        System.out.println("ENCRYPT: " + encryptData);

        String decryptData = SecureUtil.aes(Base64.decode(SECRET)).decryptStr(encryptData);
        System.out.println("DECRYPT: " + decryptData);
    }
}
```

