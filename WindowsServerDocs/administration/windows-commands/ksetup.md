---
title: ksetup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868838"
---
# <a name="ksetup"></a>ksetup



执行与设置和维护 Kerberos 协议和密钥分发中心 (KDC) 以支持也不是 Windows 域的 Kerberos 领域相关的任务。 有关如何使用此命令的示例，请参阅每个相关的副主题中的示例部分。

## <a name="syntax"></a>语法

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|使此计算机的 Kerberos 领域的成员。|
|[Ksetup:mapuser](ksetup-mapuser.md)|将 Kerberos 主体映射到帐户。|
|[Ksetup:addkdc](ksetup-addkdc.md)|定义给定领域的 KDC 条目。|
|[Ksetup:delkdc](ksetup-delkdc.md)|删除领域的 KDC 条目。|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|将添加一个领域 Kpasswd 服务器地址。|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|删除领域 Kpasswd 服务器地址。|
|[Ksetup:server](ksetup-server.md)|可以指定要应用所做的更改的 Windows 计算机的名称。|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|设置计算机的域帐户 （或主机主体） 的密码。|
|[Ksetup:removerealm](ksetup-removerealm.md)|从注册表中删除指定的领域的所有信息。|
|[Ksetup:domain](ksetup-domain.md)|允许您指定的域 (如果\<DomainName > 不通过设置 **/domain**)。|
|[Ksetup:changepassword](ksetup-changepassword.md)|可以使用 Kpasswd 更改已登录用户的密码。|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|列出了可用领域标志**ksetup**可以检测到。|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|设置特定领域的领域标志。|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|将其他领域标志添加到一个领域。|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|从领域中删除领域标志。|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|分析给定计算机上的 Kerberos 配置。 将领域映射到的主机添加到注册表。|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|添加注册表值将映射到 Kerberos 领域的主机。|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|删除映射到 Kerberos 领域的主机计算机的注册表值。|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|设置一个或多个加密类型的域信任属性。|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|获取域的加密类型信任属性。|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|将加密类型添加到域的加密类型信任属性。|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|删除域的加密类型信任属性。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

**Ksetup**用于更改计算机设置，以查找 Kerberos 领域。 在基于非 Microsoft Kerberos 实现中，此信息通常保留 Krb5.conf 文件中。 在 Windows Server 操作系统，它保存在注册表中。 此工具可用于修改这些设置。 由工作站到 Kerberos 领域和域控制器，以找到跨领域信任关系的 Kerberos 领域，将使用这些设置。

**Ksetup**初始化 Kerberos 安全支持提供程序 (SSP) 用来定位 Kerberos 领域的 KDC，如果计算机运行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2，并且不是成员的 Windows 注册表项域。 完成配置后，运行 Windows 操作系统可以登录到客户端计算机的用户帐户在 Kerberos 领域中。

Kerberos 版本 5 协议是运行 Windows XP Professional、 Windows Vista 和 Windows 7 的计算机上的网络身份验证的默认值。 Kerberos SSP 搜索用户的领域的域名的注册表，然后通过查询 DNS 服务器将名称解析为 IP 地址。 Kerberos 协议可以使用 DNS 来定位 Kdc 使用只是领域名称，但它必须专门配置为执行此操作。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)