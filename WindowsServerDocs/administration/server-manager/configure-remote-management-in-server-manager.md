---
title: 在服务器管理器中配置远程管理
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e1058a5679f73fcd2ceb8586da687158762d10f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383208"
---
# <a name="configure-remote-management-in-server-manager"></a>在服务器管理器中配置远程管理

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 Windows Server 中，你可以使用服务器管理器在远程服务器上执行管理任务。 默认情况下，在运行 Windows Server 2016 的服务器上启用远程管理。 若要使用服务器管理器远程管理服务器，请将服务器添加到服务器管理器服务器池。

你可以使用服务器管理器来管理运行较早版本 Windows Server 的远程服务器，但需要进行以下更新才能完全管理这些较旧的操作系统。

若要管理运行早于 Windows Server 2016 的 Windows Server 版本的服务器，请安装以下软件和更新，以使用 Windows Server 2016 中的服务器管理器来管理较旧版本的 Windows Server。

|操作系统|所需软件|可管理性|
|----------|-----------|---------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />@no__t 0[Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下载包更新 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 上的 Windows Management Instrumentation （WMI）提供程序。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的服务器的可管理性状态为 "**无法访问**"。<br />-在运行 Windows Server 2012 R2 或 Windows Server 2012 的服务器上不再需要与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)关联的性能更新。||
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />@no__t 0[Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下载包更新 Windows Management Instrumentation （WMI）提供程序（在 Windows Server 2008 R2 上）。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2008 R2 的服务器的可管理性状态为 "**无法访问**"。<br />-与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)相关的性能更新允许服务器管理器从 Windows Server 2008 R2 收集性能数据。||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />@no__t 0[Windows Management framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) ： Windows management framework 3.0 下载包更新 windows Server 2008 上的 WINDOWS MANAGEMENT INSTRUMENTATION （WMI）提供程序。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2008 的服务器的可管理性状态为 "**无法访问"-验证早期版本是否运行 Windows Management Framework 3.0**。<br />-与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)相关的性能更新允许服务器管理器从 Windows Server 2008 收集性能数据。||

有关如何添加工作组中要管理的服务器或从运行服务器管理器的工作组计算机管理远程服务器的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。

## <a name="enabling-or-disabling-remote-management"></a>启用或禁用远程管理
在 Windows Server 2016 中，远程管理在默认情况下处于启用状态。 必须先在目标计算机上启用服务器管理器远程管理（如果已禁用），然后才能使用服务器管理器远程连接到运行 Windows Server 2016 的计算机。 本部分中的过程将介绍如何禁用远程管理，以及如何重新启用远程管理（如果已禁用）。 在服务器管理器控制台中，本地服务器的远程管理状态会显示在 "**本地服务器**" 页的 "**属性**" 区域中。

即使已启用远程管理，内置管理员帐户之外的本地管理员帐户也可能不具有远程管理服务器的权限。 必须将远程用户帐户控制（UAC） **LocalAccountTokenFilterPolicy**注册表设置配置为允许除内置管理员帐户之外的管理员组的本地帐户远程管理服务器。

