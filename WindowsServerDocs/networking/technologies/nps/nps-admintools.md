---
title: 使用管理工具网络策略服务器管理
description: 你可以使用本主题以了解可用于管理网络策略服务器，在 Windows Server 2016 的工具。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>使用管理工具网络策略服务器管理

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解可用于管理 NPS 服务器的工具。

安装 NPS 后，你可以管理 NPS 服务器：

- 本地，通过使用 NPS NPS Microsoft 管理控制台 \(MMC\) 贴靠中、静态 NPS 控制台在管理工具、Windows PowerShell 命令或网络 Shell \(Netsh\) 命令。
- 从远程 NPS 服务器、NPS，或远程桌面连接通过 NPS mmc 贴靠、NPS Netsh 的命令、Windows PowerShell 命令。
- 来自远程工作站，其他工具，例如 NPS MMC 或 Windows PowerShell 结合使用远程桌面连接。

>[!NOTE]
>Windows Server 2016 中，您可以通过使用控制台 NPS 管理本地 NPS 服务器。 若要管理远程和本地 NPS 服务器，必须使用 NPS MMC snap\ 中。

以下部分提供有关如何管理你的本地或远程 NPS 服务器的说明进行操作。

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>通过使用控制台 NPS 配置本地 NPS 服务器

你已安装 NPS 后，可以使用此过程使用 NPS MMC 管理本地 NPS 服务器。

**管理凭据** 

若要完成此过程，必须是管理员组中的成员。

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>若要使用 NPS 控制台配置本地 NPS 服务器

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。

2. 在 NPS 控制台中，单击 NPS \(Local\)。 在详细信息窗格中，选择**标准配置**或**高级配置**，然后执行根据你选择以下选项之一：
    - 如果你选择**标准配置**从列表中，选择一种情况，然后按照说明进行操作以启动配置向导。
    - 如果你选择**高级配置**，单击箭头以展开**高级配置选项**，然后查看和配置根据所需的 NPS 功能 RADIUS 服务器、RADIUS 代理或两者的可用选项。

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>使用 NPS MMC Snap\ 中管理多个 NPS 服务器

可以使用此步骤使用 NPS MMC snap\ 中管理本地 NPS 服务器和多个远程 NPS 服务器。

执行以下步骤之前, 你必须安装 NPS 和远程计算机上本地计算机。

根据我们网络条件和您使用 NPS MMC snap\ 中管理 NPS 服务器的数量的 snap\ 中 MMC 响应可能 slow。 此外，NPS server 配置交通是通过网络发送远程管理会话期间使用 snap\ 中 NPS。 确保你的网络的物理安全性和恶意用户没有到该网络通信的访问权限。

**管理凭据** 

若要完成此过程，必须是管理员组中的成员。

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>若要使用 snap\ 中 NPS 管理多个 NPS 服务器

1. 若要打开 MMC，请以管理员身份运行的 Windows PowerShell。 在 Windows PowerShell 中，键入**mmc**，然后按 ENTER。 打开 Microsoft 管理控制台。
2. 在 MMC，在**文件**菜单上，单击**Snap\ 中添加/删除**。 **添加或删除 Snap\ 单元**对话框中打开。
3. 在**添加或删除 Snap\ 单元**中**可用 snap\ 接**、向下滚动列表、单击**网络策略服务器**，然后单击**添加**。 **选择计算机**对话框中打开。
4. 在**选择计算机**，确认**本地计算机 \（的计算机上的此控制台 running\）**选中，则，然后单击**确定**。 适用于本地 NPS server snap\ 接将添加到列表中**选择 snap\ 接**。
5. 在**添加或删除 Snap\ 单元**中**可用 snap\ 接**，确保**网络策略服务器**是静物选择，然后单击**添加**。 **选择计算机**再次打开对话框。
6. 在**选择计算机**，单击**另一台计算机**，然后键入 IP 地址或完整的域命名 \(FQDN\) 你想要使用 snap\ 中 NPS 管理远程 NPS 服务器。 或者，你可以单击**浏览**详细的计算机，你想要添加的目录。 单击**确定**。
7. 重复步骤 5 和 6 向 snap\ 中 NPS 添加更多 NPS 服务器。 添加完要进行管理，请单击的所有 NPS 服务器**确定**。
8. 若要以供以后使用保存 NPS 贴靠，请单击**文件**，然后单击**保存**。 在**另存为**对话框中，浏览要保存文件中，键入你的 Microsoft 管理控制台 \(.msc\) 文件的名称，然后单击硬盘位置**保存**。 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>使用远程桌面连接通过管理 NPS 服务器

可以使用此步骤使用远程桌面连接通过管理 NPS 远程服务器。

通过使用远程桌面连接，你可以远程管理运行 Windows Server 2016 的 NPS 服务器。 你还远程可以从一台运行 Windows 10 或早期版本的 Windows 客户端操作系统计算机管理 NPS 服务器。

你可以使用远程桌面连接通过使用两种方法之一管理多个 NPS 服务器。

