---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 简化管理附录
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ffc2849fa5e18f7984814d6187cf83d68566409b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369643"
---
# <a name="simplified-administration-appendix"></a>简化管理附录

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
-   [服务器管理器添加服务器 "对话框（Active Directory）](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [服务器管理器远程服务器状态](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Windows PowerShell 模块加载](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [旧操作系统的 RID 颁发修补程序](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil 从媒体更改安装](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>服务器管理器添加服务器 "对话框（Active Directory）  

"**添加服务器**" 对话框允许搜索服务器、操作系统、使用通配符和位置的 Active Directory。 此对话框还允许按完全限定的域名或前缀名称使用 DNS 查询。 这些搜索使用通过 .NET 实现的本机 DNS 和 LDAP 协议，而不是通过 SOAP 对 AD 管理网关进行 AD Windows PowerShell-这意味着，通过服务器管理器联系的域控制器甚至可以运行 Windows Server 2003。 还可以导入包含服务器名称的文件，以便进行设置。  
  
Active Directory 搜索使用以下 LDAP 筛选器：  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
Active Directory 搜索返回以下属性：  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>服务器管理器远程服务器状态  
服务器管理器使用地址路由协议测试远程服务器的可访问性。 不会列出任何不响应 ARP 请求的服务器，即使这些服务器位于池中。  
  
如果 ARP 做出响应，则会与服务器建立 DCOM 和 WMI 连接，以返回状态信息。 如果 RPC、DCOM 和 WMI 无法访问，服务器管理器将无法完全管理该服务器。  
  
## <a name="BKMK_PSLoadModule"></a>Windows PowerShell 模块加载  
Windows PowerShell 3.0 实现动态模块加载。 通常不再需要使用**import-module** cmdlet;相反，只需调用 cmdlet、alias 或 function，就会自动加载模块。  
  
若要查看已加载的模块，请使用**get-help** cmdlet。  
  
```  
Get-Module  
  
```  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
若要查看所有已安装的模块及其导出的函数和 cmdlet，请使用：  
  
```  
Get-Module -ListAvailable  
  
```  
  
使用**import-module**命令的主要情况是需要访问 "AD："Windows PowerShell 虚拟驱动器，但没有其他任何内容已加载该模块。 例如，使用以下命令：  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>旧操作系统的 RID 颁发修补程序  
请参阅[更新可在运行 Windows Server 2008 R2 的域控制器上检测并防止全局 RID 池的使用率过高](https://support.microsoft.com/kb/2618669)。  
  
## <a name="BKMK_IFM"></a>Ntdsutil 从媒体更改安装  
Windows Server 2012 将两个附加选项添加到用于 Ifm 的 Ntdsutil.exe 命令行工具 **（Ifm 媒体创建）** 菜单。 这样，你就可以创建 IFM 存储，而无需首先对导出的 NTDS 执行脱机碎片整理。DIT 数据库文件。 当磁盘空间不是高级版时，这将节省创建 IFM 的时间。  
  
下表介绍了两个新的菜单项：  
  
|||  
|-|-|  
|菜单项|说明|  
|创建完整的 NoDefrag% s|在文件夹% s 中为完整 AD DC 或 AD/LDS 实例创建未进行碎片整理的 IFM 介质|  
|创建 Sysvol Full NoDefrag% s|使用 SYSVOL 创建已满 AD DC 的 IFM 介质到文件夹% s|  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


