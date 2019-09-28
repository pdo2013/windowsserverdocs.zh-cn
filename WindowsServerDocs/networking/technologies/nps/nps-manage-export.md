---
title: 导出 NPS 配置以便在其他服务器上导入
description: 您可以使用本主题来了解如何导出 Windows Server 2016 中的网络策略服务器配置。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cbebd0388ccd5dd2540a20f5d325d7f97c7e2bb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405435"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>导出 NPS 配置以便在其他服务器上导入

适用于：Windows Server 2016

可以从一个 NPS 导出整个 NPS 配置，包括 RADIUS 客户端和服务器、网络策略、连接请求策略、注册表和日志记录配置。 

使用以下工具之一来导出 NPS 配置：

- 在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中，你可以使用 Netsh，或者可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008 中，使用 Netsh。

> [!IMPORTANT]
> 如果源 NPS 数据库的版本号高于目标 NPS 数据库的版本号，请不要使用此过程。 可以从**netsh NPS show config**命令的显示中查看 NPS 数据库的版本号。

由于 NPS 配置未在导出的 XML 文件中加密，因此通过网络发送时可能会带来安全风险，因此，请在将 XML 文件从源服务器移到目标服务器时采取预防措施。 例如，在移动文件之前将文件添加到受密码保护的加密存档文件。 此外，将文件存储在安全的位置，以防止恶意用户对其进行访问。

> [!NOTE]
> 如果在源 NPS 上配置 SQL Server 日志记录，则不会将 SQL Server 日志记录设置导出到 XML 文件。 在将文件导入另一个 NPS 之后，必须手动配置 SQL Server 日志记录。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>使用 Windows PowerShell 导出和导入 NPS 配置

对于 Windows Server 2012 及更高版本的操作系统版本，可以使用 Windows PowerShell 导出 NPS 配置。

用于导出 NPS 配置的命令语法如下所示。 

    Export-NpsConfiguration -Path <filename>

下表列出了 Windows PowerShell 中**export-npsconfiguration** cmdlet 的参数。 粗体参数是必需的。

|参数|描述|
|---------|-----------|
|Path|指定要导出 NPS 配置的 XML 文件的名称和位置。|

**管理凭据**

若要完成此过程，您必须是 Administrators 组的成员。

### <a name="export-example"></a>导出示例 

在下面的示例中，NPS 配置将导出到位于本地驱动器上的 XML 文件。 若要运行此命令，请在源 NPS 上以管理员身份运行 Windows PowerShell，键入以下命令，然后按 Enter。

`Export-NpsConfiguration –Path c:\config.xml` 

有关详细信息，请参阅[export-npsconfiguration](https://technet.microsoft.com/library/jj872749.aspx)。

导出 NPS 配置后，将 XML 文件复制到目标服务器。

用于在目标服务器上导入 NPS 配置的命令语法如下所示。

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>导入示例

下面的命令将名为 C:\Npsconfig.xml 的文件中的设置导入到 NPS。 若要运行此命令，请在目标 NPS 上以管理员身份运行 Windows PowerShell，键入以下命令，然后按 Enter。

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

有关详细信息，请参阅[export-npsconfiguration](https://technet.microsoft.com/library/jj872750.aspx)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>使用 Netsh 导出和导入 NPS 配置

可以使用 Network Shell \(Netsh @ no__t-1，通过使用**netsh nps 导出**命令导出 NPS 配置。

运行**netsh nps import**命令时，将自动刷新 nps，并提供更新的配置设置。 您无需在目标计算机上停止 NPS 即可运行**netsh NPS import**命令，但是，如果在配置导入期间 nps 控制台或 nps mmc 管理单元处于打开状态，则在您刷新视图之前，对服务器配置所做的更改将不可见。 

> [!NOTE]
> 使用**netsh nps export**命令时，需要提供具有值 **"是"** 的命令参数**exportPSK** 。 此参数和值明确表明你要导出 NPS 配置，并且导出的 XML 文件包含 RADIUS 客户端和远程 RADIUS 服务器组成员的未加密共享机密。

**管理凭据**

若要完成此过程，您必须是 Administrators 组的成员。

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>使用 Netsh 命令将 NPS 配置复制到其他 NPS

1. 在源 NPS 上，打开**命令提示符**，键入**netsh**，然后按 enter。

2. 在**netsh**提示符下，键入**nps**，然后按 enter。 

3. 在**netsh nps**提示符下，键入**export filename =** "*path\file.xml*" **exportPSK = YES**，其中*path*是要保存 nps 配置文件的文件夹位置， *file*是 xml 文件的名称。要保存。 按 Enter。 

这会在 XML 文件中存储配置设置 @no__t 0including 注册表设置 @ no__t-1。 路径可以是相对路径或绝对路径，也可以是通用命名约定 \(UNC @ no__t 路径。 按下 Enter 后，将显示一条消息，指示导出到文件是否成功。

4. 将创建的文件复制到目标 NPS。

5. 在目标 NPS 上的命令提示符处，键入**netsh NPS import filename =** "*path\file.xml*"，然后按 enter。 将出现一条消息，指示是否已成功导入 XML 文件。

有关 netsh 的详细信息，请参阅[Network Shell （netsh）](../netsh/netsh.md)。

