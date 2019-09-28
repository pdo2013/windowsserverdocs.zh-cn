---
title: 在 Windows Server 或客户端中安装数据中心桥接（DCB）
description: 本主题提供有关如何在 Windows Server 或 Windows 客户端中安装数据中心桥接的说明。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ecb6ef072dd2328a0a45d57d181dca9c2928a30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405791"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 中安装数据中心桥接 \(DCB @ no__t-1

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何在 Windows Server 2016 或 Windows 10 中安装 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 的先决条件

下面是配置和管理 DCB 的先决条件。

### <a name="install-a-compatible-operating-system"></a>安装兼容的操作系统

你可以在以下操作系统中使用本指南中的 DCB 命令。

- Windows Server（半年频道）
- Windows Server 2016
- Windows 10 @no__t 版本 @ no__t-1

以下操作系统包括 DCB 的早期版本，这些版本与在 Windows Server 2016 和 Windows 10 的 DCB 文档中使用的命令不兼容。

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬件要求

下面是 DCB 的硬件要求列表。

- DCB @ no__t-0capable 以太网网络适配器 @ no__t-1 @ no__t 必须安装在提供 Windows Server 2016 DCB 的计算机中。
- 必须在网络上部署 DCB @ no__t-0capable 硬件交换机。


## <a name="install-dcb-in-windows-server-2016"></a>在 Windows Server 2016 中安装 DCB

你可以使用以下部分在运行 Windows Server 2016 的计算机上安装 DCB。

**管理凭据**

若要执行这些过程，您必须是**管理员**的成员。

### <a name="install-dcb-using-windows-powershell"></a>使用 Windows PowerShell 安装 DCB

你可以使用以下过程通过 Windows PowerShell 安装 DCB。

1. 在运行 Windows Server 2016 的计算机上，单击 "**开始**"，然后右键单击 Windows PowerShell 图标。 此时将显示一个菜单。 在菜单中，单击 "**更多**"，然后单击 "以**管理员身份运行**"。 如果系统提示，请键入在计算机上具有管理员权限的帐户的凭据。 Windows PowerShell 将以管理员权限打开。
2. 输入以下命令，然后按 ENTER。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>使用服务器管理器安装 DCB

你可以使用以下过程通过服务器管理器来安装 DCB。

>[!NOTE]
>执行此过程中的第一步后，如果在运行 "添加角色和功能向导" 时之前选择了 "**默认跳过此页**"，则不会显示 "添加角色和功能向导" 的 "**开始之前**" 页。 如果未显示 "**开始之前**" 页，请跳至步骤3。

1. 在 DC1 上的服务器管理器中，单击 "**管理**"，然后单击 "**添加角色和功能**"。 将打开“添加角色和功能向导”。
2. 在“开始之前”中单击“下一步”。
3. 在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。
4. 在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。
5. 在“选择服务器角色”中，单击“下一步”。
6. 在 "**功能**" 的 "**选择功能**" 中，单击 "**数据中心桥接**"。 此时将打开一个对话框，询问你是否要添加 DCB 必需的功能。 单击 "**添加功能**"。
7. 在 "**选择功能**" 中，单击 "**下一步**"。 
8. 7.In**确认安装选择**，请单击 "**安装**"。 安装**进度**页面显示安装过程中的状态。 显示 "安装已成功" 消息后，单击 "**关闭**"。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>将内核调试器配置为允许 QoS \(Optional @ no__t-1

 默认情况下，内核调试器会阻止 NetQos。 无论你使用哪种方法来安装 DCB，如果在计算机上安装了一个内核调试器，则必须将调试器配置为允许通过运行以下命令来启用和配置 QoS。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安装 DCB

您可以在 Windows 10 计算机上执行以下过程。

若要执行此过程，您必须是**管理员**的成员。

### <a name="install-dcb"></a>安装 DCB

1. 单击 "**开始**"，然后向下滚动到并单击 " **Windows 系统**"。
2. 单击 **“控制面板”** 。 此时将打开 "**控制面板**" 对话框。
3. 在 "**控制面板**" 中，单击 "**查看方式**"，然后单击 "**大图标**" 或 "**小图标**"。
4. 单击 "**程序和功能**"。 此时将打开 "程序和功能" 对话框。
5. 在 "**程序和功能**" 的左窗格中，单击 "**打开或关闭 Windows 功能**"。 此时将打开 " **Windows 功能**" 对话框。
6. 在**Windows 功能**中，单击 "**数据中心桥接**"，然后单击 **"确定"** 。

!["打开或关闭 Windows 功能" 对话框](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


