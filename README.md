
## Overview

![](/images/deCard.jpeg)

This API package is meant to provide you, the developer, with a set of tools to help you easily and quickly tap into the core capabilities of DeCard, achieving PayFi
- Account
- Card
- Wallet

You can access the relevant features through the following domain names

| Mainnet | https://api.thedecard.com         |
|---------|-----------------------------------|
| Testnet | https://api.testnet.thedecard.com |


## Documentation
Please refer to our extensive for more information.
### [Account](./account/readme.md)
### [Card](./card/readme.md)
### [Wallet](./wallet/readme.md)
### [Crypto](./crypto/readme.md)




# AKSK
`ApiKey` 作为一种全局唯一的标识符，其作用主要在于方便用户身份识别以及数据分析等方面。为了防止其他用户通过恶意使用别人的`ApiKey`来发起请求，一般都会采用配对 `SecretKey` 的方式，类似于一种密码。

AKSK通常会组合生成一套签名，并按照一定规则进行加密处理。在请求方发起请求时，需要将这个签名值一并提交给提供方进行验证。 如果签名验证通过，则可以进行数据交互，否则将被拒绝。这种机制能够保证数据的安全性和准确性。

我们把AKSK私下交付三方平台，三方平台对OPENAPI的任何接口调用都应该遵循约定的签名协议。


# 签名算法
使用 `HmacSHA256` 算法生成签名，签名计算规则如下：
```
sign(API-KEY + TIMESTAMP + NONCE + payload)
```

- payload
  ```
    GET
        payload = original.url().encodedQuery();
    ELSE
        payload = Body;
  ```

- TIMESTAMP
  
  该参数的设定主要可以用来防止同一个请求参数被无限期的使用，默认只处理5s内的有效请求。

- NONCE

  该值是一个由接口请求方生成的随机数，在有需要的场景中，可以用它来实现请求一次性有效，也就是说同样的请求参数只能使用一次，这样可以避免接口重放攻击。








