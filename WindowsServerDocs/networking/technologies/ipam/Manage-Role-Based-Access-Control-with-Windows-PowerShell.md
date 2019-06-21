---
title: 使用 Windows PowerShell 管理基于角色的访问控制
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11dd417be4720b09851fc03f111edaaf06b55e59
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282135"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>使用 Windows PowerShell 管理基于角色的访问控制

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何使用 IPAM 管理基于角色的访问控制使用 Windows PowerShell。  
  
>[!NOTE]
>有关 IPAM 的 Windows PowerShell 命令参考，请参阅[Windows PowerShell 中的 IpamServer cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps)。  
  
新的 Windows PowerShell IPAM 命令为您提供能够检索和更改 DNS 和 DHCP 对象的访问作用域。 下表说明了要用于每个 IPAM 对象的正确命令。  
  
|IPAM 对象|Command|描述|  
|---------------|-----------|---------------|  
|DNS 服务器|Get-IpamDnsServer|此 cmdlet 返回在 IPAM 中的 DNS 服务器对象|  
|DNS 区域|Get-IpamDnsZone|此 cmdlet 返回在 IPAM 中的 DNS 区域对象|  
|DNS 资源记录|Get-IpamResourceRecord|此 cmdlet 返回在 IPAM 中的 DNS 资源记录对象|  
|DNS 条件转发器|Get-IpamDnsConditionalForwarder|此 cmdlet 将返回在 IPAM 中的 DNS 条件转发器对象|  
|DHCP 服务器|Get-IpamDhcpServer|此 cmdlet 返回在 IPAM 中的 DHCP 服务器对象|  
|DHCP 超级作用域|Get-IpamDhcpSuperscope|此 cmdlet 将返回在 IPAM 中的 DHCP 超级作用域对象|  
|DHCP 作用域|Get-IpamDhcpScope|此 cmdlet 返回在 IPAM 中的 DHCP 作用域对象|  
  
在以下示例中的命令输出中， `Get-IpamDnsZone` cmdlet 检索**dublin.contoso.com** DNS 区域。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>设置 IPAM 对象的访问作用域  
可以通过使用 IPAM 对象上设置访问作用域`Set-IpamAccessScope`命令。 若要为特定值的对象设置访问作用域或导致要从父对象继承访问作用域的对象，可以使用此命令。 以下是可以使用此命令配置的对象。  
  
-   DHCP 作用域  
  
-   DHCP 服务器  
  
-   DHCP 超级作用域  
  
-   DNS 条件转发器  
  
-   DNS 资源记录  
  
-   DNS 服务器  
  
-   DNS 区域  
  
-   IP 地址块  
  
-   IP 地址范围  
  
-   IP 地址空间  
  
-   IP 地址子网  
  
下面是有关语法`Set-IpamAccessScope`命令。  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
在下面的示例中，DNS 区域的访问作用域**dublin.contoso.com**从更改**Dublin**到**欧洲**。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


