---
title: 远程服务器管理工具
description: 远程服务器管理工具的顶级主题
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 121914f721cda7cbf0a117527b69568032d5541b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387089"
---
# <a name="remote-server-administration-tools"></a>远程服务器管理工具

>适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题支持适用于 Windows 10 的远程服务器管理工具。

> [!IMPORTANT]
> 从 Windows 10 年10月2018更新开始，在 Windows 10 中，RSAT 作为一组**按需功能**提供。 有关安装说明，请参阅以下**哪种 RSAT 版本**。

RSAT 允许 IT 管理员管理 Windows 10 PC 中的 Windows Server 角色和功能。

远程服务器管理工具包括服务器管理器、Microsoft 管理控制台（mmc）管理单元、控制台、Windows PowerShell cmdlet 和提供程序，以及一些用于管理在 Windows Server 上运行的角色和功能的命令行工具。

远程服务器管理工具包括可用于管理在远程服务器上运行的角色和功能的 Windows PowerShell cmdlet 模块。 虽然 windows PowerShell 远程管理在 Windows Server 2016 上默认启用，但在 Windows 10 上默认情况下未启用。 若要针对远程服务器运行作为远程服务器管理工具一部分的 cmdlet，请在安装后，在 Windows 客户端计算机上以提升的用户权限（即以管理员身份运行）打开的 Windows PowerShell 会话中运行 `Enable-PSremoting`远程服务器管理工具。

## <a name="BKMK_Thresh"></a>适用于 Windows 10 的远程服务器管理工具
使用适用于 Windows 10 的远程服务器管理工具来管理运行 Windows Server 2016、Windows Server 2012 R2 的计算机上的特定技术，以及有限情况、Windows Server 2012 或 Windows Server 2008 R2。

适用于 Windows 10 的远程服务器管理工具包括对运行服务器核心安装选项的计算机的远程管理，或 Windows Server 2016、Windows Server 2012 R2 的最小服务器界面配置和有限事例，Windows Server 2012 的服务器核心安装选项。 但是，不能在任何版本的 Windows Server 操作系统上安装适用于 Windows 10 的远程服务器管理工具。

### <a name="tools-available-in-this-release"></a>此版本中的可用工具
有关适用于 Windows 10 的远程服务器管理工具工具列表，请参阅[windows 操作系统远程服务器管理工具（RSAT）](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)中的表。

### <a name="system-requirements"></a>系统要求
适用于 Windows 10 的远程服务器管理工具只能安装在运行 Windows 10 的计算机上。 远程服务器管理工具不能安装在运行 Windows RT 8.1 或其他芯片设备的计算机上。

适用于 Windows 10 的远程服务器管理工具在基于 x86 和 x64 的 Windows 10 版本上运行。

> [!IMPORTANT]
> 适用于 Windows 10 的远程服务器管理工具不应安装在运行 Windows 8.1、Windows 8、windows Server 2008 R2、Windows Server 2008、Windows Server 2003 或 Windows 2000 服务器的管理工具包的计算机上。 在安装远程服务器管理之前，从计算机中删除所有旧版本的管理工具包或远程服务器管理工具，包括早期的预发布版本和不同语言或区域设置的工具版本适用于 Windows 10 的工具。

