---
title: 远程服务器管理工具
description: 远程服务器管理工具的顶部级别主题
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 30ca0a1e8a2f17f54a8f05d7270bf9512be7a8dc
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805185"
---
# <a name="remote-server-administration-tools"></a>远程服务器管理工具

>适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题支持远程服务器管理工具适用于 Windows 10。

> [!IMPORTANT]
> 随着 Windows 启动 10 2018 年 10 月更新，RSAT 是包含为一系列**按需功能**本身的 Windows 10 中。 请参阅**何时使用 RSAT 版本**下面有关安装说明。

RSAT 可让 IT 管理员从 Windows 10 电脑管理 Windows Server 角色和功能。

远程服务器管理工具包括服务器管理器，Microsoft 管理控制台 (mmc) 管理单元、 控制台、 Windows PowerShell cmdlet 和提供程序，以及管理角色和功能的 Windows Server 上运行一些命令行工具。

远程服务器管理工具包括 Windows PowerShell cmdlet 模块，它们可用于管理角色和功能的远程服务器上运行。 尽管默认情况下，Windows Server 2016 上已启用 Windows PowerShell 远程管理，但它未启用 Windows 10 上的默认情况下。 若要运行是针对远程服务器的远程服务器管理工具一部分的 cmdlet，请运行`Enable-PSremoting`使用之后在 Windows 客户端计算机上提升的用户权限 （即，以管理员身份运行） 打开的 Windows PowerShell 会话中安装远程服务器管理工具。

## <a name="BKMK_Thresh"></a>适用于 Windows 10 的远程服务器管理工具
使用远程服务器管理 Windows 10 的工具来管理运行 Windows Server 2016 中，Windows Server 2012 R2 的计算机上和在有限情况下，Windows Server 2012 或 Windows Server 2008 R2 中的特定技术。

远程服务器管理 Windows 10 的工具包括对运行服务器核心安装选项或最精简服务器界面配置的 Windows Server 2016 中，Windows Server 2012 R2，并在受限的计算机的远程管理的支持情况下，Windows Server 2012 的服务器核心安装选项。 但是，不能在任何版本的 Windows Server 操作系统上安装远程服务器管理工具适用于 Windows 10。

