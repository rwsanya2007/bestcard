
# 绑定信用卡交易指南与 API 详解

本文为您详细介绍如何通过绿界（ECPay）的 API 实现绑定信用卡交易操作，包括使用场景、API 请求和响应参数解析以及注意事项，帮助开发者快速上手。

---

## 推荐工具：WildCard 虚拟信用卡

**WildCard | 一分钟注册，轻松订阅海外服务**：[https://bit.ly/bewildcard](https://bit.ly/bewildcard)  
- **支持多平台**：支持 ChatGPT、Google Play、PayPal 等海外订阅平台。  
- **简单便捷**：支持微信、支付宝开通支付。  
- **专属优惠**：使用邀请码 **ACCPAY**，享受 0 手续费优惠，并减免开卡费用！

---

## 一、应用场景

商户需要将从商户 Web 端获取的 `BindCardPayToken` 传输至绿界服务器，以完成绑定信用卡交易的创建。此功能广泛适用于在线支付、会员卡绑定等多种业务场景。

> **注意事项**：  
该 API 需与银行建立实时连接，可能因银行网络不稳定导致响应时间延长。建议设置 API 超时时间为 **30 秒以上**。

---

## 二、HTTPS 传输协议

- **Content Type**：`application/json`  
- **HTTP Method**：`POST`  

---

## 三、特店请求参数示例（JSON 格式）

```json
{
    "MerchantID": "3002607",
    "RqHeader": {
        "Timestamp": 1234567890
    },
    "Data": "enter your data"
}
```

---

## 四、Data 参数示例（JSON 格式）

```json
{
    "MerchantID": "3002607",
    "BindCardPayToken": "enter your token",
    "MerchantMemberID": "test123456789"
}
```

---

## 五、绿界响应参数示例（JSON 格式）

```json
{
    "MerchantID": "3002607",
    "RpHeader": {
        "Timestamp": 1234564848
    },
    "TransCode": 1,
    "TransMsg": "Success",
    "Data": "..."
}
```

### Data 参数详细内容

```json
{
    "RtnCode": 1,
    "RtnMsg": "Success",
    "PlatformID": "1234567890",
    "MerchantID": "1234567890",
    "OrderInfo": {
        "MerchantTradeNo": "test123466"
    },
    "ThreeDInfo": {
        "ThreeDURL": "https://3durl.com.tw"
    }
}
```

- **RtnCode**（Int）：交易状态。  
  - `1` 表示成功，其他代码为失败，失败代码参考[交易信息代码表](https://developers.ecpay.com.tw/?p=35681)。  
- **RtnMsg**（String）：响应信息。  
- **MerchantID**（String）：商户编号。  
- **MerchantMemberID**（String）：商户会员编号。  
- **BindCardID**（String）：绑定信用卡代码。  
- **OrderInfo**（Object）：包含订单相关信息。  

---

## 六、信用卡信息返回字段

- **CardInfo**（Object）：信用卡授权信息  
  - **Card6No**：信用卡前六位号码。  
  - **Card4No**：信用卡后四位号码。  
  - **CardValidYY**：信用卡有效年份（YY）。  
  - **CardValidMM**：信用卡有效月份（MM）。  
  - **AuthCode**：银行授权码。  
  - **Amount**：交易金额。  
  - **Eci**：3D 验证值（`5,6,2,1` 表示 3D 交易）。  

---

## 七、注意事项

1. 如果返回以下代码，建议与用户确认信用卡状态：
   - `RtnCode = 10100252`：信用卡余额不足。  
   - `RtnCode = 10100255`：信用卡已挂失。  
2. 返回 `null` 或空字符串的绑定卡数据，表示该订单的绑定卡数据已失效，但不影响交易授权结果。  
3. 请保存绿界交易编号（`TradeNo`）与商户交易编号（`MerchantTradeNo`）的关联。

---

## 八、总结

通过绿界 API 实现绑定信用卡交易功能，能够有效提升支付流程的安全性和用户体验。结合如 **WildCard** 等工具，为用户提供更便捷的虚拟信用卡解决方案。
