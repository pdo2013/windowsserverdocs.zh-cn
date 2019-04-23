---
title: 管理 Windows Server Essentials 中的 BranchCache
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828798"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 BranchCache

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

BranchCache 可以帮助你优化 Internet 使用率、 改进的联网的应用程序性能并减少在广域网 (WAN) 上的流量，当 Windows Server Essentials 服务器位于远程从办公室，或者当客户端计算机连接到 SharePoint Online 库等本地服务器使用基于云的资源。  
  
 启用 branchcache，当客户端计算机请求内容从远程 Windows Server Essentials 服务器，内容缓存在本地办公室中。 在此之后，同一办公室中的其他计算机可以从本地获取内容，而无需通过 WAN 再次从服务器下载内容。 这可以改善网络应用程序的性能并通过 WAN 减少带宽使用。  
  
 在 Windows Server Essentials 服务器是本地还是远程，BranchCache 可以改善服务器共享文件夹和 （如 SharePoint Online 库） 在服务器托管的 Web 内容的响应时间。  
  
 由于 BranchCache 不需要对新硬件或网络拓扑进行新的更改，所以此功能将提供一种简单方式，来优化带宽使用并缩短对通过 WAN 访问的服务和资源的响应时间。  
  
## <a name="branchcache-scenarios"></a>BranchCache 方案  
 提供三种用于通过远程服务器使用 BranchCache 的基本方案：  
  
-   在 Microsoft Azure 中托管你的 Windows Server Essentials 服务器。  
  
-   在第三方服务提供程序的数据中心内托管你的 Windows Server Essentials 服务器。  
  
-   Windows Server Essentials 服务器位于不同的物理位置中的另一个 office。  
  
## <a name="distributed-cache-mode"></a>分布式缓存模式  
 在 Windows Server Essentials 中，BranchCache 在中实现*分布式的缓存模式*，BranchCache 中可用的两种缓存模式之一。 在分布式缓存模式下，会在客户端计算机之间分布分支机构的内容缓存。 由于无需更改任何其他硬件或拓扑，此模式非常适合使用远程服务器或本地服务器访问 SharePoint Online 等基于云的服务的小型机构。 当你启用 Windows Server Essentials 中的 BranchCache 时，实现分布式的缓存模式。  
  
> [!NOTE]
>  在具有多个子网或大量员工使用网络应用程序的较大型分支机构中，在*托管缓存模式*下实现 BranchCache 可能非常有益。 在托管缓存模式下，内容缓存存储在分支机构的一个或多个托管缓存服务器上。
  
## <a name="requirements"></a>要求  
 若要在 Windows Server Essentials 中使用 BranchCache，你的服务器和客户端计算机必须满足以下要求：  
  
-   服务器必须具有 Windows Server Essentials 体验角色 Windows Server Essentials 操作系统或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 操作系统运行。  
  
     在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 数据中心服务器上，添加 Windows Server Essentials 体验角色时添加 BranchCache。 若要打开 BranchCache，你将需要登录到 Windows Server Essentials 仪表板使用域管理员凭据。  
  
-   客户端计算机必须运行 Windows 7 企业版、 Windows 7 旗舰版、 Windows 8 企业版或 Windows 8.1 Enterprise 操作系统。  
  
-   在分布式缓存模式下，所有客户端计算机都必须在同一子网上。  
  
    > [!NOTE]
    >  如果已将客户端计算机连接到 Windows Server Essentials 服务器而未将其加入域中，在默认情况下，将从缓存中排除这些计算机。 若要将未加入域的客户端计算机包含在缓存中，请在客户端计算机上运行 [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) Windows PowerShell cmdlet。 有关详细信息，请参阅 [Windows PowerShell 中的 BranchCache Cmdlet](https://technet.microsoft.com/library/hh848392.aspx)。  
 
  
## <a name="turn-branchcache-on"></a>打开 BranchCache  
 若要在分布式的缓存模式下打开 BranchCache，只需单击 Windows Server Essentials 仪表板上的按钮。 缓存将立即开始并透明地执行。  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>在 Windows Server Essentials 中打开 BranchCache  
  
1.  在使用管理员帐户的 Windows Server Essentials 服务器上登录。  
  
2.  在 Windows Server Essentials 仪表板中，单击**设置**。  
  
     设置向导将打开。  
  
3.  单击“BranchCache”。  
  
4.  在“BranchCache 设置”  页面上，单击“打开” 。  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>使用 Windows PowerShell 打开或关闭 BranchCache  
 可以使用 Windows PowerShell 来检查 BranchCache 状态（启用或禁用）以及打开或关闭 BranchCache。  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>使用 Windows PowerShell 打开或关闭 BranchCache  
  
1.  在服务器上，以管理员身份打开 Windows PowerShell。 在“开始”页面上，右键单击 “Windows PowerShell”、单击“以管理员身份运行”，然后单击“是”。  
  
2.  在命令提示符下，输入以下任一 cmdlet：  
  
    -   若要检查 BranchCache 的状态（启用或禁用），请输入：  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   若要打开 BranchCache，请输入：  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   若要关闭 BranchCache，请输入：  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>请参阅  
    
-   [BranchCache 概述](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
