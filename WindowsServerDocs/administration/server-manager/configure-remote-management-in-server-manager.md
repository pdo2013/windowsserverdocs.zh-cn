---
title: 配置远程管理在服务器管理器
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 63d90d52b55357b5de823f2ca5e0a9fa2a3468e6
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222989"
---
# <a name="configure-remote-management-in-server-manager"></a>配置远程管理在服务器管理器

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 Windows Server 中，可以使用服务器管理器在远程服务器上执行管理任务。 在运行 Windows Server 2016 的服务器上默认启用远程管理。 若要使用服务器管理器远程管理服务器，您将服务器添加到服务器管理器服务器池。

可以使用服务器管理器来管理那些运行较旧版本的 Windows Server 的远程服务器，但必须完成下列更新才能进行完全管理这些较早的操作系统。

若要管理运行 Windows Server 版本早于 Windows Server 2016 的服务器，请安装以下软件和更新，以便较旧版本的 Windows Server 可管理 Windows Server 2016 中使用服务器管理器。

|操作系统|所需软件|可管理性|
|----------|-----------|---------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下载包更新 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的服务器的可管理性状态已**无法访问**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)不再需要运行 Windows Server 2012 R2 的服务器或 Windows Server 2012 上。||
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下载包更新 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2008 R2 的服务器具有的可管理性状态**无法访问**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允许服务器管理器从 Windows Server 2008 R2 中收集性能数据。||
| Windows Server 2008 |-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下载包更新 Windows Server 2008 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2008 的服务器具有的可管理性状态**不可访问-确定早期版本运行 Windows Management Framework 3.0**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允许服务器管理器从 Windows Server 2008 中收集性能数据。||

有关如何添加位于工作组中要管理或从服务器管理器运行的工作组计算机管理远程服务器的服务器的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。

## <a name="enabling-or-disabling-remote-management"></a>启用或禁用远程管理
在 Windows Server 2016 中，默认情况下启用远程管理。 可以连接到正在使用服务器管理器远程运行 Windows Server 2016 的计算机之前，则如果已禁用，在目标计算机上必须启用服务器管理器远程管理。 本部分中的过程将介绍如何禁用远程管理，以及如何重新启用远程管理（如果已禁用）。 在服务器管理器控制台中，本地服务器的远程管理状态显示在**属性**区域中的**本地服务器**页。

即使已启用远程管理，内置管理员帐户之外的本地管理员帐户也可能不具有远程管理服务器的权限。 远程用户帐户控制 (UAC) **LocalAccountTokenFilterPolicy**注册表设置必须配置为允许内置管理员帐户之外 Administrators 组，以便进行远程管理的本地帐户服务器。

