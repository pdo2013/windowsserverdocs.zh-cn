---
title: 安装数据中心桥接 (DCB)，在 Windows Server 或客户端
description: 本主题提供有关如何安装 Windows Server 或 Windows 客户端中的数据中心桥接的说明。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845438"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>安装数据中心桥接\(DCB\) Windows Server 2016 或 Windows 10 中

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何在 Windows Server 2016 或 Windows 10 中安装 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 的先决条件

以下是用于配置和管理 DCB 的先决条件。

### <a name="install-a-compatible-operating-system"></a>安装兼容的操作系统

以下操作系统中，可以使用本指南中的 DCB 命令。

- Windows Server（半年频道）
- Windows Server 2016
- Windows 10\(所有版本\)

以下操作系统包括 Windows Server 2016 和 Windows 10 的 DCB 文档中使用的命令与不兼容的以前版本的 DCB。

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬件要求

以下是 DCB 的硬件要求的列表。

- DCB\-支持以太网网络适配器\(s\)必须提供 Windows Server 2016 DCB 的计算机中安装。
- DCB\-必须在网络上部署支持的硬件交换机。


## <a name="install-dcb-in-windows-server-2016"></a>在 Windows Server 2016 中安装 DCB

以下各节可用于运行 Windows Server 2016 的计算机上安装 DCB。

**管理凭据**

若要执行这些过程，您必须**管理员**。

### <a name="install-dcb-using-windows-powershell"></a>安装使用 Windows PowerShell 的 DCB

以下过程可用于通过使用 Windows PowerShell 安装 DCB。

1. 在运行 Windows Server 2016 的计算机，单击**启动**，然后右键单击 Windows PowerShell 图标。 出现一个菜单。 在菜单中，单击**更多**，然后单击**以管理员身份运行**。 如果系统提示，请键入在计算机具有管理员权限的帐户的凭据。 使用管理员特权打开 Windows PowerShell。
2. 输入以下命令，然后按 ENTER。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>安装 DCB 使用服务器管理器

可以使用以下过程来通过服务器管理器安装 DCB。

>[!NOTE]
>在此过程中，执行第一步后**开始之前**如果之前已选择，则不会显示添加角色和功能向导的页**默认情况下跳过此页**时添加角色和功能向导已运行。 如果**开始之前**页未显示，从步骤 1 到步骤 3 中跳过。

1. 在 DC1 上，在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 将打开“添加角色和功能向导”。
2. 在“开始之前”中单击“下一步”。
3. 在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”**，然后单击 **“下一步”**。
4. 在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”**。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步” 。
5. 在“选择服务器角色” 中，单击“下一步” 。
6. 在中**选择的功能**，在**功能**，单击**数据中心桥接**。 这时会打开一个对话框询问你是否想要添加所需的 DCB 功能。 单击**将功能添加**。
7. 在中**选择的功能**，单击**下一步**。 
8. 7.在**确认安装选择**，单击**安装**。 **安装进度**页在安装过程中会显示状态。 该安装已成功消息会出现指出后，单击**关闭**。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>配置内核调试程序，以允许 QoS\(可选\)

 默认情况下，内核调试程序阻止 NetQos。 无论用于安装 DCB，如果必须在计算机中安装内核调试程序的方法，则必须配置调试器以允许 QoS 启用和配置通过运行以下命令。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安装 DCB

Windows 10 计算机上，可以执行以下过程。

若要执行此过程，您必须**管理员**。

### <a name="install-dcb"></a>安装 DCB

1. 单击**启动**，然后向下滚动并单击**Windows 系统**。
2. 单击 **“控制面板”**。 **控制面板**对话框随即打开。
3. 在中**Control Panel**，单击**通过查看**，然后单击**大图标**或**小图标**。
4. 单击**程序和功能**。 此时将打开程序和功能对话框。
5. 在中**程序和功能**，在左窗格中，单击**打开或关闭打开的 Windows 功能**。 **Windows 功能**对话框随即打开。
6. 在中**Windows 功能**，单击**数据中心桥接**，然后单击**确定**。

![打开 Windows 功能，或关闭对话框](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


