---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 简化管理附录
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858258"
---
# <a name="simplified-administration-appendix"></a>简化管理附录

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

  
-   [服务器管理器添加服务器对话框 (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [服务器管理器远程服务器状态](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Windows PowerShell 模块加载](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [早期版本的操作系统的 RID 颁发修补程序](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe 从媒体安装更改](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>服务器管理器添加服务器对话框 (Active Directory)  

**添加服务器**对话框允许使用通配符，操作系统和位置的服务器，搜索 Active Directory。 该对话框还允许使用 DNS 查询由完全限定的域名或前缀名称。 这些搜索使用通过.NET，对通过 SOAP-这意味着域控制器联系的服务器管理器甚至可以运行 Windows Server 2003 AD 管理网关不是 AD Windows PowerShell 实现的本机 DNS 和 LDAP 协议。 此外可以导入预配目的的服务器名称的文件。  
  
Active Directory 搜索使用以下 LDAP 筛选器：  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
Active Directory 搜索返回的以下属性：  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>服务器管理器远程服务器状态  
服务器管理器测试使用的地址路由协议的远程服务器可访问性。 未列出任何服务器未响应 ARP 请求，即使它们是在池中。  
  
如果 ARP 响应，则 DCOM 和 WMI 会通过建立连接到服务器返回状态信息。 如果无法访问 RPC、 DCOM 和 WMI，服务器管理器不能完全管理服务器。  
  
## <a name="BKMK_PSLoadModule"></a>Windows PowerShell 模块加载  
Windows PowerShell 3.0 实现动态加载的模块。 使用**导入模块**cmdlet 通常不再是必需的; 相反，只需自动调用 cmdlet、 别名或函数将加载该模块。  
  
若要查看已加载的模块，请使用**Get-module** cmdlet。  
  
```  
Get-Module  
  
```  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
若要查看所有已安装的模块与他们的导出的函数和 cmdlet 一起使用，请使用：  
  
```  
Get-Module -ListAvailable  
  
```  
  
使用主用例**导入模块**命令时需要访问"AD:"Windows PowerShell 虚拟驱动器而不是其他已加载模块。 例如，使用以下命令：  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>早期版本的操作系统的 RID 颁发修补程序  
请参阅[有可用更新来检测和防止过多消耗全局 RID 池运行 Windows Server 2008 R2 的域控制器上](https://support.microsoft.com/kb/2618669)。  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe 从媒体安装更改  
Windows Server 2012 将两个附加选项添加到的 Ntdsutil.exe 命令行工具**IFM （IFM 媒体创建）** 菜单。 这些规则可以创建 IFM 存储而不首先执行导出 NTDS 脱机碎片整理。DIT 数据库文件。 当没有高级磁盘空间时，这节省了时间创建 IFM。  
  
下表介绍了两个新菜单项：  
  
|||  
|-|-|  
|菜单项|说明|  
|创建完整 NoDefrag %s|不完整的 AD DC 或 AD LDS/实例到文件夹 %s 碎片整理的情况下创建 IFM 媒体|  
|创建 Sysvol 完整 NoDefrag %s|创建带 SYSVOL 和没有为完整的 AD DC 整理到文件夹 %s IFM 媒体|  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


