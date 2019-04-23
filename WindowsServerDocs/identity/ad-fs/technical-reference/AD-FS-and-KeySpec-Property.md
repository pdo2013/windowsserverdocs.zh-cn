---
title: Active Directory 联合身份验证服务和证书密钥规范属性信息
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879308"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS 和证书 KeySpec 属性信息
密钥规范 ("KeySpec") 是与证书和密钥关联的属性。 它指定是否可以签名、 加密和 / 或使用与证书关联的私钥。   

例如，KeySpec 值不正确可能导致 AD FS 和 Web 应用程序代理错误：


- 无法建立与 AD FS 或 Web 应用程序代理的 SSL/TLS 连接与记录 （尽管可能记录 SChannel 36888 和 36874 事件） 的任何 AD FS 事件
- 与 AD FS 或 WAP 上的登录名失败窗体的页上显示任何错误消息的基于的身份验证页。

您可能会看到事件日志中：

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

## <a name="what-causes-the-problem"></a>哪些因素会导致问题
KeySpec 属性标识的密钥生成或检索由 Microsoft CryptoAPI (CAPI)，从 Microsoft 旧加密存储提供程序 (CSP)，可以使用方式。

KeySpec 值**1**，或**AT_KEYEXCHANGE**，可以用于签名和加密。  值为**2**，或**AT_SIGNATURE**，只用于签名。

最常见的 KeySpec 错误配置的证书以外的令牌签名证书的证书使用值为 2。  

对于使用 Cryptography Next Generation (CNG) 提供程序生成其密钥的证书，没有密钥规范的概念和 KeySpec 值将始终为零。

了解如何以检查有效 KeySpec 值以下。 

### <a name="example"></a>示例
旧 CSP 的一个示例是 Microsoft Enhanced Cryptographic Provider。 

Microsoft RSA CSP 密钥 blob 格式包括算法标识符，或者**CALG_RSA_KEYX**或**CALG_RSA_SIGN**分别为请求提供服务 * * AT_KEYEXCHANGE * * 或**AT_签名**密钥。
  
RSA 密钥算法标识符映射到 KeySpec 值，如下所示

| 提供程序支持算法| CAPI 调用密钥规范值 |
| --- | --- |
|CALG_RSA_KEYX:可用于签名和解密的 RSA 密钥| AT_KEYEXCHANGE (或 KeySpec = 1)|
CALG_RSA_SIGN:RSA 签名密钥 |AT_SIGNATURE (或 KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和关联的含义
以下是各种 KeySpec 值的含义：

|Keyspec 值|方法|建议的用法，AD FS|
| --- | --- | --- |
|0|该证书是 CNG 证书|仅 SSL 证书|
|1|旧的 CAPI (非 CNG) 证书，该密钥可用于签名和解密|    SSL，令牌签名、 令牌解密，服务通信证书|
|2|旧 CAPI (非 CNG) 证书，该密钥可以是仅用于签名|不建议这样做|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>如何检查 KeySpec 值以查找您的证书 / 密钥
若要查看可以使用的证书值**certutil**命令行工具。  

以下是一个示例： **certutil – v – 存储我**。  这将转储到屏幕的证书信息。

![Keyspec 证书](media/AD-FS-and-KeySpec-Property/keyspec1.png)

下 CERT_KEY_PROV_INFO_PROP_ID 寻找两件事：


1. **提供程序类型：** 这表示是否将证书使用旧的加密存储提供程序 (CSP) 或密钥存储提供程序基于上较新证书 Next Generation (CNG) Api。  任何非零值指示旧的提供程序。
2.  **KeySpec:** 以下是有效的 AD FS 证书 KeySpec 值：

    旧 CSP 提供程序 （不等于 0 提供程序类型）：
    
    |AD FS 证书用途|有效 KeySpec 值|
    | --- | --- |
    |服务通信|1|
    |令牌解密|1|
    |令牌签名|1 和 2|
    |SSL|1|

    CNG 提供程序 (提供程序类型 = 0):
    |AD FS 证书用途|有效 KeySpec 值|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何为支持的值更改为证书 keyspec
更改 KeySpec 值不需要重新生成或重新颁发的证书颁发机构的证书。  可以通过重新导入完整的证书和私钥的 PFX 文件中使用以下步骤在证书存储区更改 KeySpec:


1. 首先，查看并记录现有证书的专用密钥权限，以便可以将其重新配置如有必要重新导入之后。
2. 导出证书包括私钥的 PFX 文件。
3. 执行以下步骤为每个 AD FS 和 WAP 服务器
    1. 删除证书 (从 AD FS / WAP 服务器)
    2. 打开提升的 PowerShell 命令提示符并使用以下 cmdlet 语法，AT_KEYEXCHANGE 值 （这适用于所有 AD FS 证书目的） 指定每个 AD FS 和 WAP 服务器上的将 PFX 文件导入：
        1. C:\>certutil-importpfx certfile.pfx AT_KEYEXCHANGE
        2. 输入 PFX 密码
    3. 上述完成后，请执行以下
        1. 检查私有密钥权限
        2. 重新启动 adfs 或 wap 服务





