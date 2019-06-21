---
title: 为 IP 地址配置 Kerberos
description: 基于 IP 的 Spn 的 Kerberos 支持
ms.openlocfilehash: aa2685fcff2fdf231e5e5884d25885585f0bd6c9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67279969"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 客户端允许在服务主体名称 (Spn) 中使用 IPv4 和 IPv6 地址的主机名

>适用于：Windows 服务器 （半年频道），Windows Server 2016

从 Windows 10 版本 1507年和 Windows Server 2016 开始，可以对 Kerberos 客户端被配置为在 Spn 中支持 IPv4 和 IPv6 主机名。

默认情况下 Windows 不会尝试 Kerberos 身份验证的主机，如果主机名的 IP 地址。 它将回退到 NTLM 之类的其他启用的身份验证协议。 但是，应用程序有时是将硬编码以便使用 IP 地址，这意味着应用程序将退回到 NTLM 和不使用 Kerberos。 当环境移动禁用 NTLM，这会导致兼容性问题。

为减少禁用 NTLM 的新功能的影响引入，它允许管理员为服务主体名称中的主机名使用 IP 地址。 通过注册表项值在客户端上启用此功能。

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

若要在 Spn 中配置 IP 地址的主机名的支持，创建 TryIPSPN 条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 此注册表值将需要在每个需要访问受 Kerberos 保护的资源的客户端计算机上设置的 IP 地址。

## <a name="configuring-a-service-principal-name-as-ip-address"></a>为 IP 地址配置的服务主体名称

服务主体名称是在 Kerberos 身份验证期间用于标识网络上的服务的唯一标识符。 SPN 包括服务、 主机名、 和 （可选） 的窗体中的端口`service/hostname[:port]`如`host/fs.contoso.com`。 在计算机加入到 Active Directory 时，Windows 将注册到一个计算机对象的多个 Spn。

IP 地址通常不使用主机名代替因为 IP 地址通常是临时。 这可能导致的冲突和身份验证失败，因为地址租约过期和续订。 因此注册 IP 地址基于 SPN 是一个手动过程，并仅用于时就无法切换到基于 DNS 的主机名。

建议的方法是使用[Setspn.exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11))工具。 请注意，一个 SPN 只能注册到 Active Directory 中的单个帐户一次因此建议 IP 地址具有静态租约，如果使用 DHCP。

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

例如：

```
Setspn -s host/192.168.1.1 server01
```