1. 单独创建远程桌面连接到每个 NPS 服务器。
2. 使用远程桌面连接到一个 NPS 服务器，，然后使用在该服务器 NPS MMC 管理其他远程服务器。 详细信息，请参阅上文**使用 NPS MMC Snap\ 中管理多个 NPS 服务器**。

**管理凭据** 

若要完成此过程，你必须是管理员组 NPS 服务器上的成员。

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>使用远程桌面连接通过管理 NPS 服务器

1. 在你希望管理远程服务器管理器中的每个 NPS 服务器上选择**本地服务器**。 在服务器管理器的详细信息窗格中查看**远程桌面**设置，然后执行下列情况之一。 
    1. 如果的值**远程桌面**设置是**启用**，不需要在此过程中执行一些步骤。 第 4 步以启动配置远程桌面用户权限下跳过。
    2. 如果**远程桌面**设置是**禁用**，请单击的单词**禁用**。 **系统属性**上打开对话框**远程**选项卡。
2. 在**远程桌面**，单击**允许远程连接到此计算机**。 **远程桌面连接**对话框中打开。 执行以下一项。
    1. 若要自定义允许网络连接，请单击**高级安全 Windows 防火墙**，然后将配置你想要允许的设置。 
    2. 若要为所有网络上计算机的连接，请启用远程桌面连接，请单击**确定**。
3. 在**系统属性**中**远程桌面**，决定是否启用**允许仅的计算机运行带网络级身份验证的远程桌面连接**，并使您的选择。
4. 单击**选择用户**。 **远程桌面用户**对话框中打开。
5. 在**远程桌面用户**，若要远程连接到 NPS 服务器，请单击的用户授予权限**添加**，然后键入的用户帐户的用户名。 单击**确定**。
6. 每个用户您要为其允许远程访问权限的 NPS 服务器重复步骤 5。 当你完成后添加的用户时，单击**确定**关闭**远程桌面用户**对话框和**确定**来关闭**系统属性**对话框。
7. 若要连接到你之前的步骤，通过配置远程的 NPS 服务器，请单击**开始**、按字母顺序排列的列表中向下滚动，然后单击**Windows 附件**，然后单击**远程桌面连接**。 **远程桌面连接**对话框中打开。
8. 在**远程桌面连接**对话框中，在**计算机**，键入 NPS 服务器名称或 IP 地址。 如果你愿意，请单击**选项**、配置其他连接选项，然后单击**保存**保存的重复使用的连接。
9. 单击**连接**，并在出现提示时提供的用户帐户凭据帐户具有权限，若要登录到和配置 NPS 服务器。

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>用于管理 NPS 服务器 Netsh NPS 命令

使用 Netsh NPS 上下文中的命令，才可以显示并记帐和审核使用 NPS 和远程访问服务数据库设置身份验证、的配置。 使用到 Netsh NPS 上下文中的命令：

- 配置或重新配置 NPS 服务器，包括 NPS 那些也可供配置的 Windows 界面中使用 NPS 主机的所有方面。
- 出口一个 NPS 服务器（源服务器），包括注册表项和 NPS 配置应用商店中的为 Netsh 脚本的配置。
- 使用脚本 Netsh 和源 NPS 服务器的已导出的配置文件导入到另一个 NPS 服务器配置。

你可以从 Windows Server 2016 的命令提示符或 Windows PowerShell 运行以下命令。 脚本和批量文件中，你也可以运行 netsh nps 命令。

**管理凭据** 

若要执行此步骤，必须是管理员组本地计算机上的成员。

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>输入 Netsh NPS 上下文 NPS 服务器上

1. 打开 Command Prompt 或 Windows PowerShell。
2. 键入**netsh**，然后按 ENTER。
3. 键入**nps**，然后按 ENTER。
4. 若要查看适用命令列表，请键入问号 \(?\)，然后按 ENTER。


Netsh NPS 命令的详细信息，请参阅[用于在 Windows Server 2008 网络策略服务器 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或下载整个[Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)从 TechNet 库。 此下载是完整网络 Shell 技术参考 Windows Server 2008 和 Windows Server 2008 R2。 格式为 zip 文件中的 Windows 帮助 \(*.chm\)。 这些命令是 Windows Server 2016 和 Windows 10 中仍然存在，以便你可以使用这些环境，netsh，建议使用 Windows PowerShell 尽管。

## <a name="use-windows-powershell-to-manage-nps-servers"></a>使用 Windows PowerShell 管理 NPS 服务器

你可以使用 Windows PowerShell 命令管理 NPS 服务器。 有关详细信息，请参阅下面的 Windows PowerShell 命令参考主题。

- [网络策略 (NPS) 服务器 Cmdlet 的 Windows PowerShell](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 你可以使用这些 netsh 命令，在 Windows Server 2012 R2 或更高版本操作系统。
- [NPS 模块](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 在 Windows Server 2016，可以使用这些 netsh 命令。

有关 NPS 管理的详细信息，请参阅[管理网络策略 Server (NPS)](nps-manage-top.md)。
