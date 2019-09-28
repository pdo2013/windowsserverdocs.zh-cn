---
title: 通过 Sconfig.cmd 配置 Windows Server 的服务器核心安装
description: 介绍如何使用 Sconfig.cmd
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 8f07c3546fd43dd3ce9cfa32094bc691fa0bcb2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360247"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>通过 Sconfig.cmd 配置 Windows Server 2016 或 Windows Server 版本 1709 的服务器核心安装

> 适用于：Windows Server（半年频道）和 Windows Server 2016

在 Windows Server 2016 和 Windows Server 版本 1709 中，可以使用服务器配置工具 (Sconfig.cmd) 配置和管理服务器核心安装的几个常见方面。 必须是管理员组的成员才能使用此工具。

可以在服务器核心和带桌面体验的服务器（仅限于 Windows Server 2016）安装中使用 Sconfig.cmd。

## <a name="start-the-server-configuration-tool"></a>启动服务器配置工具

1. 更改为系统驱动器。

2. 键入 `Sconfig.cmd`，然后按 Enter。 服务器配置工具界面打开：

    ![Sconfig.cmd 用户界面屏幕截图](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>域/工作组设置

当前域/工作组设置在默认服务器配置工具屏幕中显示。 可以通过从主菜单访问“域/工作组”设置页，并按照说明执行操作（提供所需的信息），加入某个域或工作组  。

如果域用户尚未添加到本地管理员组，则不能使用域用户进行系统更改，例如更改计算机名称。 若要将域用户添加到本地管理员组，允许计算机重启。 然后，以本地管理员身份登录计算机，并按照本文后面的[本地管理员设置](#local-administrator-settings)部分中的步骤进行操作。

> [!NOTE]
> 需要重启服务器，以便将更改应用到域或工作组成员。 不过，你还可以进行其他更改，并在所有更改完成之后再重启服务器，以免需要多次重启服务器。 默认情况下，重启 Hyper-V 服务器之前，正在运行的虚拟机会自动保存。

## <a name="computer-name-settings"></a>计算机名称设置

当前计算机名称在默认服务器配置工具屏幕中显示。 可以通过从主菜单访问“计算机名称”设置页并按照说明进行操作，来更改计算机名称  。

> [!NOTE]
> 需要重启服务器，以便将更改应用到域或工作组成员。 不过，你还可以进行其他更改，并在所有更改完成之后再重启服务器，以免需要多次重启服务器。 默认情况下，重启 Hyper-V 服务器之前，正在运行的虚拟机会自动保存。

## <a name="local-administrator-settings"></a>“本地管理员设置”

若要将其他用户添加到本地管理员组，请使用主菜单上的 **“添加本地管理员”** 选项。 在加入域的计算机上，以下面的格式输入用户：域\用户名。 在非加入域的计算机（工作组计算机）上，仅输入用户名。 更改会立即生效。

## <a name="network-settings"></a>网络设置

你可以将 IP 地址配置为由 DHCP 服务器自动分配，或者你可以手动分配静态 IP 地址。 此选项还允许你为服务器配置 DNS 服务器设置。

> [!NOTE]
> 这些选项以及更多选项现可使用网络 Windows PowerShell cmdlet。 有关详细信息，请参阅 Windows Server 库中的 [网络适配器 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) 。

## <a name="windows-update-settings"></a>Windows 更新设置

当前 Windows 更新设置在默认服务器配置工具屏幕中显示。 你可以在主菜单的 **“Windows 更新设置”** 配置选项中将服务器配置为使用自动或手动更新。

选中 **“自动更新”** 时，系统会在每天上午 3 点整查看和安装更新。 设置会立即生效。 选中 **“手动更新”** 时，系统不会自动查看更新。

你可以随时从主菜单的 **“下载和安装更新”** 选项下载和安装适用的更新。

“仅下载”选项将扫描更新、下载任何可用更新，然后在操作中心中通知你这些更新已准备好进行安装  。 这是默认选项。

## <a name="remote-desktop-settings"></a>远程桌面设置

远程桌面设置的当前状态在默认服务器配置工具屏幕中显示。 你可以通过访问 **“远程桌面”** 主菜单选项并按照屏幕上的说明进行操作，来配置以下远程桌面设置。

- 为运行带网络级别身份验证的远程桌面的客户端启用远程桌面

- 为运行任何版本的远程桌面的客户端启用远程桌面

- 禁用远程桌面

## <a name="date-and-time-settings"></a>日期和时间设置

可以通过访问“日期和时间”主菜单选项来访问和更改日期和时间设置  。

## <a name="telemetry-settings"></a>遥测设置

此选项支持配置发送到 Microsoft 的数据。

## <a name="windows-activation-settings"></a>Windows 激活设置

此选项支持配置 Windows 激活。

## <a name="to-enable-remote-management"></a>启用远程管理的步骤

你可以从 **“配置远程管理”** 主菜单选项启用各种远程管理方案。

- Microsoft 管理控制台远程管理

- Windows PowerShell

- 服务器管理器  

## <a name="to-log-off-restart-or-shut-down-the-server"></a>注销、重启或关闭服务器的步骤

若要注销、重启或关闭服务器，请访问主菜单相应的菜单项。 这些选项在“Windows 安全”菜单中也可用，可通过按 CTRL+ALT+DEL 随时从任何应用程序进行访问  。  

## <a name="to-exit-to-the-command-line"></a>退出命令行的步骤
  
选择 **“退出命令行”** 选项，按 ENTER 以退出命令行。 若要返回到服务器配置工具，请键入 **Sconfig.cmd**，然后按 Enter。
