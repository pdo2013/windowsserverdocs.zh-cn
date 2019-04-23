---
title: 网络策略服务器管理与管理工具
description: 可以使用本主题以了解可用于管理 Windows Server 2016 中的网络策略服务器的工具。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856878"
---
# <a name="network-policy-server-management-with-administration-tools"></a>网络策略服务器管理与管理工具

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解可用于管理你 NPSs 的工具。

安装 NPS 后，你可以管理 NPSs:

- 通过使用 NPS Microsoft 管理控制台在本地， \(MMC\)管理单元、 管理工具、 Windows PowerShell 命令或 Network Shell 中的静态 NPS 控制台\(Netsh\) NPS 的命令。
- 从远程 NPS，通过使用 NPS MMC 管理单元中，NPS 的 Netsh 命令，Windows PowerShell 命令为 NPS 中或远程桌面连接。
- 从远程工作站，与其他工具，如 NPS MMC 或 Windows PowerShell 结合使用远程桌面连接。

>[!NOTE]
>在 Windows Server 2016 中，可以使用 NPS 控制台来管理本地 NPS。 若要管理远程和本地 NPSs，必须使用 NPS MMC 管理单元\-中。

以下各节提供有关如何管理本地和远程 NPSs 说明。

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>使用 NPS 控制台配置本地 NPS

安装 NPS 后，可以使用此过程来通过使用 NPS MMC 来管理本地 NPS。

**管理凭据** 

若要完成此过程，必须是 Administrators 组的成员。

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>若要使用 NPS 控制台配置本地 NPS

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。

2. 在 NPS 控制台中，单击 NPS\(本地\)。 在详细信息窗格中，选择**标准配置**或**高级配置**，然后执行下列任一基于你的选择：
    - 如果愿意**标准配置**，从列表中，选择一种方案，然后按照说明进行操作以启动配置向导。
    - 如果愿意**高级配置**，单击箭头以展开**高级配置选项**，，然后查看和配置可用的选项根据所需的 NPS 功能RADIUS 服务器、 RADIUS 代理，或两者。

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>使用 NPS MMC 管理单元来管理多个 NPSs\-中

可以使用此过程通过使用 NPS MMC 管理单元来管理本地 NPS 和多个远程 NPSs\-中。

然后再执行下面的过程，必须在本地计算机和远程计算机上安装 NPS。

可以使用 NPS MMC 管理单元来管理具体取决于网络条件和 NPSs 数\-响应的 MMC 管理单元中\-中可能会很慢。 此外，NPS 配置流量通过网络发送的远程管理会话期间使用 NPS 管理单元\-中。 请确保你的网络是物理上安全和恶意用户无权对此网络流量的访问。

**管理凭据** 

若要完成此过程，必须是 Administrators 组的成员。

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>若要使用 NPS 管理单元来管理多个 NPSs\-中

1. 若要打开 MMC，请以管理员身份运行 Windows PowerShell。 在 Windows PowerShell 中，键入**mmc**，然后按 ENTER。 将打开 Microsoft 管理控制台。
2. 在 MMC 中，在**文件**菜单上，单击**添加/删除管理单元\-中**。 **添加或删除管理单元\-项**对话框随即打开。
3. 在**添加或删除管理单元\-项**，在**可用管理单元\-项**，向下滚动列表中，单击**网络策略服务器**，然后单击**添加**。 **选择计算机**对话框随即打开。
4. 在中**选择计算机**，确认**本地计算机\(在其运行此控制台的计算机\)** 已选择，然后单击**确定**。 在管理单元\-在本地 nps 添加到列表中**选定管理单元\-项**。
5. 在**添加或删除管理单元\-项**，在**可用管理单元\-项**，，请确保**网络策略服务器**仍处于选中状态，然后依次**添加**。 **选择计算机**对话框将再次打开。
6. 在中**选择计算机**，单击**另一台计算机**，然后键入 IP 地址或完全限定的域名\(FQDN\)的你想要使用 NPS 管理在远程 NPS对齐\-中。 （可选） 可以单击**浏览**细读你想要添加的计算机的目录。 单击 **“确定”**。
7. 重复步骤 5 和 6 向 NPS 管理单元添加多个 NPSs\-中。 添加完所有你想要管理，请单击 NPSs**确定**。
8. 若要保存 NPS 管理单元中以供将来使用，请单击**文件**，然后单击**保存**。 在中**另存为**对话框中，浏览到要保存该文件的硬盘位置键入你的 Microsoft 管理控制台的名称\(.msc\)文件，然后依次**保存**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>使用远程桌面连接来管理 NPS

此过程可用于通过使用远程桌面连接来管理远程 NPS。

通过使用远程桌面连接，可以远程管理运行 Windows Server 2016 在 NPSs。 您可以从运行 Windows 10 或更早的 Windows 客户端操作系统的计算机还远程管理 NPSs。

可以使用远程桌面连接来管理多个 NPSs 使用两种方法之一。

