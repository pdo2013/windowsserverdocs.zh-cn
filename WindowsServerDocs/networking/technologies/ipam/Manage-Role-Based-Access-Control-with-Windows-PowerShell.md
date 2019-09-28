---
title: 使用 Windows PowerShell 管理基于角色的访问控制
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dec5c9b9b5d5fe858e063af70ff0a8e16991e632
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355221"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>使用 Windows PowerShell 管理基于角色的访问控制

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解如何使用 IPAM 通过 Windows PowerShell 来管理基于角色的访问控制。  
  
>[!NOTE]
>有关 IPAM Windows PowerShell 命令参考，请参阅[Windows powershell 中的 IpamServer cmdlet](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps)。  
  
新的 Windows PowerShell IPAM 命令使你能够检索和更改 DNS 和 DHCP 对象的访问作用域。 下表说明了要用于每个 IPAM 对象的正确命令。  
  
|IPAM 对象|Command|描述|  
|---------------|-----------|---------------|  
|DNS 服务器|IpamDnsServer|此 cmdlet 将返回 IPAM 中的 DNS 服务器对象|  
|DNS 区域|IpamDnsZone|此 cmdlet 将返回 IPAM 中的 DNS 区域对象|  
|DNS 资源记录|IpamResourceRecord|此 cmdlet 返回 IPAM 中的 DNS 资源记录对象|  
|DNS 条件转发器|IpamDnsConditionalForwarder|此 cmdlet 将返回 IPAM 中的 DNS 条件转发器对象|  
|DHCP 服务器|IpamDhcpServer|此 cmdlet 将返回 IPAM 中的 DHCP 服务器对象|  
|DHCP 超级作用域|IpamDhcpSuperscope|此 cmdlet 将返回 IPAM 中的 DHCP 超级作用域对象|  
|DHCP 作用域|IpamDhcpScope|此 cmdlet 将返回 IPAM 中的 DHCP 作用域对象|  
  
在下面的命令输出示例中，@no__t cmdlet 检索**Dublin.contoso.com** DNS 区域。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>在 IPAM 对象上设置访问作用域  
可以通过使用 `Set-IpamAccessScope` 命令设置对 IPAM 对象的访问作用域。 您可以使用此命令将访问作用域设置为对象的特定值，或者使对象继承父对象的访问作用域。 下面是可以通过此命令配置的对象。  
  
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
  
下面是 `Set-IpamAccessScope` 命令的语法。  
  
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
  
在以下示例中，DNS 区域**dublin.contoso.com**的访问作用域从**都柏林**更改为**欧洲**。  
  
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
  