在 Windows Server 2016 中，服务器管理器依赖于 Windows 远程管理（WinRM）和分布式组件对象模型（DCOM）进行远程通信。 "**配置远程管理**" 对话框所控制的设置只会影响使用 WinRM 进行远程通信的服务器管理器和 Windows PowerShell 部分。 它们不会影响使用 DCOM 进行远程通信的服务器管理器部分。 例如，服务器管理器使用 WinRM 与运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的远程服务器进行通信，但使用 DCOM 与运行 Windows Server 2008 和 Windows Server 2008 R2 的服务器进行通信，但尚未应用[Windows Management framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)或[windows management framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019)更新。 Microsoft 管理控制台（mmc）和其他旧管理工具使用 DCOM。 有关如何更改这些设置的详细信息，请参阅本主题中的[对通过 DCOM 的 mmc 或其他工具远程管理进行配置](#to-configure-mmc-or-other-tool-remote-management-over-dcom)。

> [!NOTE]
> 本部分中的过程只能在运行 Windows Server 的计算机上完成。 无法在运行 Windows 10 的计算机上启用或禁用远程管理，因为不能使用服务器管理器管理客户端操作系统。

-   若要启用 WinRM 远程管理，可选择使用下列任一过程。

    -   [使用 Windows 界面启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [使用 Windows PowerShell 启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [使用命令行启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [在早期版本的 Windows Server 上启用服务器管理器和 Windows PowerShell 远程管理](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   若要禁用 WinRM 并服务器管理器远程管理，请选择以下过程之一。

    -   [使用组策略禁用远程管理](#to-disable-remote-management-by-using-group-policy)

    -   [在无人参与安装期间使用应答文件禁用远程管理](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   若要配置 DCOM 远程管理，请参阅[配置 DCOM 远程管理的步骤](#to-configure-mmc-or-other-tool-remote-management-over-dcom)。

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>使用 Windows 界面启用服务器管理器远程管理

1.  > [!NOTE]
    > "**配置远程管理**" 对话框所控制的设置不会影响使用 DCOM 进行远程通信的服务器管理器部分。

    在要进行远程管理的计算机上，打开服务器管理器，如果该计算机尚未打开。 在 Windows 任务栏上，单击“服务器管理器”。 在 "**开始**" 屏幕上，单击 "**服务器管理器**" 磁贴。

2.  在 "**本地服务器**" 页的 "**属性**" 区域中，单击 "**远程管理**" 属性的超链接值。

3.  执行下列操作之一，然后单击“确认”。

    -   若要阻止使用服务器管理器（或 Windows PowerShell，如果已安装）远程管理此计算机，请清除 "**从其他计算机启用此服务器的远程管理**" 复选框。

    -   若要通过使用服务器管理器或 Windows PowerShell 来远程管理此计算机，请选择 "**从其他计算机启用此服务器的远程管理**"。

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>使用 Windows PowerShell 启用服务器管理器远程管理的步骤

1.  在要进行远程管理的计算机上，执行以下操作之一，以使用提升的用户权限打开 Windows PowerShell 会话。

    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。

    -   在 Windows 的 "**开始**" 屏幕上，右键单击 " **windows PowerShell**"，然后单击应用栏上的 "以**管理员身份运行**"。

2.  键入以下项，然后按**enter**启用所有必需的防火墙规则例外。

    **Configure-smremoting.exe-enable**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>使用命令行启用服务器管理器远程管理

1.  在要对其进行远程管理的计算机上，使用提升的用户权限打开命令提示符会话。 为此，请在 "**开始**" 屏幕上键入**cmd**，右键单击 "**应用**" 结果中显示的 "**命令提示符**" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。

2.  运行下面的可执行文件。

    **%windir%\system32\Configure-SMremoting.exe**

3.  执行下列操作之一：

    -   若要禁用远程管理，键入**configure-smremoting.exe-disable**，然后按**enter**。

    -   若要启用远程管理，键入**configure-smremoting.exe-enable**，然后按**enter**。

    -   若要查看当前的远程管理设置，请键入**configure-smremoting.exe-get**，然后按 enter。

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>在较早版本 Windows Server 上启用服务器管理器和 Windows PowerShell 远程管理的步骤

-   执行下列操作之一：

    -   若要在运行 Windows Server 2012 的服务器上启用远程管理，请参阅本主题中[的使用 Windows 界面启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)。

    -   若要在运行 Windows Server 2008 R2 的服务器上启用远程管理，请参阅 Windows Server 2008 R2 帮助中的[使用服务器管理器进行远程管理](https://go.microsoft.com/fwlink/?LinkID=137378)。

    -   若要在运行 Windows Server 2008 的服务器上启用远程管理，请参阅[在 Windows PowerShell 中启用和使用远程命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>通过 DCOM 配置 mmc 或其他工具远程管理

1.  执行下列步骤之一，以打开“高级安全 Windows 防火墙”管理单元。

    -   在服务器管理器中 "**本地服务器**" 页的 "**属性**" 区域中，单击 " **Windows 防火墙**" 属性的超文本值，然后单击 "**高级设置**"。

    -   在 "**开始**" 屏幕上，键入 " **WF**"，然后在 "**应用**" 结果中显示该管理单元磁贴时单击该磁贴。

2.  在树窗格中，选择“入站规则”。

3.  验证是否启用了以下防火墙规则的例外，并且组策略设置未禁用。 如果未启用其中任一项，则继续执行下一步骤。

    -   COM+ 网络访问(DCOM-In)

    -   远程事件日志管理（NP-IN）

    -   远程事件日志管理（RPC）

    -   远程事件日志管理（rpc-epmap）

4.  右键单击未启用的规则，然后单击上下文菜单中的 “启用规则”。

5.  关闭“高级安全 Windows 防火墙”管理单元。

### <a name="to-disable-remote-management-by-using-group-policy"></a>使用组策略禁用远程管理的步骤

1.  执行下列操作之一，打开本地组策略编辑器。

    -   在运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的服务器上的 "**开始**" 屏幕上，键入 " **gpedit.msc**"，然后在显示 " **gpedit.msc** " 磁贴时单击该磁贴。

    -   在运行 Windows Server 2008 R2 或 Windows Server 2008 的服务器上，在 "**运行**" 对话框中键入 " **gpedit.msc**"，然后按**enter**。

2.  打开**计算机配置 \ 管理模板 \Windows 组件 \windows 远程管理（WinRM） \WinRM 服务**。

3.  在内容窗格中，双击“允许通过 WinRM 进行远程服务器管理”。

4.  在“允许通过 WinRM 进行远程服务器管理” 策略设置对话框中，选择“禁用” 来禁用远程管理。 单击“确定” 保存更改并关闭策略设置对话框。

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>在无人参与安装期间使用应答文件禁用远程管理的步骤

1.  使用 Windows 系统映像管理器（Windows SIM）为 Windows Server 2016 安装创建无人参与安装答案文件。 有关如何创建应答文件和使用 Windows SIM 的详细信息，请参阅[什么是 Windows 系统映像管理器？](https://technet.microsoft.com/library/cc766347.aspx)和 @no__t 1Step）：适用于 IT 专业人员的基本 Windows 部署 @ no__t-0。

2.  在答案文件中，找到 " **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**" 设置。

3.  若要在要将答案文件应用到的所有服务器上默认禁用服务器管理器远程管理，请将**enableserverremotemanagement**设置为**False**。

    > [!NOTE]
    > 此设置将禁止远程管理作为操作系统设置过程的一部分。 配置此设置不会阻止管理员在操作系统设置完成后启用服务器上服务器管理器远程管理。 管理员可以通过使用[windows 界面配置服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)中的步骤来再次启用服务器管理器远程管理，或使用[windows PowerShell 在此中启用服务器管理器远程](#to-enable-server-manager-remote-management-by-using-windows-powershell)管理标题.
    > 
    > 如果默认情况下禁用远程管理作为无人参与安装的一部分，并且安装完成后不在该服务器上启用远程管理，则无法使用服务器管理器完全管理此应答文件应用到的服务器。 在将服务器管理器控制台添加到服务器管理器服务器后，运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （并且默认禁用了远程管理）的服务器将在控制台中生成可管理性状态错误。池子.

## <a name="windows-remote-management-winrm-listener-settings"></a>Windows 远程管理（WinRM）侦听器设置
服务器管理器依赖于要管理的远程服务器上的默认 WinRM 侦听器设置。 如果远程服务器上的默认身份验证机制或 WinRM 侦听器端口号已从默认设置中更改，则服务器管理器无法与远程服务器通信。

以下列表显示使用服务器管理器管理的默认 WinRM 侦听器设置。

-   WinRM 服务正在运行。

-   创建了一个 WinRM 侦听器，以接受通过端口号 5985 的 HTTP 请求。

-   Windows 防火墙设置中启用了端口号 5985，以允许通过 WinRM 的请求。

-   同时启用了“Kerberos”和“协商式”身份验证类型。

默认端口号为 5985，以使 WinRM 与远程计算机进行通信。

有关如何配置 WinRM 侦听器设置的详细信息，请在命令提示符下键入**WinRM help config**，然后按 enter。

## <a name="see-also"></a>请参阅
[将服务器添加到服务器管理器](add-servers-to-server-manager.md)@no__t[Windows PowerShell： Windows Server 技术中心上的 about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx)
 有关[用户帐户控制的说明](https://support.microsoft.com/kb/951016)



