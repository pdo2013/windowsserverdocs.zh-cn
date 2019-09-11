---
title: klist
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8f574b65ec8c123379e1b02ee1571cc9f21fa1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867056"
---
# <a name="klist"></a>klist



显示当前缓存的 Kerberos 票证的列表。 此信息适用于 Windows Server 2012。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-lh|表示以十六进制表示的用户本地唯一标识符（LUID）的高部分。 如果不存在-lh 或– li，则该命令默认为当前登录的用户的 LUID。|
|-li|表示以十六进制表示的用户本地唯一标识符（LUID）的低部分。 如果不存在-lh 或– li，则该命令默认为当前登录的用户的 LUID。|
|赛|列出当前缓存的票证授予票证（Tgt）和指定的登录会话的服务票证。 这是默认选项。|
|tgt|显示初始 Kerberos TGT。|
|清除|允许您删除指定登录会话的所有票证。|
|会话|显示此计算机上的登录会话的列表。|
|kcd_cache|显示 Kerberos 约束委派缓存信息。|
|获取|允许您向由服务主体名称（SPN）指定的目标计算机请求票证。|
|add_bind|允许你指定用于 Kerberos 身份验证的首选域控制器。|
|query_bind|显示 Kerberos 已联系的每个域的缓存首选域控制器的列表。|
|purge_bind|为指定的域删除缓存的首选域控制器。|
|kdcoptions|显示 RFC 4120 中指定的密钥发行中心（KDC）选项。|
|/?|显示此命令的帮助。|

## <a name="remarks"></a>备注

**Domain Admins**中的成员身份或等效身份是运行此命令的所有参数所需的最低要求。

如果未提供任何参数，Klist 将检索当前已登录用户的所有票证。

参数显示以下信息：
-   **赛**

    列出自登录后对其进行身份验证的当前已缓存服务的票证。 显示所有缓存票证的以下属性：  
    -   LogonID:LUID
    -   客户端：客户端名称与客户端域名的连接
    -   服务器：服务名称和服务的域名的串联
    -   KerbTicket 加密类型：用于加密 Kerberos 票证的加密类型
    -   票证标志：Kerberos 票证标志
    -   开始时间：票证生效的时间
    -   结束时间：票证不再有效的时间。 当票证过期时，它无法再用于向服务进行身份验证或用于续订
    -   续订时间：需要新的初始身份验证的时间
    -   会话密钥类型：用于会话密钥的加密算法
-   **tgt**

    列出了当前缓存的票证的初始 Kerberos TGT 和以下属性：  
    -   LogonID:用十六进制标识
    -   ServiceName： krbtgt
    -   TargetName \<SPN >： krbtgt
    -   域名颁发 TGT 的域的名称
    -   TargetDomainName:将 TGT 颁发到的域
    -   AltTargetDomainName:将 TGT 颁发到的域
    -   票证标志：地址和目标操作和类型
    -   会话密钥：密钥长度和加密算法
    -   时间请求票证的本地计算机时间
    -   结束票证不再有效的时间。 当票证过期时，它无法再用于向服务进行身份验证。
    -   RenewUntil:票证续订截止时间
    -   TimeSkew:与密钥发行中心的时间差（KDC）
    -   EncodedTicket:编码票证
-   **清空**

    允许您删除特定的票证。 清除票证会销毁已缓存的所有票证，因此请谨慎使用此属性。 它可能会阻止你无法对资源进行身份验证。 如果发生这种情况，则必须注销并重新登录。  
    -   LogonID:用十六进制标识
-   **会话**

    允许列出和显示此计算机上所有登录会话的信息。  
    -   LogonID:如指定，则只按给定的值显示登录会话。 如果未指定，则显示此计算机上的所有登录会话。
-   **kcd_cache**

    允许您显示 Kerberos 约束委派缓存信息。  
    -   LogonID:如指定，则按给定的值显示登录会话的缓存信息。 如果未指定，则显示当前用户的登录会话的缓存信息。
-   **get**

    允许你向 SPN 指定的目标请求票证。  
    -   LogonID:如果已指定，则通过使用登录会话通过给定的值请求票证。 如果未指定，则使用当前用户的登录会话请求票证。
    -   kdcoptions:请求具有给定 KDC 选项的票证
-   **add_bind**

    允许你指定用于 Kerberos 身份验证的首选域控制器。
-   **query_bind**

    允许您显示域的缓存首选域控制器。
-   **purge_bind**

    允许删除域的缓存首选域控制器。
-   **kdcoptions**

    有关最新的选项列表及其说明，请参阅[RFC 4120](http://www.ietf.org/rfc/rfc4120.txt)。

**其他注意事项**
-   Klist 在 Windows Server 2012 和 Windows 8 中可用，无需特殊安装。

## <a name="BKMK_Examples"></a>示例

1. 当你在处理目标服务器的票证授予服务（TGS）请求时诊断事件 ID 27 时，该帐户没有合适的密钥来生成 Kerberos 票证。 你可以使用 Klist 来查询 Kerberos 票证缓存，以确定是否缺少任何票证、目标服务器或帐户是否出错，或者是否不支持加密类型。  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. 当你诊断错误，并且想要了解在计算机上缓存的用于登录会话的每个票证授予票证的详细信息时，可以使用 Klist 来显示 TGT 信息。  
   ```
   klist tgt
   ```  
3. 如果无法建立连接和诊断可能需要太长时间，则可以清除 Kerberos 票证缓存，注销，然后重新登录。  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. 如果要诊断用户或服务的登录会话，可以使用以下命令查找其他 Klist 命令中使用的 LogonID。  
   ```
   klist sessions
   ```  
5. 若要诊断 Kerberos 约束委派失败，可以使用以下命令来查找遇到的最后一个错误。  
   ```
   klist kcd_cache
   ```  
6. 如果要诊断用户或服务是否可以获取服务器的票证，则可以使用此命令请求特定 SPN 的票证。  
   ```
   klist get host/%computername%
   ```  
7. 当诊断域控制器间的复制问题时，通常需要客户端计算机以特定的域控制器为目标。 在这些情况下，可以使用以下命令将客户端计算机定向到该特定域控制器。  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. 若要查询最近与此计算机联系的域控制器，可以使用以下命令。  
   ```
   klist query_bind
   ```  
9. 如果希望 Kerberos 重新发现域控制器，可以使用以下命令。 此命令还可用于在使用 klist add_bind 创建新的域控制器绑定之前刷新缓存。  
   ```
   klist purge_bind
   ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)