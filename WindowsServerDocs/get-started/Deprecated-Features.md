---
title: Windows Server 2016 中已删除或弃用的功能
description: 各版本中已删除或计划删除的特性和功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/02/2017
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 20178a3be14c076623f647fa139e013528de9a69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858158"
---
# <a name="features-removed-or-deprecated-in--windows-server-2016"></a>Windows Server 2016 中已删除或弃用的功能

>适用于：Windows Server 2016

以下是 Windows Server 2016 中已从当前版本的产品中删除或计划在后续版本中可能删除（“已弃用”）的特性和功能列表。 本文档面向在商业环境中更新操作系统的 IT 专业人员。 在后续的版本中可能会对该列表进行更改，并且可能不包含任何已弃用的特性或功能。 有关特定特性或功能及其替代项的详细信息，请参阅相应特性或功能的文档。  

## <a name="features-removed-from-windows-server-2016"></a>从 Windows Server 2016 中删除的功能 
从此版本的 Windows Server 2016 中删除了以下特性和功能。 除非你使用了替代方法，否则依赖于这些功能的应用程序、代码或用法都将无法工作。  

> [!NOTE]  
> 如果你要从低于 Windows Server 2012 R2 或 Windows Server 2012 的服务器版本迁移到 Windows Server 2016，则还应查看 [Windows Server 2012 R2 中删除或弃用的功能](https://technet.microsoft.com/library/dn303411.aspx)和 [Windows Server 2012 中删除或弃用的功能](https://technet.microsoft.com/library/hh831568.aspx)。  


### <a name="file-server"></a>文件服务器  
Microsoft 管理控制台的“共享和存储管理”管理单元已被删除。 不过，可以执行以下任一操作：  

-   如果想要管理的计算机正在运行的操作系统早于 Windows Server 2016，则使用远程桌面与其连接，并使用“共享和存储管理”管理单元的本地版本。  

-   在运行 Windows 8.1 或更早版本的计算机上，使用 RSAT 中的“共享和存储管理”管理单元查看想要管理的计算机。  

-   在客户端计算机上使用 Hyper-V 运行正在运行具有 RSAT 中的“共享和存储管理”管理单元的 Windows 7、Windows 8 或 Windows 8.1 的虚拟机。  

### <a name="journaldll"></a>Journal.dll  
Journal.dll 已从 Windows Server 2016 中删除。 没有替换。  

### <a name="security-configuration-wizard"></a>安全配置向导  
安全配置向导已删除。 现在默认对功能提供保护。 如果需要控制特定的安全设置，可以使用组策略或 [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx)。  

### <a name="sqm"></a>SQM  
管理参与客户体验改进计划的选择加入组件已被删除。 

### <a name="windows-update"></a>Windows 更新
**wuauclt.exe /detectnow** 命令已删除，并且不再受支持。 要触发更新扫描，请执行以下任一操作：

- 运行这些 PowerShell 命令：
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"`
    $AutoUpdates.DetectNow()` 
    ````

- 或者，使用此 VBScript：
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## <a name="features-deprecated-starting-with-windows-server-2016"></a>从 Windows Server 2016 开始弃用的功能 
从此版本开始摒弃了以下特性和功能。 最后，将完全从产品中删除这些功能，但在此版本中这些功能仍然可用，只是有时删除了某些功能。 现在，你应该开始计划对依赖于这些功能的任何应用程序、代码或用法使用其他方法。  

### <a name="configuration-tools"></a>配置工具  

-   **Scregedit.exe**已弃用。 如果有依赖于 Scregedit.exe 的脚本，请调整这些脚本以使用 Reg.exe 或 Windows PowerShell 方法。  

-   **Sconfig.exe**已弃用。 使用 Windows PowerShell 代替。  

### <a name="netcfg-custom-apis"></a>NetCfg 自定义 API  
NetCfg 自定义 API 的 PrintProvider、NetClient 和 ISDN 安装已弃用。  

### <a name="remote-management"></a>远程管理  
WinRM.vbs 已弃用。 使用 Windows PowerShell 的 WinRM 提供程序中的功能代替。  

### <a name="smb"></a>SMB  
SMB 2+ over NetBT 已弃用。 相反，实现 SMB over TCP 或 RDMA。 
