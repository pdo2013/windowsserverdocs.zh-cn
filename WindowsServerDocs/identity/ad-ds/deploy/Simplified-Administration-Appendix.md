---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: "简化的管理附录"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>简化的管理附录

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

  
-   [服务器管理器添加服务器对话框 (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [服务器管理器远程服务器状态](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Windows PowerShell 模块加载](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [删除以前的操作系统颁发修补程序](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [从媒体更改 Ntdsutil.exe 安装](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>服务器管理器添加服务器对话框 (Active Directory)  

**添加服务器**对话框允许搜索 Active Directory 服务器，通过使用通配符、 操作系统和位置。 对话框也允许使用通过完整的域名或前缀名 DNS 查询的问题。 这些搜索使用通过.NET，不广告 Windows PowerShell 通过肥皂-即联系服务器管理器的域控制器甚至可以运行 Windows Server 2003 广告管理网关针对实现本机 DNS 和 LDAP 协议。 你还可以将具有服务器名称以供进行配置用途的文件导入。  
  
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
服务器管理器测试远程服务器辅助功能使用地址路由协议。 没有响应 ARP 请求任何服务器未列出，即使它们位于池。  
  
如果 ARP 响应，则 DCOM 和 WMI 是建立连接到服务器返回状态的信息。 如果无法访问 RPC、 DCOM 和 WMI，服务器管理器不能完全管理服务器。  
  
## <a name="BKMK_PSLoadModule"></a>Windows PowerShell 模块加载  
Windows PowerShell 3.0 实现动态加载的模块。 使用**导入模块**cmdlet 不再通常需要;相反，只需自动调用 cmdlet、 别名或函数加载的模块。  
  
若要查看已加载的模块，使用**获取模块**cmdlet。  
  
```  
Get-Module  
  
```  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
若要查看与他们的已导出的功能和 cmdlet 所有已安装的模块，使用：  
  
```  
Get-Module -ListAvailable  
  
```  
  
使用主的用例**导入模块**命令是你需要访问权限时，"广告:"Windows PowerShell 虚拟驱动器，而不是其他已加载的模块。 例如，使用以下命令：  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>删除以前的操作系统颁发修补程序  
请参阅[更新为可用检测和预防运行的 Windows Server 2008 R2 域控制器上的全球 RID 池太多消耗](https://support.microsoft.com/kb/2618669)。  
  
## <a name="BKMK_IFM"></a>从媒体更改 Ntdsutil.exe 安装  
Windows Server 2012 Ntdsutil.exe 命令行工具来添加另外两个选项**IFM （IFM 媒体创建）**菜单。 这些允许你无需先执行离线状态下的已导出 NTDS 磁盘碎片整理程序创建 IFM 官方商城。DIT 数据库文件。 当不磁盘空间不足时，这可以节省时间创建 IFM。  
  
下表介绍了两个新菜单项：  
  
|||  
|-|-|  
|菜单项|说明|  
|创建完整 NoDefrag %s|不完整的广告 DC 或广告/LDS 实例插入文件夹 %s 执行碎片整理创建 IFM 媒体|  
|创建 Sysvol 全屏 NoDefrag %s|SYSVOL 戴眼镜和不完整广告 dc 整理到文件夹中 %s 创建 IFM 媒体|  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![简化的管理](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


