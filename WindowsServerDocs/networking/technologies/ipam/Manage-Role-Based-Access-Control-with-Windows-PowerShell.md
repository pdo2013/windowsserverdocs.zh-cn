---
title: 管理基于角色访问与 Windows PowerShell 控件
description: 本主题介绍的 IP 地址管理 (IPAM) 管理指南中的 Windows Server 2016 的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>管理基于角色访问与 Windows PowerShell 控件

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何使用 IPAM 若要管理与 Windows PowerShell 基于角色访问控制。  
  
>[!NOTE]
>请 IPAM Windows PowerShell 命令参考[IP 地址管理 (IPAM) 服务器 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx)。  
  
新的 Windows PowerShell IPAM 命令为您提供了以下功能： 检索并更改 DNS 和 DHCP 对象访问范围。 下表显示了正确的命令，用于每个 IPAM 对象。  
  
|IPAM 对象|命令|描述|  
|---------------|-----------|---------------|  
|DNS 服务器|获取 IpamDnsServer|此 cmdlet 在 IPAM 返回 DNS 服务器对象|  
|DNS 区域|获取 IpamDnsZone|此 cmdlet 在 IPAM 返回 DNS 区域对象|  
|DNS 资源记录|获取 IpamResourceRecord|此 cmdlet 在 IPAM 返回 DNS 资源录制对象|  
|DNS 条件转发器|获取 IpamDnsConditionalForwarder|此 cmdlet 在 IPAM 返回 DNS 条件转发器对象|  
|DHCP 服务器|获取 IpamDhcpServer|此 cmdlet 在 IPAM 返回 DHCP 服务器对象|  
|DHCP 超级|获取 IpamDhcpSuperscope|此 cmdlet 在 IPAM 返回 DHCP 超级对象|  
|DHCP 范围|获取 IpamDhcpScope|此 cmdlet 在 IPAM 返回 DHCP 范围对象|  
  
在以下示例中的命令输出`Get-IpamDnsZone`cmdlet 检索**dublin.contoso.com** DNS 区域。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>设置 IPAM 对象访问范围  
你可以访问范围 IPAM 对象的设置使用`Set-IpamAccessScope`命令。 若要为特定值为对象设置访问范围或导致对象文件的访问权限的范围内父对象从沿用文件，可以使用此命令。 以下是你可以使用此命令配置的对象。  
  
-   DHCP 范围  
  
-   DHCP 服务器  
  
-   DHCP 超级  
  
-   DNS 条件转发器  
  
-   DNS 资源记录  
  
-   DNS 服务器  
  
-   DNS 区域  
  
-   IP 地址阻止  
  
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
  
在下面的示例，范围访问 DNS 区域**dublin.contoso.com**从更改**都柏林**到**欧洲**。  
  
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
  


