---
title: 在 Windows Server 上使用磁盘清理
description: 了解如何使用命令行选项来配置磁盘清理工具 (Cleanmgr.exe) 来自动清除某些文件。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fbec7cd2b8312f03998cfb27b739d0866d3a47c5
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407664"
---
# <a name="using-disk-cleanup-on-windows-server"></a>在 Windows Server 上使用磁盘清理

> 适用于：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

磁盘清理工具清除不必要的文件，在 Windows Server 环境中。 默认情况下，在 Windows Server 2019 和 Windows Server 2016 上提供了此工具，但可能需要执行一些手动步骤，以便更早版本的 Windows Server 上启用它。

若要启动磁盘清理工具，请运行 Cleanmgr.exe 命令，或选择**启动**，选择**Windows 管理工具**，然后选择**磁盘清理**。

此外可以通过运行磁盘清理[cleanmgr Windows 命令](../../administration/windows-commands/cleanmgr.md)并使用命令行选项来指定磁盘清理清理某些文件。

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>通过安装桌面体验的 Windows Server 的早期版本上启用磁盘清理

请按照这些步骤使用添加角色和功能向导在运行 Windows Server 2012 R2 的服务器上安装桌面体验或更早版本，这同时会安装磁盘清理。

1. 如果服务管理器已经打开，则继续执行下一步。 如果服务器管理器尚未打开，请执行以下任一操作打开它。

   - 在 Windows 桌面上，启动服务器管理器，方法是单击 Windows 任务栏中的“服务器管理器”  。

   - 转到**启动**，然后选择服务器管理器磁贴。

1. 上**管理**菜单中，选择添加**角色和功能**。

1. 上**在开始之前**页上，验证目标服务器和网络环境已做好准备为你想要安装的功能。 选择“**下一步**”。

1. 上**选择安装类型**页上，选择**基于角色或基于功能的安装**将一台服务器上安装所有的部分功能。 选择“**下一步**”。

1. 在“选择目标服务器”  页面上，从服务器池中选择一台服务器，或选择脱机 VHD。 选择“**下一步**”。

1. 上**选择服务器角色**页上，选择**下一步**。

1. 上**选择的功能**页上，选择**用户界面和基础结构**，然后选择**桌面体验**。

1. 在中**添加所需的桌面体验的功能？** ，选择**添加功能**。

1. 继续进行安装，然后重新启动系统。

1. 确认**磁盘清理**选项按钮将出现在属性对话框中。

   ![磁盘使用磁盘清理选项属性对话框](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>将磁盘清理手动添加到 Windows Server 的早期版本

磁盘清理工具 (cleanmgr.exe) 不存在 Windows Server 2012 R2 或更低版本除非您有安装了桌面体验功能。

若要使用 cleanmgr.exe，安装桌面体验，如前所述，或复制服务器、 cleanmgr.exe 和 cleanmgr.exe.mui 已有的两个文件。 使用下表以找到你的操作系统的文件。

| 操作系统  | 体系结构  | 文件位置  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 位 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 位 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

找到 cleanmgr.exe 并将文件移动到 **%systemroot%\System32**。

找到 cleanmgr.exe.mui 并将文件移到 **%systemroot%\System32\en-US**。

现在可以通过从命令提示符处，运行 Cleanmgr.exe 或通过单击来启动磁盘清理工具**启动**并键入**Cleanmgr**在搜索栏。

若要让磁盘的属性对话框上显示的磁盘清理按钮，还需要安装桌面体验功能。

## <a name="additional-references"></a>其他参考

[释放在 Windows 10 中的驱动器空间](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
