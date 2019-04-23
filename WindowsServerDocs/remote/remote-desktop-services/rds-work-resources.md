---
title: 使用 Windows Server 上的 PowerShell 自定义 RDS 标题“工作资源”
description: 提供了说明如何从 Windows Server 中的默认值更改工作区名称。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826708"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>使用 Windows Server 上的 PowerShell 自定义 RDS 标题“工作资源”

当使用 Windows Server 以通过 RD web 访问或新的远程桌面应用访问 RemoteApps 或台式计算机，您可能已经注意到默认工作区标题是"工作资源"。  使用 PowerShell cmdlet，可以轻松地更改标题。

若要更改标题，请打开新连接代理服务器上的 PowerShell 窗口并导入 RemoteDesktop 模块使用以下命令。

```powershell
    Import-Module RemoteDesktop
```

接下来，使用集 RDWorkspace 命令更改工作区名称。

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

例如，可以使用以下命令将工作区名称更改为"Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

如果您正在多个连接代理高可用性模式下，您必须针对活动 broker 运行。 可以使用以下命令：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

有关集 RDWorkspace cmdlet 的详细信息，请参阅[集 RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps)引用。