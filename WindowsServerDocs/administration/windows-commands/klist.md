---
title: klist
description: 'Windows 命令主题 * * *- '
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
ms.openlocfilehash: 3b3d0591f9feb12782d0c77b6c786cfe17656ab2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831158"
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
|-lh|表示用户的本地唯一标识符 (LUID)，以十六进制格式表示的高部分。 如果既没有 – lh 或 – l i 存在，该命令默认为当前登录的用户的 LUID。|
|-li|表示用户的本地唯一标识符 (LUID)，以十六进制格式表示的低部分。 如果既没有 – lh 或 – l i 存在，该命令默认为当前登录的用户的 LUID。|
|tickets|列出当前已缓存的票证-授予的票证 (Tgt)，以及指定的登录会话的服务票证。 这是默认选项。|
|tgt|显示初始的 Kerberos tgt。|
|清除|使用此选项可删除指定的登录会话的所有票证。|
|会话|显示此计算机上的登录会话的列表。|
|kcd_cache|显示 Kerberos 约束委派缓存信息。|
|获取|允许您请求的票证向服务主体名称 (SPN) 指定的目标计算机。|
|add_bind|可以指定 Kerberos 身份验证的首选的域控制器。|
|query_bind|显示 Kerberos 已联系每个域的缓存的首选的域控制器的列表。|
|purge_bind|删除的已缓存的首选指定域的域控制器。|
|kdcoptions|显示在 RFC 4120 中指定的密钥分发中心 (KDC) 选项。|
|/?|此命令将显示的帮助。|

## <a name="remarks"></a>备注

中的成员身份**Domain Admins**，或等效身份是最低要求运行此命令的所有参数。

如果未提供参数，Klist 会检索当前登录用户的所有票证。

参数会显示以下信息：
-   **tickets**

    列出当前已缓存的服务，你已通过身份验证自登录票证。 显示所有缓存票证的以下属性：  
    -   LogonID:LUID
    -   客户端：客户端名称和域名的客户端的串联
    -   服务器：服务名称和服务的域名的串联
    -   KerbTicket 加密类型：用于加密的 Kerberos 票证加密类型
    -   票证的标志：Kerberos 票证标志
    -   开始时间：从该票证有效时间
    -   结束时间：票证将不再成为有效时间。 当超过此时间是票证时，它不再可用来向服务进行身份验证，或者用于续订
    -   续订时间：新的初始身份验证所需的时间
    -   会话密钥类型：使用会话密钥的加密算法
-   **tgt**

    列出了初始的 Kerberos TGT 和当前已缓存的票证的以下属性：  
    -   LogonID:以十六进制格式标识
    -   ServiceName: krbtgt
    -   TargetName \<SPN >: krbtgt
    -   DomainName:颁发的 TGT 的域的名称
    -   TargetDomainName:TGT 颁发给域
    -   AltTargetDomainName:TGT 颁发给域
    -   票证的标志：地址和目标的操作和类型
    -   会话密钥：密钥长度和加密算法
    -   开始时间：票证已请求的本地计算机时间
    -   结束时间：票证将不再成为有效的时间。 当超过此时间是票证时，它不再可向服务进行身份验证。
    -   RenewUntil:票证续订的截止时间
    -   TimeSkew:时间差异与密钥分发中心 (KDC)
    -   EncodedTicket:编码的票证
-   **purge**

    可以删除特定的票证。 清除票证销毁所有票证，已缓存，因此请谨慎使用此属性。 它可能会阻止您能够对资源进行身份验证。 如果发生这种情况，您将必须先注销，然后重新登录。  
    -   LogonID:以十六进制格式标识
-   **会话**

    可以列出并显示在此计算机上的所有登录会话的信息。  
    -   LogonID:如果指定，仅按照给定的值将显示登录会话。 如果未指定，显示此计算机上所有登录会话。
-   **kcd_cache**

    使用此选项可显示 Kerberos 约束委派高速缓存信息。  
    -   LogonID:如果指定，将按给定的值显示登录会话的缓存信息。 如果未指定，将显示当前用户的登录会话的缓存信息。
-   **get**

    可以请求到指定的 SPN 的目标的票证。  
    -   LogonID:如果指定，则通过使用通过给定的值的登录会话请求的票证。 如果未指定，则使用当前用户的登录会话请求的票证。
    -   kdcoptions:请求的票证使用给定的 KDC 选项
-   **add_bind**

    可以指定 Kerberos 身份验证的首选的域控制器。
-   **query_bind**

    可用于显示域的缓存，首选域控制器。
-   **purge_bind**

    允许您删除域的缓存，首选域控制器。
-   **kdcoptions**

    有关选项和及其说明的当前列表，请参阅[RFC 4120](http://www.ietf.org/rfc/rfc4120.txt)。

**其他注意事项**
-   Klist.exe 可在 Windows Server 2012 和 Windows 8 中，并且它不需要特殊安装。

## <a name="BKMK_Examples"></a>示例

1.  在诊断时处理事件 ID 27 时票证授予服务 (TGS) 将请求目标服务器的帐户不具有合适的密钥可以生成 Kerberos 票证。 Klist 可用于查询的 Kerberos 票证缓存，以确定是否任何票证的缺失，如果目标服务器或帐户位于错误，或者不支持的加密类型。  
    ```
    klist 
    ```  
    ```
    klist –li 0x3e7
    ```  
2.  时诊断错误并想要知道每个票证授予票证的登录会话在计算机上缓存的详细信息，可以使用 Klist 显示 TGT 信息。  
    ```
    klist tgt
    ```  
3.  如果无法建立连接并诊断可能要花费太长，可以清除 Kerberos 票证缓存，先注销，并再重新登录。  
    ```
    klist purge
    ```  
    ```
    klist purge –li 0x3e7
    ```  
4.  当你想要诊断用户或服务的登录会话时，可以使用以下命令来查找使用 LogonID 中其他 Klist 命令。  
    ```
    klist sessions
    ```  
5.  当你想要诊断 Kerberos 约束委派失败时，可以使用以下命令以查找时遇到的最后一个错误。  
    ```
    klist kcd_cache
    ```  
6.  当你想要诊断如果某个用户或服务可以获取票证到服务器时，可以使用此命令的特定 SPN 请求票证。  
    ```
    klist get host/%computername%
    ```  
7.  当诊断域控制器的复制问题，您通常需要以面向特定的域控制器的客户端计算机。 在这些情况下，可以使用以下命令以该特定的域控制器到客户端计算机为目标。  
    ```
    klist add_bind CONTOSO KDC.CONTOSO.COM
    
    ```  
    ```
    klist add_bind CONTOSO.COM KDC.CONTOSO.COM
    ```  
8.  若要查询哪些域控制器最近联系了此计算机，可以使用以下命令。  
    ```
    klist query_bind
    ```  
9.  当你想 Kerberos 重新发现域控制器时，你可以使用以下命令。 此外可以使用此命令以使用 klist add_bind 创建新的域控制器绑定之前刷新缓存。  
    ```
    klist purge_bind
    ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)