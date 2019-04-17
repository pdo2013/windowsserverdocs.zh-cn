---
title: 要导入其他服务器上的导出 NPS 服务器配置
description: 你可以使用本主题以了解如何导出网络策略服务器配置 Windows Server 2016 中。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>要导入其他服务器上的导出 NPS 服务器配置

适用于：Windows Server 2016

你可以将导出整个 NPS 配置，包括 RADIUS 客户端和服务器、 网络策略、 连接请求策略、 注册表以及配置日志记录，从另一台 NPS 服务器导入一个 NPS 服务器。 

使用以下工具之一导出 NPS 配置：

- Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012，在你可以使用 Netsh，，或者你可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008，使用 Netsh。

>[!IMPORTANT]
>如果源 NPS 数据库有比目标 NPS 数据库的版本号较高版本号，不要使用此过程。 你可以查看 NPS 数据库中显示的版本号**netsh nps 显示配置**命令。

因为 NPS server 配置没有加密中导出 XML 文件中，发送它通过网络可能引起安全风险，因此从源服务器的 XML 文件移动到目的地的服务器时采取预防措施。 例如，将文件添加到加密，密码受保护的存档文件前将文件移动。 此外，在安全位置的用于阻止恶意用户访问它存储文件。

>[!NOTE]
>如果 SQL Server 日志记录配置源 NPS 服务器上，不会 SQL Server 记录设置导出到 XML 文件。 导入其他 NPS 服务器上的文件后，你必须手动配置 SQL Server 日志记录。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>导出和导入使用 Windows PowerShell NPS 配置

有关 Windows Server 2012 和更高版本操作系统版本，你可以将使用 Windows PowerShell NPS 配置导出。

命令语法导出 NPS 配置如下所示。 

    Export-NpsConfiguration -Path <filename>

下表列出了参数**导出 NpsConfiguration**中的 Windows PowerShell cmdlet。 参数粗体是必需的。

|参数|描述|
|---------|-----------|
|路径|指定的名称和你想要导出 NPS 服务器配置 XML 文件的位置。|

**管理凭据**

若要完成此过程，必须是管理员组中的成员。

### <a name="export-example"></a>导出示例 

在以下示例中，NPS 配置导出到本地驱动器上 XML 文件。 运行此命令，源 NPS 服务器上以管理员身份运行的 Windows PowerShell、 键入以下命令，并按 Enter。

`Export-NpsConfiguration –Path c:\config.xml` 

有关详细信息，请参阅[导出 NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。

你已将导出 NPS 配置后，复制到目标服务器 XML 文件。

导入 NPS 配置目标服务器上的 command 语法如下所示。

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>导入示例

以下命令将设置从 C:\Npsconfig.xml NPS 到指定文件导入。 若要运行以下命令，在目标 NPS 服务器上以管理员身份运行的 Windows PowerShell、 键入以下命令，，按 Enter。

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

有关详细信息，请参阅[导入 NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>导出和导入使用 Netsh 的 NPS 配置

你可以使用网络 Shell \(Netsh\) 使用导出 NPS 服务器配置**netsh nps 导出**命令。

当**netsh nps 导入**运行命令、 NPS 系统更新的配置设置将自动刷新。 不需要在目标计算机运行停止 NPS **netsh nps 导入**命令，但是如果 NPS 控制台或 NPS mmc 贴靠打开配置导入期间，服务器配置更改才会可见刷新视图。 

>[!NOTE]
>当你使用**netsh nps 导出**命令时，你必须提供命令参数**exportPSK**使用值**是**。 此参数和明确说明，了解你要导出 NPS 服务器配置中，并将导出的 XML 文件包含解密 RADIUS 客户端和远程 RADIUS 服务器组成员共享的机密信息。

**管理凭据**

若要完成此过程，必须是管理员组中的成员。

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>若要将 NPS 服务器配置复制到另一台 NPS 服务器使用 Netsh 命令

1. 在源 NPS 服务器上，打开**权限的命令提示符**，类型**netsh**，然后按 Enter。

2. 在**netsh**提示中，键入**nps**，然后按 Enter。 

3. 在**netsh nps**提示中，键入**导出 filename =**"*path\file.xml*" **exportPSK = 是**，其中*路径*是你想要保存 NPS 服务器配置文件的文件夹位置和*文件*是你想要保存的 XML 文件的名称。 按 Enter。 

这会存储配置设置 \(including registry settings\) XML 文件中。 路径可以相对或绝对，也可以是通用命名约定 \(UNC\) 路径。 按 enter 键后，将显示一条消息，指示是否成功导出到一个文件。

4. 将你创建的文件复制到目标 NPS 服务器。

5. 在命令提示符目标 NPS 服务器上，键入**netsh nps 导入 filename =**"*path\file.xml*"，然后按 Enter。 显示一条消息，指示是否已成功导入从 XML 文件。

Netsh 有关详细信息，请参阅[网络 Shell (Netsh)](../netsh/netsh.md)。

