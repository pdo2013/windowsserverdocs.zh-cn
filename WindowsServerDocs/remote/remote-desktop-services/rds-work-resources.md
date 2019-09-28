---
title: 使用 Windows Server 上的 PowerShell 自定义 RDS 标题“工作资源”
description: 介绍如何在 Windows Server 中更改默认的工作区名称。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: ea7e1505f705c87e58145e7a306ed355ab3acf7f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387048"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>使用 Windows Server 上的 PowerShell 自定义 RDS 标题“工作资源”

使用 Windows Server 通过 RD WebAccess 或新的远程桌面应用访问 RemoteApp 或桌面时，你可能已注意到，工作区默认带有“工作资源”标题。  使用 PowerShell cmdlet 可以轻松地更改此标题。

若要更改标题，请在连接代理服务器上打开新的 PowerShell 窗口，然后使用以下命令导入 RemoteDesktop 模块。

```powershell
    Import-Module RemoteDesktop
```

接下来，使用 Set-RDWorkspace 命令更改工作区名称。

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

例如，可以使用以下命令将工作区名称更改为“Contoso RemoteApps”：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

如果在高可用性模式下运行多个连接代理，必须针对活动的代理运行此命令。 可使用以下命令：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

有关 Set-RDWorkspace cmdlet 的详细信息，请参阅 [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) 参考。