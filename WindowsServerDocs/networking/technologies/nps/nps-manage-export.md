---
title: 将 NPS 配置的另一台服务器上的导入导出
description: 可以使用本主题，了解如何导出 Windows Server 2016 中的网络策略服务器配置。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b95e39af63e284d0147335faabfb740c0dd175bc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812302"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>将 NPS 配置的另一台服务器上的导入导出

适用于：Windows Server 2016

可以导出整个 NPS 配置 — 包括 RADIUS 客户端和服务器、 网络策略、 连接请求策略、 注册表和日志记录配置 — 从另一个 NPS 上导入一个 NPS。 

使用以下工具之一导出 NPS 配置：

- 在 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中，你可以使用 Netsh，或可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008 中，使用 Netsh。

> [!IMPORTANT]
> 如果源 NPS 数据库具有更高版本的版本号高于目标 NPS 数据库的版本号，则不使用此过程。 您可以查看从显示的 NPS 数据库的版本号**netsh nps 显示配置**命令。

由于 NPS 配置未加密在导出的 XML 文件中，通过网络发送可能会带来安全风险，因此将 XML 文件从源服务器移到目标服务器时采取预防措施。 例如，将文件添加到加密的密码受保护的存档文件移动文件之前。 此外，将文件存储在安全位置，以防止恶意用户对其进行访问。

> [!NOTE]
> 如果在源 NPS 上配置 SQL Server 日志记录，SQL Server 日志记录设置都不会导出到 XML 文件。 导入另一个 NPS 上的文件后，必须手动配置 SQL Server 日志记录。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>导出和导入 NPS 配置使用 Windows PowerShell

对于 Windows Server 2012 及更高版本的操作系统版本，您可以导出 NPS 配置使用 Windows PowerShell。

导出 NPS 配置的命令语法如下所示。 

    Export-NpsConfiguration -Path <filename>

下表列出了参数**导出 NpsConfiguration**在 Windows PowerShell cmdlet。 以粗体显示的参数是必需的。

|参数|描述|
|---------|-----------|
|路径|指定的名称和你想要导出 NPS 配置 XML 文件的位置。|

**管理凭据**

若要完成此过程，必须是 Administrators 组的成员。

### <a name="export-example"></a>导出示例 

在以下示例中，NPS 配置导出到 XML 文件位于本地驱动器上。 若要运行此命令，在源 NPS 上以管理员身份运行 Windows PowerShell 中，键入以下命令，然后按 Enter。

`Export-NpsConfiguration –Path c:\config.xml` 

有关详细信息，请参阅[导出 NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。

你已导出 NPS 配置后，将 XML 文件复制到目标服务器。

导入目标服务器上的 NPS 配置的命令语法如下所示。

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>导入示例

以下命令从名为 C:\Npsconfig.xml 到 NPS 的文件导入设置。 若要运行此命令，目标 NPS 上以管理员身份运行 Windows PowerShell 中，键入以下命令，然后按 Enter。

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

有关详细信息，请参阅[导入 NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>导出和导入 NPS 配置使用 Netsh

可以使用网络 Shell \(Netsh\)若要使用导出 NPS 配置**netsh nps 导出**命令。

当**netsh nps 导入**运行命令时，NPS 将自动刷新与更新的配置设置。 不需要在要运行的目标计算机上停止 NPS **netsh nps 导入**命令，但是如果在 NPS 控制台或 NPS MMC 管理单元中打开配置导入过程中，更改服务器配置为不可见之前刷新视图。 

> [!NOTE]
> 当你使用**netsh nps 导出**命令时，需要提供的命令参数**exportPSK**的值**是**。 此参数和值显式声明了解要导出 NPS 配置，以及导出的 XML 文件包含未共享的机密加密 RADIUS 客户端和远程 RADIUS 服务器组的成员。

**管理凭据**

若要完成此过程，必须是 Administrators 组的成员。

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>若要将 NPS 配置复制到另一个 NPS 使用 Netsh 命令

1. 在源 NPS 上，打开**命令提示符**，类型**netsh**，然后按 Enter。

2. 在**netsh**提示符下，键入**nps**，然后按 Enter。 

3. 在**netsh nps**提示符下，键入**导出 filename =** "*path\file.xml*" **exportPSK = YES**，其中*路径*是你想要保存 NPS 配置文件的文件夹位置并*文件*是你想要保存的 XML 文件的名称。 按 Enter。 

此存储配置设置\(包括注册表设置\)XML 文件中。 路径可以是相对或绝对的也可以是通用命名约定\(UNC\)路径。 按 enter 键后，会显示一条消息，指示是否已成功导出到文件。

4. 将你创建的文件复制到目标 NPS。

5. 在目标 NPS 上的命令提示符，键入**netsh nps 导入 filename =** "*path\file.xml*"，然后按 Enter。 出现一条消息，指示是否已成功从 XML 文件导入。

有关 netsh 详细信息，请参阅[Network Shell (Netsh)](../netsh/netsh.md)。