### <a name="tools-available-in-this-release"></a>此版本中的可用工具
远程服务器管理工具适用于 Windows 10 中提供的工具的列表，请参阅中的表[远程服务器管理工具 (RSAT) 的 Windows 操作系统](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

### <a name="system-requirements"></a>系统要求
可以仅在运行 Windows 10 的计算机上安装远程服务器管理工具适用于 Windows 10。 不能在运行 Windows RT 8.1 或其他片上系统设备的计算机上安装远程服务器管理工具。

远程服务器管理工具适用于 Windows 10 的 Windows 10 的基于 x86 和基于 x64 的版本上运行。

> [!IMPORTANT]
> 不应在正在运行 Windows 8.1、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 或 Windows 2000 Server 管理工具包的计算机上安装远程服务器管理工具适用于 Windows 10。 删除所有旧版本的管理工具包或者远程服务器管理工具，包括早期预发布版本中，并从计算机的不同语言或区域设置的工具版本，然后再安装远程服务器管理适用于 Windows 10 的工具。

若要使用此版本的服务器管理器中访问和管理运行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的远程服务器，则必须安装一些更新，以便旧版本的 Windows Server 操作系统可通过使用 Se进行服务器管理器。 有关如何在远程服务器管理工具适用于 Windows 10 中使用服务器管理器管理的准备 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 的详细信息，请参阅[Manage Multiple，Remote Servers使用服务器管理器](https://technet.microsoft.com/library/hh831456.aspx)。

若要使用属于远程服务器管理工具适用于 Windows 10 的工具管理，必须在远程服务器上启用 Windows PowerShell 和服务器管理器远程管理。 在运行 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的服务器上默认启用远程管理。 有关如何在已禁用远程管理的情况下启用它的详细信息，请参阅 [使用服务器管理器管理多台远程服务器](https://go.microsoft.com/fwlink/p/?LinkId=241358)。

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>安装、 卸载和禁用/RSAT 工具启用

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>使用按需 (FoD) 上 Windows 10 2018 年 10 月安装特定的 RSAT 工具的功能更新，或更高版本

随着 Windows 启动 10 2018 年 10 月更新，RSAT 是包含为一系列**按需功能**直接从 Windows 10。 现在，而不是下载 RSAT 程序包可以只需转到**管理可选功能**中**设置**然后单击**添加一项功能**若要查看可用的 RSAT 工具的列表。 选择并安装所需的特定 RSAT 工具。 若要查看安装进度，请单击**回**按钮以上查看状态**管理可选功能**页。

请参阅[列表中的 RSAT 工具可通过**按需功能**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了通过图形安装之外**设置**应用程序中，你还可以安装特定的 RSAT 工具通过命令行或使用自动化[ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)。

按需功能的优点之一是在 Windows 10 版本升级之间保持已安装的功能 ！

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>若要卸载特定 RSAT 工具在 Windows 10 2018 年 10 月更新或更高版本 （安装后使用 FoD）

在 Windows 10 上打开**设置**应用程序中，转到**管理可选功能**、 选择和卸载你想要删除的特定 RSAT 工具。 请注意，在某些情况下，你将需要手动卸载依赖关系。 具体而言，如果需要 RSAT 工具的 RSAT 工具 B，然后选择要卸载 RSAT 工具的将失败如果仍安装 RSAT 工具 B。 首先，在这种情况下，卸载 RSAT 工具 B，然后卸载 RSAT 工具 a。另请注意，在某些情况下，卸载 RSAT 工具可能看起来是成功即使仍然安装该工具。 在这种情况下，重新启动 PC 将在完成删除该工具。

请参阅[RSAT 工具包括依赖项的列表](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了卸载通过图形设置应用程序，也可以卸载特定 RSAT 工具通过命令行或使用自动化[ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)。

### <a name="when-to-use-which-rsat-version"></a>何时使用 RSAT 版本

如果必须在 2018 年 10 月之前的 Windows 10 的更新 (1809) 的版本，你将不能使用**按需功能**。 你将需要下载并安装 RSAT 程序包。

- **上面所述直接从 Windows 10 中，安装 RSAT FODs**:在 Windows 上安装时 10 2018 年 10 月更新 (1809) 或更高版本，用于管理 Windows Server 2019 或以前的版本。

- **下载并安装 WS_1803 RSAT 程序包，如下所述**:在 Windows 上安装时 10 2018 年 4 月更新 (1803) 或更早版本，用于管理 Windows Server、 1803年版或 Windows Server 版本 1709年。

- **下载并安装 WS2016 RSAT 程序包，如下所述**:在 Windows 上安装时 10 2018 年 4 月更新 (1803) 或更早版本，用于管理 Windows Server 2016 或以前的版本。

#### <a name="BKMK_installthresh"></a>下载要安装远程服务器管理工具适用于 Windows 10 的 RSAT 程序包

1.  立即下载从远程服务器管理工具适用于 Windows 10 软件包[Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281)。 你可以从下载中心网站运行安装程序，或者将下载包保存到本地计算机或共享。

    > [!IMPORTANT]
    > 只能在运行 Windows 10 的计算机上安装远程服务器管理工具适用于 Windows 10。 不能在运行 Windows RT 8.1 或其他片上系统设备的计算机上安装远程服务器管理工具。

2.  如果你将下载包保存到本地计算机或共享，则根据要安装工具的计算机的体系结构，双击安装程序 **WindowsTH-KB2693643-x64.msu** 或 **WindowsTH-KB2693643-x86.msu**。

3.  当 **“Windows 更新独立安装程序”** 对话框提示你安装更新时，单击 **“是”** 。

4.  阅读并接受许可条款。 单击 **“我接受”** 。

5.  安装需要几分钟才能完成。

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>若要卸载适用于 Windows 10 （安装完成后 RSAT 程序包） 的远程服务器管理工具

1. 在桌面上，依次单击“开始”  、“所有应用”  、“Windows 系统”  和“控制面板”  。

2. 在“程序”  下，单击“卸载程序”  。

3. 单击 **“查看已安装的更新”** 。

4. 右键单击“Microsoft Windows 的更新(KB2693643)”  ，然后单击“卸载”  。

5. 当系统询问你是否确定要卸载更新时，单击 **“是”** 。
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>若要关闭特定工具 （在安装 RSAT 程序包） 之后

6. 在桌面上，依次单击“开始”  、“所有应用”  、“Windows 系统”  和“控制面板”  。

7. 单击 **“程序”** ，然后在 **“程序和功能”** 中单击 **“打开或关闭 Windows 功能”** 。

8. 在 **“Windows 功能”** 对话框中，展开 **“远程服务器管理工具”** ，然后展开 **“角色管理工具”** 或 **“功能管理工具”** 。

9. 清除要关闭的工具对应的复选框。

   > [!NOTE]
   > 如果关闭服务器管理器，必须重新启动计算机，并从可用**工具**必须从打开的服务器管理器菜单**管理工具**文件夹。

10. 关闭完不使用的工具后，单击 **“确定”** 。

### <a name="run-remote-server-administration-tools"></a>运行远程服务器管理工具

> [!NOTE]
> 安装远程服务器管理工具适用于 Windows 10 后**管理工具**文件夹显示在**启动**菜单。 可以从以下位置访问这些工具。
>
> -   **工具**在服务器管理器控制台中的菜单。
> -   “控制面板”\“系统和安全”\“管理工具”  。
> -   从“管理工具”  文件夹保存到桌面的快捷方式（若要创建此快捷方式，请右键单击“控制面板”\“系统和安全”\“管理工具”  链接，然后单击“创建快捷方式”  ）。

远程服务器管理工具适用于 Windows 10 的一部分安装的工具不能用于管理本地客户端计算机。 无论运行该工具，必须指定远程服务器或多个远程服务器，在其上运行该工具。 由于大多数工具集成使用服务器管理器，添加你想要在使用中的工具管理该服务器之前对服务器管理器服务器池管理的远程服务器**工具**菜单。 有关如何将服务器添加到服务器池并创建服务器的自定义组的详细信息，请参阅 [将服务器添加到服务器管理器](https://go.microsoft.com/fwlink/p/?LinkId=241353) 和 [创建和管理服务器组](https://go.microsoft.com/fwlink/?LinkId=247328)。

框中，从远程服务器管理工具，适用于 Windows 10，所有基于 GUI 的服务器管理工具，如 mmc 管理单元和对话框中的访问**工具**菜单中的服务器管理器控制台。 尽管运行远程服务器管理工具适用于 Windows 10 的计算机运行基于客户端的操作系统，安装这些工具后，服务器管理器，包括远程服务器管理工具适用于 Windows 10，将自动打开默认情况下在客户端计算机。 请注意，没有任何**本地服务器**在客户端计算机运行的服务器管理器控制台中的页。

##### <a name="to-start-server-manager-on-a-client-computer"></a>在客户端计算机上启动服务器管理器的步骤

1.  在“开始”  菜单中，单击“所有应用”  ，然后单击“管理工具”  。

2.  在“管理工具”  文件夹中，单击“服务器管理器”  。

尽管服务器管理器控制台中未列出它们**工具**菜单、 Windows PowerShell cmdlet 和命令提示符管理工具也安装角色和功能的远程服务器管理工具的一部分。 例如，如果你使用提升的用户权限 （以管理员身份运行） 打开 Windows PowerShell 会话并运行该 cmdlet `Get-Command -Module RDManagement`，结果包含一系列现已可用于运行后在本地计算机上的远程桌面服务 cmdlet安装远程服务器管理工具，只要 cmdlet 针对的运行全部或部分远程桌面服务角色的远程服务器。

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>使用提升的用户权限启动 Windows PowerShell（以管理员身份运行）

1.  在“开始”  菜单中，依次单击“所有应用”  、“Windows 系统”  和“Windows PowerShell”  。

2.  若要从桌面以管理员身份运行 Windows PowerShell，右键单击**Windows PowerShell**快捷方式，并单击**以管理员身份运行**。

> [!NOTE]
> 此外可以通过右键单击服务器管理器中的角色或组页面中的托管的服务器，然后单击为目标的特定服务器的 Windows PowerShell 会话**Windows PowerShell**。


## <a name="known-issues"></a>已知问题

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**问题**:RSAT FOD 安装将失败，错误代码为 0x800f0954

> **影响**:在 Windows 10 1809 RSAT FODs （2018 年 10 月更新） 在 WSUS/SCCM 环境
> 
> **解析**:若要在其中接收通过 WSUS 或 SCCM 更新已加入域的 PC 上安装 FODs，您需要更改组策略设置以启用下载 FODs 直接从 Windows 更新或本地共享。 有关详细信息和如何更改该设置的说明，请参阅[如何使功能上的需求和语言包可使用 WSUS/SCCM 时](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**问题**:通过设置应用的 RSAT FOD 安装不会显示状态/进度

> **影响**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:若要查看安装进度，请单击**回**按钮以上查看状态**管理可选功能**页。

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**问题**:通过设置应用的 RSAT FOD 卸载可能会失败

> **影响**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:在某些情况下，卸载故障是由于需要手动卸载依赖关系。 具体而言，如果需要 RSAT 工具的 RSAT 工具 B，然后选择要卸载 RSAT 工具的将失败如果仍安装 RSAT 工具 B。 首先，在这种情况下，卸载 RSAT 工具 B，然后卸载 RSAT 工具 a。请参阅 RSAT FODs 包括依赖项的列表。

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**问题**:RSAT FOD 卸载操作似乎成功，但仍然安装该工具

> **影响**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:重新启动 PC 将在完成删除该工具。

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**问题**:缺少 Windows 10 升级后的 RSAT

> **影响**:任何 RSAT。MSU 包 （在之前的安装 RSAT FODs) 不会自动重新安装
> 
> **解析**:不能跨 OS 升级由于 RSAT 保持 RSAT 安装。MSU 以 Windows 更新包的形式传递。 请升级 Windows 10 后，安装 RSAT。 请注意，此限制是为什么我们已经移到 FODs 随着 Windows 10 1809年启动的原因之一。 安装的 RSAT FODs 将持续到将来的 Windows 10 版本升级。

## <a name="see-also"></a>请参阅
>- [适用于 Windows 10 的远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [适用于 Windows Vista，Windows 7，Windows 8、 Windows Server 2008、 Windows Server 2008 R2、 Windows Server 2012 和 Windows Server 2012 R2 的远程服务器管理工具 (RSAT)](https://go.microsoft.com/fwlink/p/?LinkID=221055)
