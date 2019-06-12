---
title: 创建 HYPER-V VHD 设置文件
description: 若要创建的 hyper-v 2016 上的 VHDset 文件的步骤
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: a5a6f79d362b9058ca29d979457a1dcdfc0c9f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445688"
---
# <a name="create-hyper-v-vhd-set-files"></a>创建 HYPER-V VHD 设置文件
设置 VHD 文件是针对 Windows Server 2016 中来宾群集的新共享的虚拟磁盘模型。 VHD 设置文件支持的共享的虚拟磁盘的联机大小调整、 支持的 HYPER-V 副本和可以包括在应用程序一致的检查点。 

设置 VHD 文件使用新的 VHD 文件类型。VHD。 设置 VHD 文件存储有关组的虚拟磁盘来宾群集，在元数据的窗体中使用的检查点信息。

HYPER-V 处理管理检查点链的所有方面，并合并共享的 VHD 设置。 管理软件可以运行磁盘操作，例如联机调整大小的 VHD 设置文件的方式相同。VHDX 文件。 这意味着管理软件，无需了解的有关 VHD 设置文件格式。

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>从 Hyper-v 管理器创建的 VHD 设置文件

1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。
2.  在操作窗格中，单击**新建**，然后单击**硬盘**。
3.  上**选择磁盘格式**页上，选择**VHD 设置**作为虚拟硬盘格式。
4.  继续完成向导后，若要自定义虚拟硬盘的页。 可以单击**下一步**浏览每一页的向导中，也可以单击页面直接移至该页面的左窗格中的名称。
5.  配置虚拟硬盘之后，请单击**完成**。

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>从 Windows PowerShell 中创建的 VHD 设置文件

使用[NEW-VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet，与文件类型。文件路径中的 VHD。 此示例创建名为 base.vhds 大小为 10 千兆字节为单位的 VHD 设置文件。

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>将共享的 VHDX 文件迁移到 VHD 设置文件

将现有的共享的 VHDX 迁移到 VHD 需要使 VM 脱机。 这是使用 Windows PowerShell 的建议的过程：

1. 从 VM 删除的 VHDX。 例如，运行： 
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
  



