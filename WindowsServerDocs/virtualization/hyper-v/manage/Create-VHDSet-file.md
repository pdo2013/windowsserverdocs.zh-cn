---
title: 创建 Hyper-v VHD 集文件
description: 在 Hyper-v 2016 上创建 VHDset 文件的步骤
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: f5c9b932cabfea8df55ba8622165bbb04b4a4113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392718"
---
# <a name="create-hyper-v-vhd-set-files"></a>创建 Hyper-v VHD 集文件
VHD 集文件是 Windows Server 2016 中来宾群集的新共享虚拟磁盘模型。 VHD 集文件支持联机调整共享虚拟磁盘大小、支持 Hyper-v 副本，并可包括在应用程序一致性检查点中。 

VHD 集文件使用新的 VHD 文件类型。Vhd. VHD 集文件以元数据的形式存储有关来宾群集中使用的组虚拟磁盘的检查点信息。

Hyper-v 处理管理检查点链和合并共享 VHD 集的各个方面。 管理软件可以像对 VHD 集文件进行联机大小调整一样运行磁盘操作。VHDX 文件。 这意味着管理软件无需知道 VHD 集文件格式。

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>从 Hyper-v 管理器创建 VHD 集文件

1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。
2.  在 "操作" 窗格中，单击 "**新建**"，然后单击 "**硬盘**"。
3.  在 "**选择磁盘格式**" 页上，选择 " **VHD 集**" 作为虚拟硬盘的格式。
4.  继续浏览向导页面，以自定义虚拟硬盘。 您可以单击 "**下一步**" 以在向导的每一页上移动，或者单击左侧窗格中的某一页的名称，直接转到该页。
5.  完成虚拟硬盘的配置后，单击 "**完成**"。

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>从 Windows PowerShell 创建 VHD 集文件

使用带有文件类型的[新 VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet。文件路径中的 VHD。 此示例创建一个名为的 VHD 集文件，其大小为 10 Gb。

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>将共享 VHDX 文件迁移到 VHD 集文件

将现有共享 VHDX 迁移到 VHD 需要使 VM 脱机。 建议使用 Windows PowerShell 执行此过程：

1. 从 VM 中删除 VHDX。 例如，运行： 
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```
  
2. 将 VHDX 转换为 VHD。 例如，运行：
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```
  
3. 将 VHD 添加到 VM。 例如，运行：
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```
  



