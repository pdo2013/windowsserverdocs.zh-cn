---
title: "管理分支缓存中 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>管理分支缓存中 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

分支缓存可以帮助你优化 Internet 使用、提高联网的应用程序的性能和减少网络范围内区域 (WAN) 上的流量 Windows Server Essentials 服务器时所在远程从你的 office，或在客户端计算机连接到本地服务器 SharePoint Online 库如使用基于云的资源。  
  
 与分支缓存启用，当客户端计算机远程 Windows Server Essentials 服务器，该内容从请求内容缓存在本地 office。 在此之后，在同一办公室中的其他计算机可以获取本地而再次通过 WAN 从服务器下载内容的内容。 这可以提高联网应用程序的性能，并通过 WAN 减少占用带宽。  
  
 本地或远程 Windows Server Essentials 服务器是否、分支缓存可提升服务器共享文件夹和位于（如 SharePoint Online 库）服务器的 Web 内容的响应时间。  
  
 因为新硬件或网络拓扑更改分支缓存不需要，此功能向你提供的简单方法优化带宽使用量和改进的服务和访问过通过 WAN 资源响应时间。  
  
## <a name="branchcache-scenarios"></a>分支缓存方案  
 有三个基本方案使用远程服务器分支缓存：  
  
-   Windows Server Essentials 服务器位于 Microsoft Azure 中。  
  
-   Windows Server Essentials 服务器托管的第三方服务提供商数据中心中。  
  
-   Windows Server Essentials 服务器处于其他 office 在不同的物理位置。  
  
## <a name="distributed-cache-mode"></a>分布式的缓存模式  
 在 Windows Server Essentials 分支缓存中实现*分布式的缓存模式*，两种分支缓存中提供的缓存模式。 分布式的缓存模式中的内容的缓存中分支机构分布在客户端计算机之间。 不需要任何额外的硬件或拓扑更改，因为此模式下将非常适用于小型办公室，使用远程服务器或使用本地服务器访问基于云的服务，如 SharePoint Online。 当你打开分支缓存中 Windows Server Essentials 时，实现分布式的缓存模式。  
  
> [!NOTE]
>  在较大分支机构或大量员工使用联网应用程序使用多个子网是非常有用实现分支缓存中的*托管缓存模式*。 托管的缓存型分支办公室中的一个或多个托管的缓存服务器上存储的内容的缓存。
  
## <a name="requirements"></a>要求  
 若要在 Windows Server Essentials 使用分支缓存，服务器和客户端计算机必须满足以下要求：  
  
-   服务器必须与 Windows Server Essentials 体验角色 Windows Server Essentials 操作系统或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 操作系统运行。  
  
     在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 服务器上，当您添加 Windows Server Essentials 体验角色添加分支缓存。 若要打开分支缓存，你将需要登录到域管理员凭据与 Windows Server Essentials 仪表板中。  
  
-   客户端计算机必须运行 Windows 7 企业版、Windows 7 旗舰版、Windows 8 企业版或 Windows 8.1 企业版操作系统。  
  
-   分布式的缓存型所有客户端计算机必须相同子网。  
  
    > [!NOTE]
    >  如果你连接到你的 Windows Server Essentials 服务器无加入域的客户端，从默认情况下缓存排除这些计算机。 若要在缓存包括是未加入域的客户端计算机，运行[启用 BCDistributed](https://technet.microsoft.com/library/hh848398.aspx)客户端计算机上的 Windows PowerShell cmdlet。 有关详细信息，请参阅[分支缓存 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx)。  
 
  
## <a name="turn-branchcache-on"></a>打开分支缓存  
 若要在分布式的缓存模式下打开分支缓存，你只需单击 Windows Server Essentials 仪表板上的按钮。 缓存立即开始，并且坦诚执行。  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>若要打开分支缓存中 Windows Server Essentials  
  
1.  在 Windows Server Essentials 服务器上的管理员帐户登录。  
  
2.  在 Windows Server Essentials 仪表板中，单击**设置**。  
  
     打开设置向导。  
  
3.  单击**分支缓存**。  
  
4.  在**分支缓存设置**页上，单击**打开**。  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>使用 Windows PowerShell 要打开或关闭分支缓存  
 你可以使用 Windows PowerShell 检查分支缓存（启用或禁用）的状态以及要打开或关闭分支缓存。  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>若要将使用 Windows PowerShell 分支缓存打开或关闭  
  
1.  在服务器上，打开 Windows PowerShell 以管理员身份登录。 在**开始**页上，请右键单击**Windows PowerShell**，单击**以管理员身份运行**，然后单击**是**。  
  
2.  输入在命令提示符下列 cmdlet 任一。  
  
    -   若要检查分支缓存（启用或禁用）的状态，请输入：  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   若要打开分支缓存，输入：  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   若要关闭分支缓存，输入：  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>请参阅  
    
-   [分支缓存概述](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