1. 单独创建远程桌面连接到每个你 NPSs。
2. 使用远程桌面连接到一个 NPS，然后使用该服务器上 NPS MMC 来管理其他远程服务器。 有关详细信息，请参阅上一节**通过使用 NPS MMC 管理单元管理多个 NPSs\-中**。

**管理凭据** 

若要完成此过程，您必须在 NPS 上的 Administrators 组的成员。

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>若要通过使用远程桌面连接来管理 NPS

1. 你想要远程管理在服务器管理器中，每个 NPS 上选择**本地服务器**。 在服务器管理器详细信息窗格中，查看**远程桌面**设置，然后执行下列任一操作。 
    1. 如果的值**远程桌面**设置为**已启用**，不需要执行此过程中的某些步骤。 跳到步骤 4，以开始配置远程桌面用户的权限。
    2. 如果**远程桌面**设置为**禁用**，单击单词**禁用**。 **系统属性**上打开对话框**远程**选项卡。
2. 在中**远程桌面**，单击**允许远程连接到此计算机**。 **远程桌面连接**对话框随即打开。 请执行以下操作之一。
    1. 若要自定义允许的网络连接，请单击**高级安全 Windows 防火墙**，然后配置你想要允许的设置。 
    2. 若要启用远程桌面连接，所有网络连接的计算机上，单击**确定**。
3. 在中**系统属性**，在**远程桌面**，决定是否启用**允许仅从计算机运行带网络级身份验证远程桌面连接**，并使你的选择。
4. 单击**选择用户**。 **Remote Desktop Users**对话框随即打开。
5. 在中**Remote Desktop Users**，可将权限授予用户远程连接到 NPS 中，单击**添加**，然后键入用户的帐户的用户名。 单击 **“确定”**。
6. 为你要为其授予向 NPS 的远程访问权限的每个用户重复步骤 5。 完成添加用户后，单击**确定**以关闭**Remote Desktop Users**对话框并**确定**以关闭**系统属性**对话框。
7. 若要连接到使用前面的步骤配置远程 NPS，请单击**启动**，按字母顺序排列的列表中向下滚动，然后单击**Windows Accessories**，然后单击**远程桌面连接**。 **远程桌面连接**对话框随即打开。
8. 在中**远程桌面连接**对话框中**计算机**，键入 NPS 名称或 IP 地址。 如果您愿意，请单击**选项**，配置其他连接选项，然后单击**保存**以保存连接以便重复使用。
9. 单击**Connect**，并出现提示时提供用户帐户凭据有权登录到以及将 NPS 配置的帐户。

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>使用 Netsh NPS 命令管理 NPS

可以使用 Netsh NPS 上下文中的命令以显示和设置的配置验证、 授权、 记帐和审核数据库使用 NPS 和远程访问服务。 使用 Netsh NPS 上下文中的命令：

- 配置或重新配置的 NPS，包括通过 Windows 界面中使用 NPS 控制台还有可用于配置的 NPS 的所有方面。
- 导出一个 NPS （源服务器），其中包括注册表项和 NPS 配置存储，作为 Netsh 脚本的配置。
- 通过使用来自源 NPS 的 Netsh 脚本，并导出的配置文件导入到另一个 NPS 配置。

从 Windows Server 2016 命令提示符处或从 Windows PowerShell，可以运行这些命令。 此外可以在脚本和批处理文件中运行 nps 的 netsh 命令。

**管理凭据** 

若要执行此过程，您必须是本地计算机 Administrators 组的成员。

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>在 NPS 上输入 Netsh NPS 上下文

1. 打开命令提示符或 Windows PowerShell。
2. 类型**netsh**，然后按 ENTER。
3. 类型**nps**，然后按 ENTER。
4. 若要查看可用命令的列表，请键入问号\(？\)然后按 ENTER。


有关 Netsh NPS 命令的详细信息，请参阅[用于在 Windows Server 2008 中的网络策略服务器的 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或下载整个[Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)TechNet 库中。 此下载是完整的网络 Shell 技术参考为 Windows Server 2008 和 Windows Server 2008 R2。 格式是 Windows 帮助\(*.chm\) zip 文件中。 这些命令是在 Windows Server 2016 和 Windows 10 中仍然存在，因此你可以使用 netsh 在这些环境中，但建议使用 Windows PowerShell。

## <a name="use-windows-powershell-to-manage-npss"></a>使用 Windows PowerShell 管理 NPSs

可以使用 Windows PowerShell 命令来管理 NPSs。 有关详细信息，请参阅以下 Windows PowerShell 命令参考主题。

- [网络策略服务器 (NPS) Cmdlet 在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 可以使用这些 Windows Server 2012 R2 或更高版本操作系统中的 netsh 命令。
- [NPS 模块](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 在 Windows Server 2016 中，可以使用以下 netsh 命令。

有关 NPS 管理的详细信息，请参阅[管理网络策略服务器 (NPS)](nps-manage-top.md)。
