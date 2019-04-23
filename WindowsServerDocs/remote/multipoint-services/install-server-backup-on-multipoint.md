---
title: 在多点服务器上安装服务器备份
description: 将指导你完成安装的备份和恢复工具的步骤
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832488"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>在多点服务器上安装服务器备份
建议您考虑备份和恢复计划为多点服务器。
  
良好的备份和恢复计划非常重要的任何大型的环境。 Windows Server Backup 是一项功能提供了一组向导和其他工具，您可以在其安装的服务器执行基本备份和恢复任务的 Windows Server 2016 中。 您可以使用 Windows Server Backup 来备份整个服务器 （所有卷）、 选定的卷、 系统状态或特定文件或文件夹，并创建可用于重新生成您的系统备份。  
  
可以恢复卷、文件夹、文件、某些应用程序和系统状态。 而且，可以针对诸如硬盘故障之类的灾难，重新生成系统从零开始或使用备用的硬件。 若要执行此操作，必须具有完全服务器或只需包含操作系统文件和 Windows 恢复环境的卷的备份。 这将还原到旧系统或新的硬盘上将完整的系统。  
  
Windows Server Backup 的一个重要功能是能够计划自动执行备份。  
  
使用以下过程来设置的所需的备份类型。  
  
## <a name="install-backup-and-recovery-tools"></a>安装备份和恢复工具  
  
1.  从**启动**屏幕上，打开**服务器管理器**。  
  
2.  单击**添加角色和功能**以启动添加角色向导。 然后单击**下一步**查看之后**在开始之前**说明。  
  
3.  选择**基于角色或功能基于安装**选项，并单击**下一步**。  
  
4.  选择本地计算机的管理，并单击**下一步**。  
  
    这将打开“添加功能向导”。  
  
5.  上**选择功能**页上，展开 Windows Server Backup 功能，选择对应的复选框**Windows Server Backup**并**命令行工具**，然后单击**下一步**。  
  
    > [!NOTE]  
    > 或者，如果只是想要安装的管理单元和 Wbadmin 命令行工具，展开**Windows Server Backup 功能**，然后选择**Windows Server Backup**复选框仅 — 请确保**命令行工具**复选框已清除。  
  
6.  上**确认安装选择**页上，查看你的选择，然后单击**安装**。  
  
    如果在安装期间出现任何错误**安装结果**页会注意到这些错误。  
  
7.  安装成功完成后，您应能够访问这些备份和恢复工具：  
  
    -   若要打开 Windows Server Backup 管理单元中，在**启动**屏幕上，键入**备份**，然后单击**Windows Server Backup**结果中。  
  
    -   若要启动 Wbadmin 工具并查看其命令的语法：上**启动**屏幕上，键入**命令**。 在结果中，右键单击**命令提示符**，单击**以管理员身份运行**页上，并单击底部**是**在确认提示。 在命令提示符处，键入**wbadmin /？** 然后按 ENTER。 应会看到命令语法和工具的说明。  
  
## <a name="configure-backups-using-windows-server-backup"></a>配置使用 Windows Server Backup 备份  
  
-   遵循中的指导[Backing Up Your Server](https://technet.microsoft.com/library/cc753528.aspx)。 