---
title: 正在为 IP 地址配置 Kerberos
description: Kerberos 支持基于 IP 的 Spn
author: daveba
ms.author: daveba
ms.openlocfilehash: 1061364528100fe005e80f64c6315f9fca69ad98
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980296"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 客户端允许 IPv4 和 IPv6 地址主机名位于服务主体名称 (Spn) 中

>适用于：Windows Server（半年频道）、Windows Server 2016

从 Windows 10 版本1507和 Windows Server 2016 开始, Kerberos 客户端可以配置为在 Spn 中支持 IPv4 和 IPv6 主机名。

默认情况下, 如果主机名是 IP 地址, 则 Windows 将不会对主机尝试 Kerberos 身份验证。 它将回退到其他已启用的身份验证协议, 例如 NTLM。 但是, 有时应用程序会硬编码为使用 IP 地址, 这意味着应用程序将回退到 NTLM 而不使用 Kerberos。 当环境移动到禁用 NTLM 时, 这可能会导致兼容性问题。

为了降低禁用 NTLM 的影响, 引入了一项新功能, 允许管理员将 IP 地址用作服务主体名称中的主机名。 此功能在客户端上通过注册表项值启用。

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

若要配置对 Spn 中 IP 地址主机名的支持, 请创建 TryIPSPN 条目。 默认情况下，注册表中不存在此项。 创建项后, 将 DWORD 值更改为1。 需要在每台客户端计算机上设置此注册表值, 这些计算机需要按 IP 地址访问受 Kerberos 保护的资源。

## <a name="configuring-a-service-principal-name-as-ip-address"></a>将服务主体名称配置为 IP 地址

服务主体名称是在 Kerberos 身份验证期间用于标识网络上的服务的唯一标识符。 SPN 由服务、主机名和 (可选) 形式`service/hostname[:port]`组成, `host/fs.contoso.com`如。 当计算机加入到 Active Directory 时, Windows 将向计算机对象注册多个 Spn。

IP 地址通常不用于替代主机名, 因为 IP 地址通常是临时的。 这可能会导致冲突和身份验证失败, 因为地址租约过期和续订。 因此, 注册基于 IP 地址的 SPN 是一个手动过程, 只应在无法切换到基于 DNS 的主机名时使用。

推荐的方法是使用[Setspn](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11))工具。 请注意, 每次只能将一个 SPN 注册到 Active Directory 中的一个帐户, 因此, 如果使用 DHCP, 则建议 IP 地址具有静态租约。

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

例如：

```
Setspn -s host/192.168.1.1 server01
```
