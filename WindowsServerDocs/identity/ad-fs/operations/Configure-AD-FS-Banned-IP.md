---
title: 配置 AD FS 禁止的 IP 地址
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8ff87a1043b589e83faa875467ddced536291b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867508"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS 和禁止的 IP 地址

>适用于：Windows Server 2016

在 2018 年 6 月，Windows Server 2016 上的 AD FS 引入**禁止的 Ip** 2018 年 6 月更新与 AD FS。  此更新，可在 AD FS 中，全局配置一组 IP 地址，以便针对来自这些 IP 地址，或该请求中有这些 IP 地址**x-转发-对于**或**x-ms-转发的客户端的 ip**标头，将阻止由 AD FS。

## <a name="adding-banned-ips"></a>添加禁止的 Ip
若要将被阻止的 Ip 添加到全局列表，请使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允许的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 的 CIDR 格式

不存在的 300 项被阻止的 IP 地址限制。 CIDR 或范围格式可用于拒绝包含单个项的项的大块。

## <a name="removing-banned-ips"></a>删除禁止的 Ip
若要从全局列表中删除被阻止的 Ip，请使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>读取禁止的 Ip
若要读取当前集的被阻止的 IP 地址，使用以下 Powershell cmdlet:

``` powershell
PS C:\ >Get-AdfsProperties 
```

输出示例：

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他参考  
[保护 Active Directory 联合身份验证服务的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
