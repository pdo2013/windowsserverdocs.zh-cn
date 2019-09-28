---
title: 网络策略服务器管理与管理工具
description: 你可以使用本主题来了解可用于管理 Windows Server 2016 中的网络策略服务器的工具。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 527fbf52d68f36d198068514476868bcba930a68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396448"
---
# <a name="network-policy-server-management-with-administration-tools"></a>网络策略服务器管理与管理工具

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解可用于管理 NPSs 的工具。

安装 NPS 后，可以管理 NPSs：

- 在本地，通过使用 NPS Microsoft 管理控制台 \(MMC @ no__t 管理单元、管理工具中的静态 NPS 控制台、Windows PowerShell 命令或用于 NPS 的网络 Shell \(Netsh @ no__t-3 命令。
- 通过使用 NPS MMC 管理单元、用于 NPS 的 Netsh 命令、用于 NPS 的 Windows PowerShell 命令或远程桌面连接。
- 从远程工作站使用远程桌面连接与其他工具（例如 NPS MMC 或 Windows PowerShell）结合使用。

>[!NOTE]
>在 Windows Server 2016 中，可以使用 NPS 控制台来管理本地 NPS。 若要同时管理远程和本地 NPSs，必须使用 NPS MMC snap @ no__t-0in。

以下各节提供有关如何管理本地和远程 NPSs 的说明。

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>使用 NPS 控制台配置本地 NPS

安装了 NPS 后，可以使用此过程通过使用 NPS MMC 来管理本地 NPS。

**管理凭据** 

若要完成此过程，您必须是 Administrators 组的成员。

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>使用 NPS 控制台配置本地 NPS

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。

2. 在 NPS 控制台中，单击 "NPS \(Local @ no__t-1"。 在详细信息窗格中，选择 "**标准配置**" 或 "**高级配置**"，然后根据你的选择执行以下操作之一：
    - 如果选择 "**标准配置**"，请从列表中选择一个方案，然后按照说明启动配置向导。
    - 如果选择 "**高级配置**"，请单击箭头展开 "**高级配置选项**"，然后根据所需的 NPS 功能（radius 服务器、radius 代理）查看和配置可用选项。

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>使用 NPS MMC Snap @ no__t-0in 管理多个 NPSs

可以使用此过程通过使用 NPS MMC snap @ no__t-0in 来管理本地 NPS 和多个远程 NPSs。

在执行以下过程之前，必须在本地计算机和远程计算机上安装 NPS。

根据网络条件和使用 NPS MMC snap @ no__t-0in 管理的 NPSs 数，MMC snap @ no__t-1in 的响应可能会很慢。 此外，通过使用 NPS snap @ no__t-0in 在远程管理会话过程中通过网络发送 NPS 配置流量。 确保你的网络在物理上是安全的，并且恶意用户无权访问此网络流量。

**管理凭据** 

若要完成此过程，您必须是 Administrators 组的成员。

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>使用 NPS snap @ no__t-0in 管理多个 NPSs

1. 若要打开 MMC，请以管理员身份运行 Windows PowerShell。 在 Windows PowerShell 中，键入**mmc**，然后按 enter。 将打开 Microsoft 管理控制台。
2. 在 MMC 的 "**文件**" 菜单上，单击 "**添加/删除 Snap @ no__t-2in**"。 此时将打开 "**添加/删除 Snap @ no__t-1ins** " 对话框。
3. 在 "**添加或删除 snap @ no__t-1ins**" 的 "**可用" snap @ no__t-3ins**中，向下滚动列表，单击 "**网络策略服务器**"，然后单击 "**添加**"。 此时将打开 "**选择计算机**" 对话框。
4. 在 "**选择计算机**" 中，验证是否选择了 **"运行此控制台的本地计算机 \(the 计算机**"，然后单击 **"确定"** 。 将本地 NPS 的 snap @ no__t-0in 添加到**选定的 snap @ no__t-2ins**中的列表。
5. 在 "**添加或删除 snap @ no__t-1ins**" 的 "**可用" snap @ no__t-3ins**中，确保仍选中 "**网络策略服务器**"，然后单击 "**添加**"。 此时将打开 "**选择计算机**" 对话框。
6. 在 "**选择计算机**" 中，单击 "**另一台计算机**"，然后键入要使用 NPS snap @ no__t-4IN 管理的远程 NPS 的 IP 地址或完全限定的域名 @no__t 2FQDN @ no__t）。 还可以单击 "**浏览**" 细读要添加的计算机的目录。 单击 **“确定”** 。
7. 重复步骤5和步骤6，将更多 NPSs 添加到 NPS snap @ no__t-0in。 添加要管理的所有 NPSs 后，单击 **"确定"** 。
8. 若要保存 NPS 管理单元以供以后使用，请单击 "**文件**"，然后单击 "**保存**"。 在 "**另存为**" 对话框中，浏览到要在其中保存该文件的硬盘位置，键入 Microsoft 管理控制台的名称 \( @ no__t-2 文件，然后单击 "**保存**"。 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>使用远程桌面连接管理 NPS

您可以使用此过程通过远程桌面连接来管理远程 NPS。

通过使用远程桌面连接，你可以远程管理运行 Windows Server 2016 的 NPSs。 还可以从运行 Windows 10 或更早版本的 Windows 客户端操作系统的计算机上远程管理 NPSs。

可以使用远程桌面连接通过以下两种方法之一来管理多个 NPSs。

1. 分别创建与每个 NPSs 的远程桌面连接。
2. 使用远程桌面连接到一个 NPS，然后使用该服务器上的 NPS MMC 来管理其他远程服务器。 有关详细信息，请参阅上一节**使用 NPS Mmc Snap @ no__t-1In 管理多个 NPSs**。

**管理凭据** 

若要完成此过程，您必须是 NPS 上 Administrators 组的成员。

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>使用远程桌面连接管理 NPS

1. 在要远程管理的每个 NPS 上，在服务器管理器中，选择 "**本地服务器**"。 在 "服务器管理器详细信息" 窗格中，查看 "**远程桌面**" 设置，然后执行以下操作之一。 
    1. 如果 "**远程桌面**" 设置的值为 "已**启用**"，则无需执行此过程中的某些步骤。 跳到步骤4，开始配置远程桌面用户权限。
    2. 如果 "**远程桌面**" 设置处于**禁用状态**，请单击 "**已禁用**"。 "**系统属性**" 对话框将在 "**远程**" 选项卡上打开。
2. 在**远程桌面**中，单击 "**允许远程连接到此计算机**"。 此时将打开 "**远程桌面连接**" 对话框。 请执行以下操作之一。
    1. 若要自定义允许的网络连接，请单击 "**高级安全 Windows 防火墙**"，然后配置你希望允许的设置。 
    2. 若要为计算机上的所有网络连接启用远程桌面连接，请单击 **"确定"** 。
3. 在 "**系统属性**" 中的 "**远程桌面**" 中，决定是启用 "**仅允许从运行远程桌面的计算机连接" 网络级别身份验证**，然后进行选择。
4. 单击**选择用户**。 "**远程桌面用户**" 对话框将打开。
5. 在 "**远程桌面用户**" 中，若要向用户授予远程连接到 NPS 的权限，请单击 "**添加**"，然后键入用户帐户的用户名。 单击 **“确定”** 。
6. 对于要向其授予对 NPS 的远程访问权限的每个用户，请重复步骤5。 添加完用户后，请单击 **"确定**" 关闭 "**远程桌面用户**" 对话框，然后再次单击 **"确定"** 关闭 "**系统属性**" 对话框。
7. 若要使用前面的步骤连接到已配置的远程 NPS，请单击 "**开始**"，向下滚动按字母顺序排列的列表，然后单击 " **Windows 附件**"，然后单击 "**远程桌面连接**"。 此时将打开 "**远程桌面连接**" 对话框。
8. 在 "**远程桌面连接**" 对话框的 "**计算机**" 中，键入 NPS 名称或 IP 地址。 如果需要，请单击 "**选项**"，配置其他连接选项，然后单击 "**保存**" 以保存连接以重复使用。
9. 单击 "**连接**"，然后在出现提示时，提供有权登录并配置 NPS 的帐户的用户帐户凭据。

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>使用 Netsh NPS 命令管理 NPS

可以使用 Netsh NPS 上下文中的命令显示和设置 NPS 和远程访问服务使用的身份验证、授权、记帐和审核数据库的配置。 使用 Netsh NPS 上下文中的命令来执行以下操作：

- 配置或重新配置 NPS，包括也可通过在 Windows 界面中使用 NPS 控制台进行配置的 NPS 的所有方面。
- 将一个 NPS （源服务器）的配置（包括注册表项和 NPS 配置存储）作为 Netsh 脚本导出。
- 使用 Netsh 脚本和源 NPS 中的导出配置文件，将配置导入到另一个 NPS。

你可以从 Windows Server 2016 命令提示符或 Windows PowerShell 中运行这些命令。 你还可以在脚本和批处理文件中运行 netsh nps 命令。

**管理凭据** 

若要执行此过程，您必须是本地计算机 Administrators 组的成员。

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>在 NPS 上输入 Netsh NPS 上下文

1. 打开 "命令提示符" 或 "Windows PowerShell"。
2. 键入**netsh**，然后按 enter。
3. 键入**nps**，然后按 enter。
4. 若要查看可用命令的列表，请键入问号 \(？ \)，然后按 ENTER。


有关 Netsh NPS 命令的详细信息，请参阅[Windows Server 2008 中的网络策略服务器的 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或从 TechNet 库下载完整的[netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)。 此下载是适用于 Windows Server 2008 和 Windows Server 2008 R2 的完整网络 Shell 技术参考。 格式为 Windows 帮助 \( * .chm @ no__t-1 在 zip 文件中。 这些命令仍然存在于 Windows Server 2016 和 Windows 10 中，因此你可以在这些环境中使用 netsh，但建议使用 Windows PowerShell。

## <a name="use-windows-powershell-to-manage-npss"></a>使用 Windows PowerShell 管理 NPSs

可以使用 Windows PowerShell 命令来管理 NPSs。 有关详细信息，请参阅下面的 Windows PowerShell 命令参考主题。

- [Windows PowerShell 中的网络策略服务器（NPS） cmdlet](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 你可以在 Windows Server 2012 R2 或更高版本的操作系统中使用这些 netsh 命令。
- [NPS 模块](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 可以使用 Windows Server 2016 中的这些 netsh 命令。

有关 NPS 管理的详细信息，请参阅[管理网络策略服务器（NPS）](nps-manage-top.md)。
