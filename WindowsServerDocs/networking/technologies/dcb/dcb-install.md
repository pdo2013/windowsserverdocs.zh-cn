---
title: 安装在 Windows Server 或客户端桥 (DCB) 的数据中心
description: 本主题提供有关如何安装 Windows Server 或 Windows 客户端中的数据中心桥的说明进行操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>安装桥 \(DCB\) Windows Server 2016 或 Windows 10 中的数据中心

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何在 Windows Server 2016 或 Windows 10 安装 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 先决条件

以下是配置和管理 DCB 的先决条件。

### <a name="install-a-compatible-operating-system"></a>安装兼容的操作系统

在以下操作系统，你可以使用本指南从 DCB 命令。

- Windows Server（半年通道）
- Windows Server 2016
- Windows 10 \(all versions\)

以下操作系统包括以前版本的 DCB 无法与兼容的 DCB 文档 Windows Server 2016 和 Windows 10 中使用的命令。

- Windows Server 2012R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬件要求

以下是硬件要求 DCB 的列表。

- 支持 DCB\ 的以太网网络 adapter\(s\) 必须安装在 Windows Server 2016 DCB 提供的计算机。
- 必须在你的网络部署 DCB\ 支持的硬件开关。


## <a name="install-dcb-in-windows-server-2016"></a>在 Windows Server 2016 中安装 DCB

你可以使用以下各部分运行 Windows Server 2016 的计算机上安装 DCB。

**管理凭据**

若要执行这些过程，你必须**管理员**。

### <a name="install-dcb-using-windows-powershell"></a>安装 DCB 使用 Windows PowerShell

你可以使用下面的过程中使用 Windows PowerShell 安装 DCB。

1. 在运行 Windows Server 2016 的计算机中，单击**开始**，然后右键单击 Windows PowerShell 图标。 将显示一个菜单。 在菜单中，单击**更多**，然后单击**以管理员身份运行**。 如果出现提示，请键入具有管理员权限在计算机的帐户有关的凭据。 使用管理员权限打开 Windows PowerShell。
2. 键入以下命令，然后再次按 ENTER。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>安装 DCB 使用服务器管理器

你可以使用下面的过程中安装 DCB 使用服务器管理器。

>[!NOTE]
>在此过程中，在执行第一步之后**开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。 如果**开始之前**页面未显示，请从第 1 步直接跳到第 3 步。

1. 在 DC1，在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。
2. 在**开始之前**，单击**下一步**。
3. 在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。
4. 在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。
5. 在**选择服务器角色**，单击**下一步**。
6. 在**选择功能**中**功能**，单击**数据中心桥**。 对话框中打开询问你是否想要添加所需的 DCB 功能。 单击**添加功能**。
7. 在**选择功能**，单击**下一步**。 
8. 7。在**确认安装选择**，单击**安装**。 **安装进度**页面显示在安装过程中的状态。 该安装成功的消息显示说明后，单击**关闭**。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>配置内核调试程序中以允许 QoS \(Optional\)

 默认情况下，内核调试程序阻止 NetQos。 无论用于安装 DCB，如果你已安装在计算机中的内核调试程序的方法，你必须配置调试程序中以允许 QoS 启用并配置通过运行以下命令。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安装 DCB

在 Windows 10 计算机上，你可以执行以下步骤。

若要执行此过程，你必须**管理员**。

### <a name="install-dcb"></a>安装 DCB

1. 单击**开始**，然后向下滚动到，单击**Windows 系统**。
2. 单击**“控制面板”**。 **“控制面板”**对话框中打开。
3. 在**“控制面板”**，单击**通过查看**，然后单击或者**大图标**或**小图标**。
4. 单击**程序和功能**。 打开程序和功能对话框。
5. 在**程序和功能**，在左侧窗格中，单击**打开或关闭 Windows 功能**。 **Windows 功能**对话框中打开。
6. 在**Windows 功能**，单击**数据中心桥**，然后单击**确定**。

![打开或关闭对话框中的 Windows 功能](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


