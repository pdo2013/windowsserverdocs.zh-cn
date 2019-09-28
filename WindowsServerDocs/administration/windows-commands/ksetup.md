---
title: ksetup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 265f67bff65794938485472a41064837551c7699
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374804"
---
# <a name="ksetup"></a>ksetup



执行与设置和维护 Kerberos 协议和密钥发行中心（KDC）相关的任务，以支持 Kerberos 领域，这也不是 Windows 域。 有关如何使用此命令的示例，请参阅每个相关副标题中的 "示例" 部分。

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
|[Ksetup:setrealm](ksetup-setrealm.md)|使此计算机成为 Kerberos 领域的成员。|
|[Ksetup:mapuser](ksetup-mapuser.md)|将 Kerberos 主体映射到帐户。|
|[Ksetup:addkdc](ksetup-addkdc.md)|为给定领域定义 KDC 条目。|
|[Ksetup:delkdc](ksetup-delkdc.md)|删除领域的 KDC 条目。|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|为某个领域添加 Kpasswd 服务器地址。|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|删除领域的 Kpasswd 服务器地址。|
|[Ksetup:server](ksetup-server.md)|允许您指定要应用更改的 Windows 计算机的名称。|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|设置计算机的域帐户（或主机主体）的密码。|
|[Ksetup:removerealm](ksetup-removerealm.md)|从注册表中删除指定领域的所有信息。|
|[Ksetup:domain](ksetup-domain.md)|允许你指定域（@no__t 如果尚未使用 **/domain**设置 0DomainName >）。|
|[Ksetup:changepassword](ksetup-changepassword.md)|允许你使用 Kpasswd 更改已登录用户的密码。|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|列出**ksetup**可检测的可用领域标志。|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|设置特定领域的领域标志。|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|将其他领域标志添加到领域。|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|删除领域中的领域标志。|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|分析给定计算机上的 Kerberos 配置。 将主机添加到注册表的领域映射。|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|添加用于将主机映射到 Kerberos 领域的注册表值。|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|删除将主计算机映射到 Kerberos 领域的注册表值。|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|设置域的一个或多个加密类型信任属性。|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|获取域的加密类型信任属性。|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|向域的 "加密类型" 信任属性添加加密类型。|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|删除域的 "加密类型" 信任属性。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

**Ksetup**用于更改用于定位 Kerberos 领域的计算机设置。 在基于非 Microsoft Kerberos 的实现中，此信息通常保存在 Krb5.conf 文件中。 在 Windows Server 操作系统中，它保存在注册表中。 您可以使用此工具来修改这些设置。 工作站使用这些设置来查找 Kerberos 领域，并由域控制器用于查找跨领域信任关系的 Kerberos 领域。

如果计算机运行的是 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2，并且不是 Windows 的成员，则**Ksetup**将初始化 Kerberos 安全支持提供程序（SSP）用于查找 kerberos 领域的 KDC 的注册表项域名. 完成配置后，运行 Windows 操作系统的客户端计算机的用户可以登录到 Kerberos 领域中的帐户。

Kerberos 版本5协议是运行 Windows XP Professional、Windows Vista 和 Windows 7 的计算机上网络身份验证的默认值。 Kerberos SSP 会在注册表中搜索用户领域的域名，然后通过查询 DNS 服务器将该名称解析为 IP 地址。 Kerberos 协议可以使用 DNS 来仅使用领域名称查找 Kdc，但必须对其进行特殊配置。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)