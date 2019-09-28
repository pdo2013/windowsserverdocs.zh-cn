---
title: Active Directory 联合身份验证服务和证书密钥规范属性信息
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 51c9828cfe494c68422f4985e5b17113020c8414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407424"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS 和 certificate KeySpec 属性信息
密钥规范（"KeySpec"）是与证书和密钥关联的属性。 它指定与证书关联的私钥是否可用于签名和/或加密。   

不正确的 KeySpec 值可能会导致 AD FS 和 Web 应用程序代理错误，如：


- 无法建立到 AD FS 或 Web 应用程序代理的 SSL/TLS 连接，未记录任何 AD FS 事件（尽管可能会记录 SChannel 36888 和36874事件）
- 在 AD FS 或 WAP 窗体的身份验证页上登录失败，页面上没有显示错误消息。

你可能会在事件日志中看到以下内容：

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

## <a name="what-causes-the-problem"></a>导致问题的原因
KeySpec 属性标识如何使用 microsoft 旧版加密存储提供程序（CSP）中由 Microsoft CryptoAPI （CAPI）生成或检索到的密钥。

KeySpec 值**1**或**AT_KEYEXCHANGE**可用于签名和加密。  值**2**或**AT_SIGNATURE**仅用于签名。

最常见的 KeySpec 错误配置是对证书（而不是令牌签名证书）使用值2。  

对于其密钥是使用下一代加密技术（CNG）提供程序生成的证书，没有密钥规范的概念，KeySpec 值将始终为零。

请参阅下面的如何检查有效的 KeySpec 值。 

### <a name="example"></a>示例
Microsoft 增强的加密提供程序就是一个旧 CSP 的示例。 

Microsoft RSA CSP 密钥 blob 格式包括一个算法标识符，分别是**CALG_RSA_KEYX**或**CALG_RSA_SIGN**，用于为<strong>AT_KEYEXCHANGE * * 或 * * AT_SIGNATURE</strong>密钥的请求提供服务。

RSA 密钥算法标识符映射到 KeySpec 值，如下所示

| 提供程序支持的算法| CAPI 调用的密钥规范值 |
| --- | --- |
|CALG_RSA_KEYX :可用于签名和解密的 RSA 密钥| AT_KEYEXCHANGE （或 KeySpec = 1）|
CALG_RSA_SIGN :RSA 仅签名密钥 |AT_SIGNATURE （或 KeySpec = 2）|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和关联的含义
以下是各种 KeySpec 值的含义：

|Keyspec 值|也就是说|建议 AD FS 使用|
| --- | --- | --- |
|0|证书是 CNG 证书|仅适用于 SSL 证书|
|1|对于旧版 CAPI （非 CNG）证书，密钥可用于签名和解密|    SSL、令牌签名、令牌解密和服务通信证书|
|2|对于旧版 CAPI （非 CNG）证书，密钥只能用于签名|不建议|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>如何检查证书/密钥的 KeySpec 值
若要查看证书值，可以使用**certutil**命令行工具。  

下面是一个示例： **certutil – v – store my**。  这会将证书信息转储到屏幕。

![Keyspec 证书](media/AD-FS-and-KeySpec-Property/keyspec1.png)

在 CERT_KEY_PROV_INFO_PROP_ID 下，查找以下两项内容：


1. **ProviderType：** 这表示证书是使用旧的加密存储提供程序（CSP）还是基于较新的证书下一代（CNG） Api 的密钥存储提供程序。  任何非零值都表示旧提供程序。
2. **KeySpec**下面是 AD FS 证书的有效 KeySpec 值：

   旧 CSP 提供程序（ProviderType 不等于0）：

   |AD FS 证书目的|有效的 KeySpec 值|
   | --- | --- |
   |服务通信|1|
   |令牌解密|1|
   |令牌签名|1和2|
   |SSL|1|

   CNG 提供程序（ProviderType = 0）：

   |AD FS 证书目的|有效的 KeySpec 值|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何将证书的 keyspec 更改为支持的值
更改 KeySpec 值不需要证书颁发机构重新生成或重新颁发证书。  可以通过将完整的证书和私钥从 PFX 文件重新导入到证书存储中，使用以下步骤更改 KeySpec：


1. 首先，检查并记录现有证书上的私钥权限，以便在重新导入后需要重新配置这些权限。
2. 将包含私钥的证书导出到 PFX 文件。
3. 针对每个 AD FS 和 WAP 服务器执行以下步骤
    1. 删除证书（从 AD FS/WAP 服务器）
    2. 使用下面的 cmdlet 语法打开提升的 PowerShell 命令提示符，并在每个 AD FS 和 WAP 服务器上导入 PFX 文件，并指定 AT_KEYEXCHANGE 值（适用于所有 AD FS 证书目的）：
        1. C @no__t：0certutil – importpfx certfile AT_KEYEXCHANGE
        2. 输入 PFX 密码
    3. 完成上述操作后，请执行以下操作
        1. 检查私钥权限
        2. 重新启动 adfs 或 wap 服务