若要使用此版本的服务器管理器访问和管理运行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的远程服务器，你必须安装多个更新，以使旧版 Windows Server 操作系统可通过使用 Se 进行管理服务器管理器。 有关如何使用 Windows 10 的远程服务器管理工具中的服务器管理器为管理准备 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 的详细信息，请参阅使用[服务器管理多台远程服务器管理器](https://technet.microsoft.com/library/hh831456.aspx)。

必须在远程服务器上启用 windows PowerShell 和服务器管理器远程管理，才能使用 Windows 10 远程服务器管理工具中的工具管理它们。 默认情况下，在运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的服务器上启用远程管理。 有关如何在已禁用远程管理的情况下启用它的详细信息，请参阅 [使用服务器管理器管理多台远程服务器](https://go.microsoft.com/fwlink/p/?LinkId=241358)。

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>安装、卸载和关闭/启用 RSAT 工具

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>使用点播功能（FoD）在 Windows 10 十月2018更新或更高版本上安装特定的 RSAT 工具

从 Windows 10 年10月2018版更新开始，RSAT 作为一组**功能**直接包含在 windows 10 中。 现在，你可以转到管理 "**设置**" 中的**可选功能**，然后单击 "**添加功能**" 以查看可用的 rsat 工具列表，而不是下载 RSAT 包。 选择并安装所需的特定 RSAT 工具。 若要查看安装进度，请单击 "**后退**" 按钮以查看 "**管理可选功能**" 页上的 "状态"。

请参阅[**按需功能**提供的 RSAT 工具列表](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了通过图形**设置**应用程序安装以外，还可以通过命令行或自动化使用[**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)安装特定的 RSAT 工具。

按需功能的一个好处是，已安装的功能在 Windows 10 版本升级期间仍然有效！

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>在 Windows 10 十月2018更新或更高版本（安装后安装了 FoD）上卸载特定的 RSAT 工具

在 Windows 10 上，打开 "**设置**" 应用，"**管理可选功能**"，选择并卸载要删除的特定 RSAT 工具。 请注意，在某些情况下，你将需要手动卸载依赖项。 具体来说，如果 RSAT 工具 B 需要 RSAT 工具 A，则如果仍安装了 RSAT 工具 B，则选择卸载 RSAT 工具 A 会失败。 在这种情况下，先卸载 RSAT 工具 B，然后卸载 RSAT 工具 A。另请注意，在某些情况下，即使仍安装了该工具，卸载 RSAT 工具也可能会成功。 在这种情况下，重新启动计算机将会完成此工具的删除。

请参阅[RSAT 工具列表，包括依赖关系](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了通过图形设置应用程序进行卸载以外，还可以使用[**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)通过命令行或自动化来卸载特定的 RSAT 工具。

### <a name="when-to-use-which-rsat-version"></a>何时使用 RSAT 版本

如果你的 Windows 10 版本早于2018年10月更新（1809），则将不能按**需使用功能**。 你将需要下载并安装 RSAT 包。

- 如上所述**直接从 Windows 10 安装 RSAT FODs，如下所述**：在 Windows 10 十月2018更新（1809）或更高版本上安装时，用于管理 Windows Server 2019 或早期版本。

- **下载并安装 WS_1803 RSAT 包，如下所述**：在 Windows 10 2018 年4月更新（1803）或更早版本上安装时，用于管理 Windows Server、版本1803或 Windows Server 版本1709。

- **下载并安装 WS2016 RSAT 包，如下所述**：在 Windows 10 2018 年4月更新（1803）或更早版本上安装时，用于管理 Windows Server 2016 或早期版本。

#### <a name="BKMK_installthresh"></a>下载用于安装 Windows 10 远程服务器管理工具的 RSAT 包

1.  从[Microsoft 下载中心](https://go.microsoft.com/fwlink/?LinkID=404281)下载适用于 Windows 10 包的远程服务器管理工具。 你可以从下载中心网站运行安装程序，或者将下载包保存到本地计算机或共享。

    > [!IMPORTANT]
    > 你只能在运行 Windows 10 的计算机上安装适用于 Windows 10 的远程服务器管理工具。 远程服务器管理工具不能安装在运行 Windows RT 8.1 或其他芯片设备的计算机上。

2.  如果你将下载包保存到本地计算机或共享，则根据要安装工具的计算机的体系结构，双击安装程序 **WindowsTH-KB2693643-x64.msu** 或 **WindowsTH-KB2693643-x86.msu**。

3.  当 **“Windows 更新独立安装程序”** 对话框提示你安装更新时，单击 **“是”** 。

4.  阅读并接受许可条款。 单击 **“我接受”** 。

5.  安装需要几分钟才能完成。

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>卸载适用于 Windows 10 的远程服务器管理工具（在 RSAT 包安装后）

1. 在桌面上，依次单击“开始”、“所有应用”、“Windows 系统”和“控制面板”。

2. 在“程序”下，单击“卸载程序”。

3. 单击 **“查看已安装的更新”** 。

4. 右键单击“Microsoft Windows 的更新(KB2693643)”，然后单击“卸载”。

5. 当系统询问你是否确定要卸载更新时，单击 **“是”** 。
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>关闭特定工具（在 RSAT 包安装后）

6. 在桌面上，依次单击“开始”、“所有应用”、“Windows 系统”和“控制面板”。

7. 单击 **“程序”** ，然后在 **“程序和功能”** 中单击 **“打开或关闭 Windows 功能”** 。

8. 在 **“Windows 功能”** 对话框中，展开 **“远程服务器管理工具”** ，然后展开 **“角色管理工具”** 或 **“功能管理工具”** 。

9. 清除要关闭的工具对应的复选框。

   > [!NOTE]
   > 如果关闭服务器管理器，则必须重新启动计算机，并且必须从 "**管理工具**" 文件夹中打开从服务器管理器的 "**工具**" 菜单中访问的工具。

10. 关闭完不使用的工具后，单击 **“确定”** 。

### <a name="run-remote-server-administration-tools"></a>运行远程服务器管理工具

> [!NOTE]
> 安装适用于 Windows 10 的远程服务器管理工具后，"**管理工具**" 文件夹将显示在 "**开始**" 菜单上。 可以从以下位置访问这些工具。
>
> -   服务器管理器控制台中的 "**工具**" 菜单。
> -   “控制面板”\“系统和安全”\“管理工具”。
> -   从“管理工具” 文件夹保存到桌面的快捷方式（若要创建此快捷方式，请右键单击“控制面板”\“系统和安全”\“管理工具” 链接，然后单击“创建快捷方式”）。

作为 Windows 10 远程服务器管理工具的一部分安装的工具不能用于管理本地客户端计算机。 无论运行哪种工具，都必须指定要在其上运行该工具的一台或多台远程服务器。 因为大多数工具都与服务器管理器集成，所以你需要在使用 "**工具**" 菜单中的工具管理服务器之前，将你想要管理的远程服务器添加到服务器管理器服务器池。 有关如何将服务器添加到服务器池并创建服务器的自定义组的详细信息，请参阅 [将服务器添加到服务器管理器](https://go.microsoft.com/fwlink/p/?LinkId=241353) 和 [创建和管理服务器组](https://go.microsoft.com/fwlink/?LinkId=247328)。

在 Windows 10 远程服务器管理工具中，所有基于 GUI 的服务器管理工具（如 mmc 管理单元和对话框）都可以从服务器管理器控制台的 "**工具**" 菜单中访问。 尽管运行 Windows 10 远程服务器管理工具的计算机运行基于客户端的操作系统，但安装工具后，服务器管理器（随 Windows 10 远程服务器管理工具提供）将在默认情况下自动打开在客户端计算机上。 请注意，在客户端计算机上运行的服务器管理器控制台中没有 "**本地服务器**" 页面。

##### <a name="to-start-server-manager-on-a-client-computer"></a>在客户端计算机上启动服务器管理器的步骤

1.  在“开始” 菜单中，单击“所有应用”，然后单击“管理工具”。

2.  在“管理工具” 文件夹中，单击“服务器管理器”。

尽管它们未在服务器管理器控制台 "**工具**" 菜单中列出，但 Windows PowerShell Cmdlet 和命令提示符管理工具也作为远程服务器管理工具的一部分为角色和功能安装。 例如，如果你使用提升的用户权限打开 Windows PowerShell 会话（以管理员身份运行），并 `Get-Command -Module RDManagement` 运行 cmdlet，则结果将包含一个远程桌面服务 cmdlet 列表，在安装之后，这些 cmdlet 现在可在本地计算机上运行远程服务器管理工具，只要 cmdlet 针对的是运行全部或部分远程桌面服务角色的远程服务器。

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>使用提升的用户权限启动 Windows PowerShell（以管理员身份运行）

1.  在“开始”菜单中，依次单击“所有应用”、“Windows 系统”和“Windows PowerShell”。

2.  若要从桌面以管理员身份运行 Windows PowerShell，请右键单击**Windows powershell**快捷方式，然后单击 "以**管理员身份运行**"。

> [!NOTE]
> 你还可以通过右键单击服务器管理器中角色或组页面中的托管服务器，然后单击 " **Windows powershell**"，来启动针对特定服务器的 Windows powershell 会话。


## <a name="known-issues"></a>已知问题

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**问题**：RSAT FOD 安装失败，错误代码为0x800f0954

> **影响**：WSUS/SCCM 环境中的 RSAT FODs on Windows 10 1809 （10月2018更新）
> 
> **解决方法**:若要在已加入域的电脑上安装 FODs （通过 WSUS 或 SCCM 接收更新），需要更改组策略设置，以便直接从 Windows 更新或本地共享下载 FODs。 有关如何更改该设置的更多详细信息和说明，请参阅[使用 WSUS/SCCM 时如何根据需要提供功能和语言包](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**问题**：通过 "设置" 应用的 RSAT FOD 安装不显示状态/进度

> **影响**：Windows 10 1809 上的 RSAT FODs （2018年10月更新）
> 
> **解决方法**:若要查看安装进度，请单击 "**后退**" 按钮以查看 "**管理可选功能**" 页上的 "状态"。

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**问题**：通过设置应用进行的 RSAT FOD 卸载可能会失败

> **影响**：Windows 10 1809 上的 RSAT FODs （2018年10月更新）
> 
> **解决方法**:在某些情况下，卸载失败的原因是需要手动卸载依赖项。 具体来说，如果 RSAT 工具 B 需要 RSAT 工具 A，则如果仍安装了 RSAT 工具 B，则选择卸载 RSAT 工具 A 会失败。 在这种情况下，先卸载 RSAT 工具 B，然后卸载 RSAT 工具 A。请参阅包含依赖项的 RSAT FODs 列表。

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**问题**：RSAT FOD 卸载似乎已成功，但仍在安装该工具

> **影响**：Windows 10 1809 上的 RSAT FODs （2018年10月更新）
> 
> **解决方法**:重新启动计算机将完成此工具的删除。

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**问题**：Windows 10 升级后，RSAT 丢失

> **影响**：任何 RSAT。未自动重新安装 MSU 包安装（在 RSAT FODs 之前）
> 
> **解决方法**:由于 RSAT，无法在操作系统升级过程中保留 RSAT 安装。将 MSU 作为 Windows 更新包交付。 请在升级 Windows 10 后再次安装 RSAT。 请注意，此限制是我们从 Windows 10 1809 开始迁移到 FODs 的原因之一。 在未来的 Windows 10 版本升级中，安装的 RSAT FODs 将保持不变。

## <a name="see-also"></a>请参阅
>- [适用于 Windows 10 的远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [适用于 Windows Vista、Windows 7、Windows 8、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 和 Windows Server 2012 R2 的远程服务器管理工具（RSAT）](https://go.microsoft.com/fwlink/p/?LinkID=221055)
