---
title: "Active Directory 联合身份验证服务和证书键规范属性信息"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>广告金融服务和证书 KeySpec 属性信息
键规格 ("KeySpec") 是属性证书和码关联。 它指定是否关联的证书专用键可用于登录、加密或两者都。   

不正确的 KeySpec 值如可能会导致广告金融服务和 Web 应用程序代理错误：


- 但无法与记录（尽管可能记录 SChannel 36888 和 36874 事件）的任何广告 FS 事件建立 SSL 月 TLS 连接到 FS 广告或者 Web 应用程序代理，
- 但无法登录的广告 FS 或 WAP 任何页面上显示的错误消息形成根据身份验证页面。

你可能会看到以下事件日志中：

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>导致该问题的原因
KeySpec 属性标识如何使用检索由 Microsoft CryptoAPI (CAPI)，从 Microsoft 旧版加密存储提供商 (CSP)，或生成一个键。

KeySpec 值为**1**，或**AT_KEYEXCHANGE**，可用于签名和加密。  值**2**，或**AT_SIGNATURE**，仅用于登录。

最常见的 KeySpec 错误配置的令牌签名证书以外的证书使用值 2。  

对于使用加密技术下一代 (CNG) 提供商生成其键证书，没有密钥规范的概念并且 KeySpec 值始终会为零。

了解如何检查以下有效 KeySpec 值。 

### <a name="example"></a>示例
旧版 CSP 的一个示例是 Microsoft 增强加密提供。 

MicrosoftRSA CSP 关键斑点格式包含算法标识符，或者**CALG_RSA_KEYX**或**CALG_RSA_SIGN**，分别为可以请求提供服务 ** AT_KEYEXCHANGE ** 或**AT_SIGNATURE**键。
  
RSA 密钥算法标识符，如下所示映射到 KeySpec 值

| 受支持的提供商算法| 键指标值 CAPI 呼叫 |
| --- | --- |
|可用于登录和解密 CALG_RSA_KEYX: RSA 键| AT_KEYEXCHANGE (或 KeySpec = 1)|
仅关键 CALG_RSA_SIGN: RSA 签名 |AT_SIGNATURE (或 KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和关联的含义
以下是在各个 KeySpec 值的含义：

|Keyspec 值|意味着|推荐的广告 FS 使用|
| --- | --- | --- |
|0|证书已 CNG 证书|仅 SSL 证书|
|1|有关旧版的 CAPI (非 CNG) 证书，该密钥可用于登录和解密|    SSL，令牌签名，令牌进行解密，服务通信证书|
|2|有关旧版的 CAPI (非 CNG) 证书，键可以仅用于登录|不建议这样做|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>如何为你的证书检查 KeySpec / 键
若要查看你可以使用证书值**certutil**命令行工具。  

以下是一个示例：**certutil-v – 存储我**。  这将转储到屏幕的证书信息。

![Keyspec 证书](media/AD-FS-and-KeySpec-Property/keyspec1.png)

下 CERT_KEY_PROV_INFO_PROP_ID 查找两项操作：


1. **提供程序类型：**这表示证书使用传统的加密存储提供商 (CSP)，还是键存储提供商基于在较新证书下一代 (CNG) Api。  任何非零值表示旧版提供商。
2.  **KeySpec:**以下是有关广告 FS 证书有效 KeySpec 值：

    旧版 CSP 提供商（不等于 0 提供程序类型）：
    
    |广告 FS 证书目的|有效的 KeySpec 值|
    | --- | --- |
    |服务通信|1|
    |标记解密|1|
    |令牌签名|1 和 2|
    |SSL|1|

    CNG 提供商 (提供程序类型 = 0):
    |广告 FS 证书目的|有效的 KeySpec 值|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何更改你的证书 keyspec 到受支持的值
更改 KeySpec 值不需要的证书，才能重新证书颁发机构颁发或重新生成。  可以更改 KeySpec 重新导入使用下面的步骤证书官方商城的完整证书和 PFX 文件中的专用密钥：


1. 首先，检查并录制现有证书上的专用关键权限，以便他们可以重新配置如有必要后重新导入。
2. 导出证书包括专用关键 PFX 文件。
3. 对于每个广告金融服务和 WAP 服务器执行以下步骤
    1. 删除证书 (从广告 FS / WAP 服务器)
    2. 打开提升了权限的命令提示符 PowerShell 和导入的每个广告金融服务和 WAP 服务器使用下面的 cmdlet 语法、指定 AT_KEYEXCHANGE 值（这适用于所有广告 FS 证书目的）上 PFX 文件：
        1. C:\ > certutil-importpfx certfile.pfx AT_KEYEXCHANGE
        2. 输入 PFX 密码
    3. 上述完成后，请执行以下
        1. 检查专用项的权限
        2. 重新启动 adfs 或 wap 服务





