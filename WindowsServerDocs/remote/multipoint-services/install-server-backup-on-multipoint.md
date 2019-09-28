---
title: 在 MultiPoint 服务器上安装服务器备份
description: 指导您完成安装备份和恢复工具的步骤
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 933a24ee91fa1f5ccbe31ff4cb722a7c3eb54e4b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395117"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>在 MultiPoint 服务器上安装服务器备份
建议你考虑 MultiPoint 服务器的备份和恢复计划。
  
好的备份和恢复计划对于任何规模环境都非常重要。 Windows Server 备份是 Windows Server 2016 中的一项功能，它提供了一组向导和其他工具，使你可以为安装该功能的服务器执行基本备份和恢复任务。 您可以使用 Windows Server 备份备份整个服务器（所有卷）、选定卷、系统状态或特定的文件或文件夹，并创建可用于重新生成系统的备份。  
  
可以恢复卷、文件夹、文件、某些应用程序和系统状态。 而且，对于诸如硬盘故障之类的灾难，可以从头开始或通过使用备用硬件重建系统。 要执行此操作，必须备份完整服务器或只备份包含操作系统文件和 Windows 恢复环境的卷。 这会将完整的系统还原到旧系统中或新的硬盘上。  
  
Windows Server 备份的一项重要功能是能够计划自动运行备份。  
  
使用以下过程设置所需的备份类型。  
  
## <a name="install-backup-and-recovery-tools"></a>安装备份和恢复工具  
  
1.  从 "**开始**" 屏幕打开**服务器管理器**。  
  
2.  单击 "**添加角色和功能**" 以启动 "添加角色向导"。 查看**开始之前**，请单击 "**下一步**"。  
  
3.  选择 "**基于角色或基于功能的安装**" 选项，然后单击 "**下一步**"。  
  
4.  选择要管理的本地计算机，然后单击 "**下一步**"。  
  
    这将打开“添加功能向导”。  
  
5.  在 "**选择功能**" 页上，展开 Windows Server 备份功能，选中**Windows Server 备份**和**命令行工具**的复选框，然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > 或者，如果只是想要安装管理单元和 Wbadmin 命令行工具，请展开**Windows Server 备份功能**"，然后选中" 仅**Windows Server 备份**"复选框，确保已清除"**命令行工具**"复选框。  
  
6.  在 "**确认安装选择**" 页上，查看你的选择，然后单击 "**安装**"。  
  
    如果在安装过程中出现任何错误，则 "**安装结果**" 页将会记下错误。  
  
7.  安装成功完成后，应能够访问这些备份和恢复工具：  
  
    -   若要打开 "Windows Server 备份" 管理单元，请在 "**开始**" 屏幕上键入 "**备份**"，然后单击结果中的 " **Windows Server 备份**"。  
  
    -   启动 Wbadmin 工具并查看其命令的语法：在 "**开始**" 屏幕上，键入**命令**。 在结果中，右键单击 "**命令提示符**"，单击页面底部的 "**以管理员身份运行**"，然后在确认提示中单击 **"是"** 。 在命令提示符下，键入**wbadmin/？** 然后按 ENTER。 应会看到该工具的命令语法和说明。  
  
## <a name="configure-backups-using-windows-server-backup"></a>使用 Windows Server 备份配置备份  
  
-   按照[备份服务器](https://technet.microsoft.com/library/cc753528.aspx)中的指南进行操作。 