在 Windows Server 2016 中，服务器管理器依赖 Windows 远程管理 (WinRM) 和分布式组件对象模型 (DCOM) 进行远程通信。 通过控制的设置**配置远程管理**对话框只会影响使用 WinRM 进行远程通信的服务器管理器和 Windows PowerShell 的部分。 它们不会影响部分使用 DCOM 进行远程通信的服务器管理器。 例如，服务器管理器使用 WinRM 与运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的远程服务器进行通信，而使用 DCOM 与运行 Windows Server 2008 的服务器和 Windows Server 2008 R2，通信但没有[Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)或[Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019)应用更新。 Microsoft 管理控制台 (mmc) 和其他旧管理工具使用 DCOM。 有关如何更改这些设置的详细信息，请参阅[来配置通过 DCOM 的 mmc 或其他工具远程管理](#to-configure-mmc-or-other-tool-remote-management-over-dcom)本主题中。

> [!NOTE]
> 本部分中的过程只能在运行 Windows Server 的计算机上完成。 无法启用或禁用远程管理运行 Windows 10 通过使用这些过程中，因为客户端操作系统无法通过使用服务器管理器管理的计算机上。

-   若要启用 WinRM 远程管理，可选择使用下列任一过程。

    -   [若要使用 Windows 界面启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [若要使用 Windows PowerShell 启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [若要使用命令行启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [若要启用服务器管理器和 Windows PowerShell 远程管理在早期版本上的 Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   若要禁用 WinRM 和服务器管理器远程管理，请选择以下过程之一。

    -   [若要通过使用组策略禁用远程管理](#to-disable-remote-management-by-using-group-policy)

    -   [若要禁用远程管理的无人参与安装期间使用应答文件](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   若要配置 DCOM 远程管理，请参阅[配置 DCOM 远程管理的步骤](#to-configure-mmc-or-other-tool-remote-management-over-dcom)。

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>使用 Windows 界面启用服务器管理器远程管理

1.  > [!NOTE]
    > 通过控制的设置**配置远程管理**对话框不会影响部分使用 DCOM 进行远程通信的服务器管理器。

    在你想要远程管理的计算机，打开服务器管理器中，如果它不是已打开。 在 Windows 任务栏上，单击  “服务器管理器”。 上**启动**屏幕上，单击**服务器管理器**磁贴。

2.  在中**属性**区域中的**本地服务器**页上，单击超链接值**远程管理**属性。

3.  执行下列操作之一，然后单击“确认”  。

    -   若要阻止此计算机远程管理使用服务器管理器 （或如果已安装的 Windows PowerShell），请清除**启用远程管理此服务器从其他计算机**复选框。

    -   若要允许通过服务器管理器或 Windows PowerShell 远程管理此计算机，请选择**启用远程管理此服务器从其他计算机**。

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>使用 Windows PowerShell 启用服务器管理器远程管理的步骤

1.  在你想要远程管理的计算机，执行下列操作之一，使用提升的用户权限打开 Windows PowerShell 会话。

    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell  ，然后单击“以管理员身份运行”  。

    -   在 Windows 上**启动**屏幕上，右键单击**Windows PowerShell**，然后在应用栏上单击**以管理员身份运行**。

2.  键入以下内容，，然后按**Enter**启用所有必需的防火墙规则例外。

    **Configure-SMremoting.exe -enable**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>使用命令行启用服务器管理器远程管理

1.  在要对其进行远程管理的计算机上，使用提升的用户权限打开命令提示符会话。 为此，请在**启动**屏幕上，键入**cmd**，右键单击**命令提示符下**磁贴中显示时**应用**结果，并然后在应用栏上，单击**以管理员身份运行**。

2.  运行下面的可执行文件。

    **%windir%\system32\Configure-SMremoting.exe**

3.  执行下列操作之一：

    -   若要禁用远程管理，键入**SMremoting.exe-禁用**，然后按**Enter**。

    -   若要启用远程管理，键入**SMremoting.exe-启用**，然后按**Enter**。

    -   若要查看当前的远程管理设置，请键入**SMremoting.exe-获取**，然后按 ENTER。

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>在较早版本 Windows Server 上启用服务器管理器和 Windows PowerShell 远程管理的步骤

-   执行下列操作之一：

    -   若要启用远程管理运行 Windows Server 2012 的服务器上，请参阅[若要使用 Windows 界面启用服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)本主题中。

    -   若要启用远程管理运行 Windows Server 2008 R2 的服务器上，请参阅[远程管理使用服务器管理器](https://go.microsoft.com/fwlink/?LinkID=137378)Windows Server 2008 R2 帮助中。

    -   若要启用远程管理运行 Windows Server 2008 的服务器上，请参阅[启用和使用 Windows PowerShell 中远程命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>若要配置通过 DCOM 的 mmc 或其他工具远程管理

1.  执行下列步骤之一，以打开“高级安全 Windows 防火墙”管理单元。

    -   在中**属性**区域中的**本地服务器**页上的服务器管理器中，单击超链接值**Windows 防火墙**属性，，然后单击**高级设置**。

    -   上**启动**屏幕上，键入**WF.msc**，然后单击管理单元磁贴中显示时**应用**结果。

2.  在树窗格中，选择“入站规则”  。

3.  验证以下防火墙规则的例外情况已启用，并且不由组策略设置已禁用。 如果未启用其中任一项，则继续执行下一步骤。

    -   COM+ 网络访问(DCOM-In)

    -   远程事件日志管理 (Np-in)

    -   远程事件日志管理 (RPC)

    -   远程事件日志管理 (RPC-EPMAP)

4.  右键单击未启用的规则，然后单击上下文菜单中的  “启用规则”。

5.  关闭“高级安全 Windows 防火墙”管理单元。

### <a name="to-disable-remote-management-by-using-group-policy"></a>使用组策略禁用远程管理的步骤

1.  执行下列操作之一以打开本地组策略编辑器。

    -   在运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的服务器上**启动**屏幕上，键入**gpedit.msc**，然后单击**gpedit**磁贴显示时。

    -   在运行 Windows Server 2008 R2 或 Windows Server 2008 的服务器上**运行**对话框中，键入**gpedit.msc**，然后按**Enter**。

2.  打开**计算机配置 \ 管理模板 \windows 组件 \windows 远程管理 (WinRM) \WinRM 服务**。

3.  在内容窗格中，双击“允许通过 WinRM 进行远程服务器管理”  。

4.  在“允许通过 WinRM 进行远程服务器管理”  策略设置对话框中，选择“禁用”  来禁用远程管理。 单击“确定”  保存更改并关闭策略设置对话框。

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>在无人参与安装期间使用应答文件禁用远程管理的步骤

1.  使用 Windows 系统映像管理器 (Windows SIM) 创建用于 Windows Server 2016 安装的无人参与的安装答案文件。 有关如何创建应答文件和使用 Windows SIM 的详细信息，请参阅[什么是 Windows 系统映像管理器？](https://technet.microsoft.com/library/cc766347.aspx)和[分步：面向 IT 专业人员基本 Windows 部署](https://technet.microsoft.com/library/dd349348.aspx)。

2.  在答案文件中，找到设置**Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**。

3.  若要禁用默认情况下，你想要应用应答文件的所有服务器上的服务器管理器远程管理，请设置**Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement**到**False**.

    > [!NOTE]
    > 此设置将禁止远程管理作为操作系统设置过程的一部分。 配置此设置不会阻止管理员在操作系统安装完成后启用服务器管理器远程管理服务器上。 管理员可以启用服务器管理器中的步骤再次通过使用远程管理[若要使用 Windows 界面配置服务器管理器远程管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)或[使用启用服务器管理器远程管理Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)本主题中。
    > 
    > 如果禁止远程管理默认作为无人参与安装的一部分，并完成安装后重新启用服务器上的远程管理，此答案文件应用到的服务器不能完全管理通过使用服务器管理器。 服务器的运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （和其远程管理默认情况下禁用） 在它们添加到服务器管理器服务器后在服务器管理器控制台中生成可管理性状态错误池。

## <a name="windows-remote-management-winrm-listener-settings"></a>Windows 远程管理 (WinRM) 侦听器设置
服务器管理器依赖于你想要管理的远程服务器上的默认 WinRM 侦听器设置。 如果已从默认设置的默认身份验证机制或 WinRM 侦听器端口号的远程服务器上，服务器管理器无法与远程服务器通信。

以下列表显示默认的 WinRM 侦听器设置为使用服务器管理器管理的。

-   WinRM 服务正在运行。

-   创建了一个 WinRM 侦听器，以接受通过端口号 5985 的 HTTP 请求。

-   Windows 防火墙设置中启用了端口号 5985，以允许通过 WinRM 的请求。

-   同时启用了“Kerberos”  和“协商式”  身份验证类型。

默认端口号为 5985，以使 WinRM 与远程计算机进行通信。

有关详细信息，有关如何配置 WinRM 侦听器设置，在命令提示符下键入**winrm 帮助配置**，然后按 ENTER。

## <a name="see-also"></a>请参阅
[将服务器添加到服务器管理器](add-servers-to-server-manager.md)
[Windows PowerShell: about_remote_Troubleshooting Windows Server TechCenter 上的](https://technet.microsoft.com/library/dd347642.aspx)
[用户帐户控制的说明](https://support.microsoft.com/kb/951016)



